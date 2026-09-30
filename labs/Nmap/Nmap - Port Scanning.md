
tags: #nmap #pentesting #networking #recon

---

## ¿Qué es?

Herramienta estándar de la industria para **port scanning y enumeración**. Antes de atacar un objetivo necesitás saber qué puertos/servicios tiene abiertos.

Cada equipo tiene **65535 puertos disponibles**, de los cuales **1024 son "well-known"** (ej: 80 HTTP, 443 HTTPS, 139/445 SMB).

---

## Switches principales

| Switch           | Para qué sirve                                                |
| ---------------- | ------------------------------------------------------------- |
| `-sS`            | SYN scan (stealth, requiere sudo)                             |
| `-sT`            | TCP Connect scan (no requiere sudo)                           |
| `-sU`            | UDP scan                                                      |
| `-sN`            | NULL scan                                                     |
| `-sF`            | FIN scan                                                      |
| `-sX`            | Xmas scan                                                     |
| `-sn`            | Ping sweep (sin escanear puertos)                             |
| `-O`             | Detectar OS del objetivo                                      |
| `-sV`            | Detectar versión de servicios                                 |
| `-A`             | Modo agresivo (OS + versión + traceroute + scripts)           |
| `-v` / `-vv`     | Verbosidad (nivel 1 / nivel 2)                                |
| `-p 80`          | Escanear puerto específico                                    |
| `-p 1000-1500`   | Rango de puertos                                              |
| `-p-`            | Todos los puertos                                             |
| `-T5`            | Timing máximo (0-5, más rápido = más ruidoso)                 |
| `-Pn`            | No hacer ping antes de escanear (útil si ICMP está bloqueado) |
| `--top-ports 20` | Los N puertos más comunes (útil con UDP)                      |
| -h               | ayuda                                                         |


---

## Output( Guardar salida )

|Switch|Formato|
|---|---|
|`-oN`|Normal|
|`-oG`|Grepable|
|`-oA`|Los tres formatos a la vez|

---

## Tipos de escaneo

### TCP Connect (`-sT`)

Completa el three-way handshake. No requiere sudo. Es el más detectable.

- Puerto abierto → recibe SYN/ACK
- Puerto cerrado → recibe RST
- Puerto filtrado → no recibe nada (firewall droppea)

### SYN / Half-Open (`-sS`) ⭐

Manda SYN, si recibe SYN/ACK manda RST (no completa el handshake). Requiere sudo.

- Más rápido y sigiloso que Connect
- No queda logueado en la mayoría de servicios
- Es el default cuando se corre con sudo

### UDP (`-sU`)

Sin handshake, solo manda paquetes y espera.

- Sin respuesta → `open|filtered`
- Recibe ICMP "port unreachable" → cerrado
- Muy lento (~20 min para los primeros 1000 puertos)
- Usar con `--top-ports 20` para acelerar

### NULL / FIN / Xmas (`-sN / -sF / -sX`)

Pensados para **evasión de firewalls**. No usan el flag SYN, por lo que bypasean firewalls que bloquean conexiones nuevas.

- NULL: sin flags
- FIN: solo flag FIN
- Xmas: flags PSH + URG + FIN (se ve como un árbol navideño en Wireshark)
- Respuesta cerrado → RST | Abierto → sin respuesta → resultado: `open|filtered`
- ⚠️ Windows y Cisco responden RST en todos los puertos (resultados inútiles)

### Ping Sweep (`-sn`)

Detecta qué IPs están activas en una red sin escanear puertos.

```bash
nmap -sn 192.168.0.0/24
nmap -sn 172.16.0.0/16
```

---

## NSE - Nmap Scripting Engine

Scripts escritos en **Lua**. Se guardan en `/usr/share/nmap/scripts/`.

|Categoría|Descripción|
|---|---|
|`safe`|No afecta al objetivo|
|`intrusive`|Puede afectar al objetivo ⚠️|
|`vuln`|Busca vulnerabilidades|
|`exploit`|Intenta explotar|
|`auth`|Intenta bypassear autenticación|
|`brute`|Fuerza bruta de credenciales|
|`discovery`|Interroga servicios para más info|

```bash
# Correr categoría
nmap --script=vuln <target>
nmap --script=safe <target>

# Script específico
nmap --script=http-fileupload-exploiter <target>

# Múltiples scripts
nmap --script=smb-enum-users,smb-enum-shares <target>

# Script con argumentos
nmap -p 80 --script http-put --script-args http-put.url='/dav/shell.php',http-put.file='./shell.php' <target>

# Buscar scripts localmente
grep "ftp" /usr/share/nmap/scripts/script.db
ls -l /usr/share/nmap/scripts/*smb*

# Ayuda de un script
nmap --script-help <script-name>
```

---

## Firewall Evasion

|Switch|Para qué sirve|
|---|---|
|`-Pn`|No pinguear antes (bypassea bloqueo ICMP, típico en Windows)|
|`-f`|Fragmenta paquetes|
|`--mtu <n>`|Tamaño máximo de paquete (múltiplo de 8)|
|`--scan-delay <ms>`|Delay entre paquetes (evita triggers por tiempo)|
|`--badsum`|Checksum inválido para detectar presencia de firewall/IDS|
|`--data-length`|Añade datos aleatorios al final de los paquetes|

---

## Ejemplos rápidos

```bash
# Escaneo básico con detección de servicios
nmap -sV -vv -oA output <target>

# SYN scan en todos los puertos
sudo nmap -sS -p- <target>

# UDP top 20 puertos
nmap -sU --top-ports 20 <target>

# Xmas scan primeros 999 puertos
nmap -sX -p 1-999 <target>

# Sin ping + agresivo
nmap -Pn -A <target>

# Ping sweep /24
nmap -sn 192.168.1.0/24
```

---

### NULL / FIN / Xmas (`-sN / -sF / -sX`)

**¿Para qué sirven?** Para evadir firewalls que bloquean paquetes con flag SYN (el flag que inicia conexiones nuevas). Al no tener SYN, el firewall los deja pasar.

|Scan|Flags que manda|
|---|---|
|NULL (`-sN`)|Ninguno|
|FIN (`-sF`)|FIN|
|Xmas (`-sX`)|PSH + URG + FIN (parece un árbol navideño en Wireshark)|

**¿Cómo interpretan la respuesta?** Se basan en lo que el **RFC 793** (el documento oficial que define cómo debe funcionar TCP) establece:

> _"Si llega un paquete raro a un puerto cerrado → respondé RST. Si el puerto está abierto → ignoralo."_

```
Nmap manda paquete raro →  sin respuesta  → open|filtered (abierto o firewall)
Nmap manda paquete raro →  RST            → closed
```

El problema es que silencio puede ser puerto abierto **o** firewall droppeando. Por eso nunca podés estar seguro y el resultado es `open|filtered`.

**⚠️ El problema con Windows y Cisco** es que no respetan el RFC — responden RST siempre, sin importar si el puerto está abierto o cerrado. Resultado: todo aparece como `closed` aunque haya servicios corriendo. Completamente inútil contra estos sistemas.

**¿Cuándo usar cada tipo de scan?**

|Situación|Scan recomendado|
|---|---|
|Target Linux, sin firewall|`-sS` SYN (rápido y sigiloso)|
|Target Linux, firewall bloquea SYN|`-sN / -sF / -sX` (bypasean el firewall)|
|Target Windows, sin firewall|`-sS` o `-sT` (funcionan bien)|
|Target Windows, con firewall|`-sS` + `-f` (fragmentación) o `--scan-delay`|
|No tenés sudo|`-sT` Connect (único que no lo requiere)|
|ICMP bloqueado (host parece muerto)|Agregar `-Pn` a cualquiera de los anteriores|
