
### Objetivos y temas que se cubrirán en la sección

- **Objetivos oficiales CompTIA:** Cobertura de los dominios 3 y 4 (Objetivos 3.2: _Security Architecture Principles_ y 4.5: _Enterprise Security Capabilities_).
    
      
    
- **Puertos y Protocolos:** Repaso de puertos/protocolos fundamentales (continuación de A+/Network+).
    
      
    
- **Firewalls:** Tipos (WAF, UTM, NGFW), diferencias de capa OSI (Layer 4 vs. Layer 7), hardware vs. software y configuración (ACLs, reglas, Screened Subnets).
    
      
    
- **IDS / IPS:** Sistemas de detección y prevención de intrusiones, identificación de amenazas y firmas.
    
      
    
- **Dispositivos y Seguridad de Red:** Proxies, load balancers, Port Security (filtrado MAC), 802.1X y EAP.
    
      
    
- **Comunicaciones Seguras:** VPNs, IPsec tunnels, TLS.
    
      
    
- **SD-WAN & SASE:**
    
      
    - **SD-WAN:** Enrutamiento inteligente y optimización dinámica de enlaces WAN (MPLS, celular, banda ancha) con gestión centralizada.
        
          
        
    - **SASE (Secure Access Service Edge):** Marco nativo en la nube que integra capacidades de red (SD-WAN) y seguridad (FWaaS, SWG, ZTNA, CASB).
        
          
        
- **Consideraciones de Infraestructura:** Ubicación de dispositivos, zonas de seguridad, superficie de ataque, modos de falla (**Fail-Open** vs. **Fail-Closed**), monitoreo activo/pasivo e inline/TAP.
    
      
    

### Para el examen

- **Términos clave a tener en el radar:** **SD-WAN**, **SASE**, **Fail-Open / Fail-Closed**, **WAF**, **802.1X**, **Screened Subnet** (anteriormente conocida como DMZ).
    
      
    
- **Lo que van a evaluar en los siguientes videos:** Diferenciar dispositivos de Capa 4 vs. Capa 7, saber cuándo aplicar SASE frente a arquitecturas tradicionales, y elegir los controles de infraestructura adecuados según el escenario planteado.
## Puertos y Protocolos

### Conceptos base

**Puerto**: punto final de comunicación lógica en un dispositivo. Rango: 0–65,535.

**Inbound vs Outbound**:

- **Inbound port**: abierto y "escuchando" conexiones entrantes (ej: servidor web con puerto 443 abierto esperando visitantes)
- **Outbound port**: abierto temporalmente por un cliente cuando inicia una conexión — usa un número alto y aleatorio, se cierra al terminar la sesión

**Ejemplo SSH end-to-end**: servidor escucha en puerto 22 (inbound) con IP pública. Laptop cliente (con IP privada vía NAT) abre un puerto outbound aleatorio (ej: 51233), envía request al puerto 22 del servidor. El servidor responde a la IP/puerto outbound del cliente. Se establece la sesión; al cerrarla, el cliente cierra su puerto outbound, el servidor mantiene el 22 abierto para el próximo usuario.

### Tres rangos de puertos

|Rango|Nombre|Descripción|
|---|---|---|
|**0–1023**|Well-known ports|Asignados por **IANA** para protocolos de uso común (ej: 443=HTTPS, 23=Telnet)|
|**1024–49,151**|Registered ports|Vendors registran sus protocolos propietarios ante IANA (ej: 1433=MS SQL, 3389=RDP)|
|**49,152–65,535**|Dynamic/Private ports|Uso libre sin registro — típicamente elegidos por clientes para conexiones outbound temporales (gaming, chat, IM)|

### Tabla de puertos clave a memorizar (número, protocolo, TCP/UDP, uso)

| Puerto    | Protocolo        | TCP/UDP       | Uso                                                                    |
| --------- | ---------------- | ------------- | ---------------------------------------------------------------------- |
| 21        | FTP              | TCP           | Transferencia de archivos                                              |
| 22        | SSH / SCP / SFTP | TCP           | Acceso remoto seguro / copia segura / transferencia segura de archivos |
| 23        | Telnet           | TCP           | Acceso remoto **no encriptado** (inseguro — reemplazar por SSH)        |
| 25        | SMTP             | TCP           | Envío de email                                                         |
| 53        | DNS              | TCP/UDP       | Traducción de dominios a IP                                            |
| 69        | TFTP             | UDP           | Transferencia simplificada de archivos                                 |
| 80        | HTTP             | TCP           | Web no encriptada                                                      |
| 88        | Kerberos         | UDP           | Autenticación de red                                                   |
| 110       | POP3             | TCP           | Recuperar email                                                        |
| 119       | NNTP             | TCP           | Acceso a newsgroups                                                    |
| 135       | RPC              | TCP/UDP       | Comunicación entre procesos (usado en file sharing de Windows)         |
| 137-139   | NetBIOS          | UDP/TCP       | Compartir nombres de red/archivos/impresoras en Windows                |
| 143       | IMAP             | TCP           | Acceso a email en servidor                                             |
| 161       | SNMP             | UDP           | Gestión de dispositivos de red                                         |
| 162       | SNMP Traps       | UDP           | Envío de mensajes SNMP trap                                            |
| 389       | LDAP             | TCP           | Servicios de directorio                                                |
| 443       | HTTPS            | TCP           | Web segura (encriptada)                                                |
| 445       | SMB              | TCP           | Compartir archivos/impresoras                                          |
| 465 / 587 | SMTPS            | TCP (SSL/TLS) | Email seguro                                                           |
| 514       | Syslog           | UDP           | Envío de logs                                                          |
| 636       | LDAPS            | TCP (SSL/TLS) | LDAP seguro                                                            |
| 993       | IMAPS            | TCP (SSL/TLS) | IMAP seguro                                                            |
| 995       | POP3S            | TCP (SSL/TLS) | POP3 seguro                                                            |
| 1433      | MS SQL           | TCP           | Comunicación con SQL Server                                            |
| 1645/1646 | RADIUS (TCP)     | TCP           | AAA — autenticación/autorización/accounting                            |
| 1812/1813 | RADIUS (UDP)     | UDP           | AAA — versión UDP                                                      |
| 3389      | RDP              | TCP           | Acceso remoto a escritorio (Windows)                                   |
| 6514      | Syslog TLS       | TCP           | Syslog encriptado                                                      |

### Para el examen

- **El examen NO pregunta directo** ("¿qué puerto usa SSH?") — pregunta indirecto: por qué una conexión falla, qué puerto abrir/cerrar en un firewall, cómo asegurar un servicio. Necesitás saber los 4 datos de memoria para cada puerto: **número, protocolo, TCP/UDP, y para qué sirve**.
- **Telnet (23) → reemplazar por SSH (22)**: patrón de pregunta clásico — "¿cómo asegurar el acceso remoto de este escenario?" → cerrar Telnet, abrir SSH.
- **Pares seguro/inseguro** son muy preguntados: HTTP(80)/HTTPS(443), POP3(110)/POP3S(995), IMAP(143)/IMAPS(993), LDAP(389)/LDAPS(636), SMTP(25)/SMTPS(465,587), Syslog(514)/Syslog TLS(6514).
- **RADIUS tiene 2 pares de puertos** (1645/1646 legacy TCP, 1812/1813 estándar UDP) — memorizar que existen ambos, el estándar moderno es UDP 1812/1813.
- Recomendación del instructor: hacer flashcards (protocolo de un lado, puerto+TCP/UDP del otro) — dado el volumen, conviene repasar esta tabla varias veces antes del examen real.

## Firewalls

**Firewall** = dispositivo/software de seguridad que monitorea y controla tráfico entrante/saliente según reglas de seguridad predefinidas.

### Screened Subnet (dual-homed host)

Al colocar un firewall delante de un segmento de red, se crea una **screened subnet** — actúa como barrera entre redes externas no confiables e internas confiables. Un dual-homed host (con packet-filtering firewall u otros mecanismos) filtra el tráfico entre ambas redes, dejando pasar solo lo legítimo/autorizado.

### Tipos de Firewall (por profundidad de inspección — trade-off velocidad vs seguridad)

|Tipo|Capa OSI|Cómo inspecciona|Rendimiento|
|---|---|---|---|
|**Packet-filtering firewall**|Capa 4 (Transport)|Solo header del paquete — IP y puerto. Como una ACL de router|Más rápido, menos seguro. No previene IP spoofing, fragmentation attacks, ni ataques al TCP handshake|
|**Stateful firewall**|—|Rastrea el **estado de las conexiones** — recuerda requests salientes para permitir el tráfico de retorno correspondiente|Balance entre velocidad y seguridad|
|**Proxy firewall**|Circuit-level: Capa 5 (Session) / Application-level: Capa 7 (Application)|Hace conexiones **en nombre de** los endpoints. Circuit-level (ej: SOCKS) = capa sesión; Application-level = deep packet inspection específica por tipo de app (HTTP ≠ FTP)|Application-level = más lento (deep inspection); mejor ubicarlo cerca del servidor que protege|
|**Kernel proxy firewall**|Todas las capas|5ta generación — inspección completa en cada capa, pero con **impacto mínimo en performance**|Rápido Y seguro — mejor ubicado cerca del sistema que protege|

### Evoluciones modernas

**NGFW (Next-Generation Firewall)** Firewalls **application-aware** — distinguen tráfico por aplicación específica, no solo puerto/protocolo. Hacen single-pass deep packet inspection + IPS basado en firmas cuando están in-line. Rápidos, buen impacto en performance, visibilidad completa, firmas personalizables. Contras: complejos de gestionar, riesgo de **vendor lock-in** al integrarse con otros productos del mismo proveedor.

**UTM (Unified Threat Management)** Combina múltiples funciones de seguridad en **un solo dispositivo**: firewall, IPS, antivirus/antispam gateway, VPN concentrator, content filtering, load balancing, DLP.

- Ventajas: menos dispositivos que gestionar, menor costo, menor mantenimiento/consumo, instalación más simple
- Desventaja crítica: **single point of failure** — si falla el UTM, se pierde TODA la security stack de una vez
- Usa motores separados por función (menos eficiente que NGFW, que usa un motor unificado más eficiente)
- Se ubica típicamente como gateway firewall entre la LAN e internet

**WAF (Web Application Firewall)** Enfocado específicamente en tráfico **HTTP/HTTPS** — previene ataques comunes contra apps web: **XSS, SQL injection**.

- **Inline**: entre el firewall de red y los web servers — puede bloquear ataques en vivo, pero ralentiza tráfico y puede generar falsos positivos (bloquear tráfico legítimo)
- **Out-of-band**: recibe copia del tráfico vía port mirror/SPAN de un switch — menos intrusivo pero **no puede bloquear** tráfico en vivo, funciona más como un IDS (solo detecta/alerta)

### Para el examen

- **Layer 4 vs Layer 7 firewall**: Layer 4 (packet-filtering) = solo IP/puerto, sin inspeccionar payload; Layer 7 (application/proxy) = inspecciona contenido/características del payload. Trampa clásica.
- **Packet-filtering firewall NO previene**: IP spoofing, fragmentation attacks, TCP handshake attacks — memorizar esta limitación específica, muy citada en el examen.
- **Stateful vs Stateless (packet-filtering)**: stateful recuerda el contexto de la conexión (permite tráfico de retorno esperado); stateless solo mira cada paquete de forma aislada.
- **UTM = single point of failure** es el dato más importante de esta lección — trade-off central entre UTM (todo en uno, pero SPOF) vs NGFW (más rápido, motor único, pero más complejo/vendor lock-in).
- **WAF inline vs out-of-band**: inline puede bloquear pero ralentiza + falsos positivos; out-of-band solo detecta (como un IDS), no bloquea. Trampa clásica de "¿puede este WAF prevenir el ataque en tiempo real?" según su modo de despliegue.
- **XSS y SQL injection** asociados específicamente a WAF — refuerza conexión con conceptos de application security vistos/por ver en el curso.
- **Kernel proxy firewall (5th gen)** es menos citado que los otros pero puede aparecer como "el firewall con mejor balance seguridad/performance".

### Configuring Firewalls — ACLs

**ACL (Access Control List)** = conjunto de reglas en firewall/router/dispositivo de red que permite o deniega tráfico por interfaz. Compuesta por: tipo de tráfico, IP origen, IP destino, y acción (permit/deny). Ej: "permitir TCP desde 192.168.0.0 a cualquier destino por puerto 22".
- **Orden de reglas en ACL (specific → general) + primera coincidencia detiene evaluación**: concepto central, muy propenso a pregunta de "¿por qué esta regla no se está aplicando?" (porque una regla anterior más genérica ya capturó el tráfico).
- **Implicit deny**: memorizar que es un comportamiento por defecto en muchos dispositivos, y que agregar un **explicit deny-all** al final es buena práctica cuando no está garantizado.
- **Port forwarding / port triggering** = mecanismo para permitir tráfico entrante específico hacia un host interno — término técnico que puede aparecer en preguntas de escenario sobre exponer un servicio interno (ej: servidor FTP/web) a internet.
- **Stealth mode (Mac)** = no responder a pings — concepto de "hacer el sistema invisible a reconnaissance" (conecta con la lección de reconnaissance en pentesting).
- Esta lección es más práctica/demostrativa que conceptual — lo importante para el examen es entender el **flujo lógico de una ACL** (orden, implicit deny, logging) más que memorizar los pasos exactos de las UIs mostradas.

## IDS vs IPS — resumen para Security+

### Diferencia clave

- **IDS** (Intrusion Detection System): detecta, registra, alerta. No actúa.
- **IPS** (Intrusion Prevention System): detecta, registra, alerta **y actúa** (bloquea tráfico, mata la app, desconecta el dispositivo).

### Tipos por ubicación

|Tipo|Dónde vive|Qué mira|
|---|---|---|
|**NIDS** (Network)|dispositivo standalone, conectado a un **SPAN port** / mirror port del switch troncal|tráfico de red completo — port scans, payloads sospechosos, IPs/puertos raros|
|**HIDS** (Host)|software instalado en un server/endpoint puntual|tráfico hacia/desde ese host + procesos y archivos sospechosos|
|**WIDS** (Wireless)|red inalámbrica|ataques DoS wireless: auth flooding, disassociation, deauthentication|

Para versión IPS de cada uno (NIPS/HIPS/WIPS): mismo scope, pero con capacidad de reaccionar.

- **NIPS**: se ubica inline, cerca del borde de la red, justo detrás del firewall — así todo el tráfico pasa _a través_ de él y puede bloquear.
- **NIDS**: pasivo, va en el mirror port (copia del tráfico, no inline).

### Métodos de detección

- **Signature-based**: matchea contra firmas conocidas de ataques.
    - **Pattern-matching**: secuencia específica de pasos de un ataque → más común en NIDS/WIDS.
    - **Stateful-matching**: compara contra un baseline conocido del sistema, reporta cambios → más común en HIDS.
    - Débil contra **zero-days** (nunca vistos antes); necesita updates constantes.
- **Anomaly-based** (behavior-based): compara contra tráfico normal (baseline). Detecta desviaciones → más falsos positivos. 5 subtipos: statistical, protocol, traffic, rule/heuristic, application-based.

### Para el examen

- Trampa clásica: **IDS = detect only** / **IPS = detect + react**. Si la pregunta menciona "blocks", "denies", "takes action" → es IPS.
- Ubicación física es examinable: **NIDS en SPAN/mirror port** (pasivo) vs **NIPS inline detrás del firewall**.
- Pattern-matching → NIDS/WIDS | Stateful-matching → HIDS (memorizar el emparejamiento).
- Signature-based no detecta zero-day; anomaly-based sí puede, pero con más false positives — este trade-off es pregunta típica.
- WIDS/WIPS: asociarlo específicamente con deauth/disassociation attacks, no con ataques de red genéricos.
## Network Devices — resumen para Security+

### Concepto general

**Network appliance**: hardware dedicado con software preinstalado para dar un servicio específico (seguridad, storage, funciones de server). Cuatro tipos vistos: **load balancers**, **proxy servers**, **sensors**, **jump servers**.

### Load Balancers

- Distribuyen tráfico entre varios servidores → evita que uno solo se sature, mejora tiempo de respuesta.
- Hacen **health checks** continuos; si un server cae, redirige el tráfico a los que siguen operativos → reduce downtime (planeado o no).
- **ADC** (Application Delivery Controller) = versión avanzada del load balancer. Suma: **SSL termination**, **HTTP compression**, **content caching**.
- En cloud: distribuyen carga entre instancias o incluso entre datacenters.

### Proxy Servers

- Intermediario entre cliente y server. Funciones: **caching**, **request filtering**, **login management**.
- Cachea contenido → responses más rápidas, menos uso de ancho de banda.
- Seguridad: oculta los endpoints reales de la red → protege contra **DDoS** y ataques directos (el atacante le pega al proxy, no al server real).
- También hace: auth de usuarios, cifrado/tunneling de datos sensibles, y enrutamiento para compliance de **data sovereignty** (evitar que el tráfico cruce fronteras no deseadas).

### Sensors

- Monitorean tráfico y flujo de datos para detectar actividad inusual, brechas de seguridad, problemas de performance.
- Dos funciones principales:
    - **Performance monitoring**: responsiveness/disponibilidad, detecta picos anormales de tráfico.
    - **Security**: primera línea de defensa, alimentan a los sistemas IDS/IPS detectando patrones/comportamientos sospechosos (ej: spike repentino desde una IP desconocida → posible DDoS).

### Jump Servers (Jump Box)

- Gateway dedicado que usan sysadmins para acceder de forma segura a dispositivos en **distintas zonas de seguridad**.
- Centraliza el acceso administrativo → reduce superficie de ataque (todo pasa por un único punto controlado y monitoreado).
- Ventaja clave: **logging centralizado** — quién accedió a qué y cuándo → facilita auditoría e incident response.
- Suele alojar las herramientas/scripts que los admins necesitan para sus tareas rutinarias.

### Para el examen

- Si la pregunta describe "distributes traffic across multiple servers" → **load balancer**. Si menciona SSL termination/caching además de distribución → es un **ADC**, no un load balancer básico.
- Proxy server ≠ solo cache: el examen suele testear la parte de **anonimizar el endpoint real** como defensa anti-DDoS.
- Sensor = insumo de datos para IDS/IPS, no confundir con el IDS/IPS en sí.
- Jump server: la palabra clave para el examen es **"single point of entry"** entre zonas de seguridad distintas, usado para admin access — asociarlo con auditoría/logging.
## Port Security & 802.1X — resumen para Security+

### Port Security (switch level)

- Función de switches de **capa 2** que restringe qué dispositivos se conectan a un puerto según su **MAC address**.
- Los switches usan bridging transparente + tabla **CAM** (Content Addressable Memory) para mapear MAC ↔ puerto. Cada puerto = su propio dominio de colisión → full duplex, y el switch solo reenvía tráfico al puerto correspondiente (a diferencia de un hub, que lo difunde a todos).
- **Ataque: MAC flooding** — inunda la tabla CAM con MACs random hasta desbordarla → el switch "falla abierto" y empieza a comportarse como un **hub**, broadcasteando todo a todos los puertos (pierde la ventaja de seguridad).
- **Defensa: Port Security / MAC filtering** — asocia una MAC específica a una interfaz específica. Cualquier otra MAC es rechazada.
- **Sticky MAC** (persistent MAC learning): en vez de configurar manualmente cada MAC, el puerto aprende dinámicamente la primera MAC que se conecta y la autoriza.
- Debilidad: **MAC spoofing** — el atacante clona una MAC ya aprobada y bypassea el filtro. Por eso port security sola no alcanza → se combina con 802.1X.

### 802.1X

- Framework estandarizado (IEEE) para **autenticación basada en puertos**, cableado e inalámbrico. No es un protocolo de auth en sí — necesita RADIUS o TACACS+ para hacer la autenticación real.
- **3 roles**:
    - **Supplicant**: el dispositivo/usuario que pide acceso.
    - **Authenticator**: el dispositivo intermedio (switch, AP, VPN concentrator).
    - **Authentication server**: normalmente RADIUS o TACACS+.

||RADIUS|TACACS+|
|---|---|---|
|Plataforma|multiplataforma|propietario Cisco|
|Transporte|—|TCP (más lento, pero más seguro)|
|Separa AAA|no|sí (authentication/authorization/accounting independientes)|
|Soporte protocolos|no soporta remote access, NetBIOS frame protocol, X.25 PAD|soporta todos|

- Regla práctica: todo Cisco → TACACS+; infraestructura mixta → RADIUS.

### EAP (Extensible Authentication Protocol)

Framework encapsulado dentro de 802.1X, con varias variantes:

|Variante|Certificado cliente|Certificado server|Mutual auth|Notas|
|---|---|---|---|---|
|**EAP-MD5**|no (password)|no|❌ unidireccional|usa challenge-handshake, vulnerable, requiere password fuerte|
|**EAP-TLS**|sí|sí|✅|inmune a ataques basados en password (PKI en ambos lados) — el más seguro|
|**EAP-TTLS**|no (password)|sí|parcial|más seguro que MD5, menos que TLS|
|**EAP-FAST**|no (PAC)|—|✅|usa Protected Access Credential en vez de certificado|
|**PEAP**|no|sí|✅|valida contra Active Directory|
|**LEAP**|—|—|—|propietario Cisco, solo en dispositivos Cisco|

### Para el examen

- Trampa clásica: MAC flooding → switch se degrada a comportarse como **hub**. Memorizar esa palabra exacta.
- **Sticky MAC** = aprendizaje automático de la primera MAC conectada, no confundir con configuración manual.
- **EAP-TLS = el más seguro** (certificados en ambos lados, mutual auth) — típica pregunta de "cuál elegir para máxima seguridad".
- **LEAP y TACACS+** = ambos propietarios de Cisco (patrón para memorizar juntos).
- RADIUS vs TACACS+: memorizar la tabla de diferencias, es clásico de examen.
- Port security + 802.1X + EAP = defensa en profundidad; una pregunta puede pedir identificar la capa que falta si solo mencionan port security.
### Qué es 802.1X y cómo funciona con el servidor?

**es poner un servidor de control detrás del switch que te pide credenciales antes de habilitar el puerto**.

Cuando enchufas un cable de red, el switch **bloquea todo el tráfico** hacia internet o la red local. Solo deja pasar un mensaje hacia el **servidor de autenticación** (servidor RADIUS/TACACS+).

1. Te conectas.
    
2. El switch te frena y le pregunta al servidor: _"¿Este usuario/dispositivo puede entrar?"_.
    
3. Vos ingresas tu usuario/contraseña (o tu certificado).
    
4. El servidor verifica y le responde al switch: _"Sí, abrele el puerto"_.
    

### 2. ¿Qué es un Certificado Digital?

Un **certificado** es como un **DNI digital infalsificable**.

- En lugar de usar usuario y contraseña (que se pueden robar, adivinar o compartir), se instala un archivo digital en la máquina.
- Cuando te conectas, el servidor y tu computadora se muestran sus "DNIs digitales" para demostrar quiénes son de forma matemática. Nadie tiene que escribir ninguna clave.
    
### 3. ¿Qué es EAP?

**EAP (Extensible Authentication Protocol)** es el **lenguaje o formato de los mensajes** que se envían entre tu computadora y el servidor para hacer el proceso de autenticación.

Como 802.1X es solo la "puerta de entrada", necesita a **EAP** para llevar los datos del login de un lado a otro de forma segura. Dependiendo del tipo de EAP que elijas (EAP-TLS, PEAP, etc.), ese diálogo usará contraseñas, certificados o ambos.

**RADIUS** está pensado para dar acceso a **usuarios finales** (Wi-Fi, VPN, puertos de red), mientras que **TACACS+** está diseñado para controlar a los **administradores de red** que configuran los equipos.
- **Con RADIUS:** Cuando te autenticas (decís quién sos), automáticamente recibes todos los permisos asociados a tu perfil.
    
- **Con TACACS+:** Permite un control fino comando por comando. Podés permitir que un administrador entre al switch (_Autenticación_), pero **prohibirle** ejecutar ciertos comandos específicos como `reload` o `delete` (_Autorización_).

## Secure Network Communications — resumen para Security+

### Tres tecnologías principales

**VPN**, **TLS**, **IPSec** — protegen datos en tránsito contra fisgoneo/manipulación.

### Tipos de VPN (por topología)

- **Site-to-site**: conecta dos redes/oficinas completas entre sí (ej: sucursal ↔ sede). Alternativa barata a una línea alquilada dedicada. Todo el tráfico se enruta primero a la sede central (aunque sea para salir a internet) → más seguro pero más lento (hairpinning).
- **Client-to-site**: un solo host (laptop, celular) se conecta directo a la red central.
- **Clientless**: acceso remoto vía navegador, sin instalar software — es básicamente lo que ya usás todo el tiempo con **HTTPS/TLS** (candado verde). Usa TCP; su variante UDP es **DTLS** (Datagram TLS) — más rápido, ideal para video streaming.

### Full tunnel vs Split tunnel

||Full tunnel|Split tunnel|
|---|---|---|
|Qué enruta por VPN|**todo** el tráfico|solo tráfico hacia la sede; el resto va directo a internet|
|Seguridad|mayor|menor (superficie de ataque: pueden pivotear desde el tráfico sin cifrar)|
|Performance|menor|mayor|
|Cuándo usar|redes no confiables (wifi de hotel/café)|cuando priorizás velocidad|

### IPSec

Suite más popular para VPNs hoy. Da: **confidentiality** (encryption), **integrity** (hash check), **authentication**, **anti-replay** (sequence numbers).

**5 pasos del túnel IPSec:**

1. Solicitud de key exchange
2. **IKE Phase 1** — autentica las partes, crea canal seguro (túnel **ISAKMP**)
3. **IKE Phase 2** — negocia Security Associations, arma el túnel completo (túnel dentro del túnel)
4. Transferencia de datos
5. Terminación del túnel (por acuerdo mutuo o timeout)

**Modos:**

- **Transport mode**: mantiene el header IP original, no agrega overhead → usado en **client-to-site**. Evita superar el MTU (1500 bytes default).
- **Tunnel mode**: encapsula todo el paquete + nuevo header → usado en **site-to-site**. Aumenta tamaño de paquete, puede requerir **jumbo frames** (hasta 9000 bytes, solo en LAN propia) o bajar el MTU interno a ~1400 para dejar margen.

**AH vs ESP** (los dos protocolos de IPSec):

||Authentication Header (AH)|Encapsulating Security Payload (ESP)|
|---|---|---|
|Integrity + auth|✅|✅|
|Anti-replay|✅|✅|
|Confidentiality (encryption)|❌|✅|
|Qué cubre|todo el datagram|solo el payload (transport mode) o payload+header original (tunnel mode)|

- Transport mode + ESP: cifra el payload pero el header queda visible (se ve origen/destino, no el contenido) — analogía: ver el remitente en el sobre sin abrir la carta.
- Tunnel mode + AH+ESP: cifra hasta el header original → nadie ve ni origen ni destino real en internet.

### Para el examen

- **Site-to-site = tunnel mode** | **Client-to-site = transport mode** (asociación clásica de examen).
- **AH = integrity/auth sin encryption** | **ESP = todo, incluida encryption** — la trampa típica es preguntar cuál da confidencialidad.
- **Full tunnel = red no confiable / máxima seguridad**; **split tunnel = performance**, memorizar el trade-off.
- **DTLS = TLS sobre UDP**, útil para video/voice — no confundir con IPSec.
- Clientless VPN = TLS vía browser, sin instalar nada — HTTPS de todos los días es técnicamente esto.
- IKE Phase 1 vs Phase 2: Fase 1 = autentica y crea canal seguro (ISAKMP); Fase 2 = negocia las SAs y termina de armar el túnel de datos.
### SD-WAN & SASE

- **WAN Tradicional:** Topología en estrella (_hub-and-spoke_). Todo el tráfico de las sucursales viaja a la sede central antes de salir a internet. Genera cuellos de botella y baja el rendimiento.
    
- **SD-WAN (Software-Defined WAN):** Arquitectura virtual sobre los routers de las oficinas. Enruta de forma inteligente: el tráfico hacia la nube sale directo desde la sucursal y el tráfico corporativo va a la sede. _(Optimiza la red física)_.
    
- **SASE (Secure Access Service Edge):** Unifica **SD-WAN + Seguridad (Firewall, ZTNA, CASB)** en un único servicio **100% nativo de la nube**.
    
    - Reemplaza el hardware de la oficina por puntos de presencia en la nube.
        
    - Sirve por igual para sucursales y trabajadores remotos desde sus casas.
        
    - _Ejemplo real de mercado:_ Cloudflare One, Zscaler.

## Infrastructure Considerations — resumen para Security+

Seis factores a considerar al diseñar la arquitectura de red:

### 1. Device placement

- Ubicación de routers, switches, APs afecta performance y seguridad directamente.
- Router en el borde de la red → filtra tráfico entrante/saliente.
- Mala ubicación → cuellos de botella, puntos vulnerables, zonas sin cobertura.

### 2. Security zones & screened subnets

- **Security zone**: segmento aislado lógicamente (vía firewall u otro dispositivo) que agrupa dispositivos con nivel de confianza/requisitos similares (ej: servicios públicos, recursos internos, datos sensibles — cada uno con sus propias políticas de acceso).
- **Screened subnet**: buffer entre la red interna y una red externa no confiable (internet). **Término moderno de lo que antes se llamaba DMZ** — el examen usa "screened subnet", aunque en equipos reales todavía se vea "DMZ".
- Aloja servicios públicos (web, mail, DNS servers) → si se compromete, el atacante no llega directo a la red interna sensible.
- **Internet** $\rightarrow$ _(Firewall 1)_ $\rightarrow$ **Subred Protegida / Screened Subnet** _(Servidor Web, Mail)_ $\rightarrow$ _(Firewall 2)_ $\rightarrow$ **Red Interna Privada**.

### 3. Attack surface

- Suma de todos los puntos por donde un actor no autorizado podría entrar o extraer datos.
- Crece con: más dispositivos, apps, puertos abiertos innecesarios, misconfigs, software desactualizado, controles de acceso débiles.
- Reducirla = identificar vulnerabilidades y mitigarlas o eliminarlas — evaluación regular y proactiva.

### 4. Connectivity methods

|Tipo|Pros|Contras|
|---|---|---|
|Wired (Ethernet)|estable, rápido|poca movilidad|
|Fiber|más rápido y largo alcance, mínima degradación|—|
|Wireless (Wi-Fi, microwave, satellite)|flexible, escalable|interferencia, riesgos de seguridad si mal configurado|
|Hybrid|combina fortalezas, agrega redundancia|—|

- Elegir según: escalabilidad, velocidad, seguridad, presupuesto.

### 5. Device attributes: active/passive + inline/tap

- **Active** (ej: IPS): monitorea Y actúa sobre el tráfico en tiempo real.
- **Passive** (ej: IDS): solo observa/reporta, no interviene.
- **Inline**: dispositivo está en el camino directo del tráfico, puede bloquearlo/influirlo (firewall, router, IPS).
- **Tap/monitor**: fuera del path directo, solo escucha/captura para análisis, sin riesgo de interrumpir tráfico.

### 6. Fail mode

- **Fail-open**: en caso de fallo, deja pasar todo el tráfico sin inspección → prioriza disponibilidad, sacrifica seguridad.
- **Fail-closed**: en caso de fallo, bloquea todo el tráfico → prioriza seguridad, sacrifica disponibilidad.
- Elección depende de la criticidad del segmento: datacenter financiero → fail-closed; red wifi de invitados → fail-open (no hay nada sensible que proteger).

### Para el examen

- **Screened subnet es el término que van a usar en el examen**, no DMZ — memorizar este reemplazo de terminología explícitamente.
- Active/passive vs inline/tap son **dos ejes distintos** que se pueden combinar (ej: IPS es active E inline; IDS es passive y normalmente tap).
- Fail-open vs fail-closed: trampa clásica es preguntar cuál prioriza **availability** vs **security** — memorizar el par con ejemplos (guest wifi = fail-open, datacenter financiero = fail-closed).
- Attack surface: reducirla = eliminar vulnerabilidades/puertos innecesarios, no es lo mismo que "vulnerability" en sí.

## Selecting Infrastructure Controls 

### Qué es un control

Medida/salvaguarda aplicada para mitigar riesgos y proteger activos de la organización.

### Principios clave para seleccionar controles

- **Least privilege**: acceso mínimo necesario → reduce attack surface.
- **Defense in depth**: capas múltiples de seguridad; si una falla, las otras siguen protegiendo.
- **Risk-based approach**: priorizar controles según riesgo real, no tratar de mitigar todo (recursos limitados).
- **Lifecycle management**: revisar, actualizar y retirar controles periódicamente — no es "configurar y olvidar".
- **Open design principle**: transparencia — los controles deben poder someterse a escrutinio/testing riguroso (la seguridad no depende de que el diseño sea secreto).

### Metodología para seleccionar controles (proceso, en orden)

1. Evaluar estado actual (baseline de vulnerabilidades y controles existentes)
2. **Gap analysis** (postura actual vs deseada)
3. Fijar objetivos claros
4. Benchmarking contra mejores prácticas del sector
5. Cost-benefit analysis
6. Stakeholder involvement
7. Monitoring/feedback loops continuos

### Mejores prácticas

- Empezar siempre con una evaluación de riesgos exhaustiva y **recurrente** (no una sola vez).
- Alinear los controles a un framework reconocido: **NIST** (Cybersecurity Framework, Risk Management Framework) o **ISO**.
- Personalizar el framework al perfil de riesgo específico de la organización.
- Involucrar y capacitar a los stakeholders — un control es tan efectivo como la gente que lo implementa/monitorea.

### Para el examen

- Memorizar los 5 principios clave (least privilege, defense in depth, risk-based, lifecycle management, open design) — es la parte más "testeable" del video.
- **NIST CSF / NIST RMF** e **ISO** como frameworks de referencia — el examen puede preguntar cuál framework usar en un escenario dado.
- Este tema es más de gestión/proceso que técnico — esperá preguntas tipo "¿cuál es el primer paso al seleccionar controles?" (respuesta: evaluar el estado actual / risk assessment).