This video es solo la intro/overview de la sección de IAM — no trae contenido técnico nuevo, así que va el resumen corto que pediste.

## Identity and Access Management (IAM) — overview de la sección

Video introductorio, sin profundidad técnica todavía. Plantea el mapa de lo que viene en la sección, cubriendo objetivos **2.4** (indicadores de actividad maliciosa) y **4.6** (implementación de IAM).

**Los 4 procesos de IAM:**

- Identification (usuario reclama identidad — username/email)
- Authentication (verificación de esa identidad)
- Authorization (qué permisos/nivel de acceso tiene)
- Accounting (auditoría — tracking y logging de actividad)

**Temas que se van a cubrir en la sección:**

- Provisioning/deprovisioning de cuentas, identity proofing, interoperability, attestation
- MFA — factores (something you know/have/are/do, somewhere you are) e implementaciones (biometría, hard/soft tokens, security keys, passkeys)
- Password security — políticas, password managers, passwordless auth
- Ataques de contraseña: password spraying, brute force, dictionary, hybrid attacks + demo de cracking
- SSO (LDAP, OAuth, SAML) y Federation
- PAM (Privileged Access Management) — just-in-time permissions, password vaulting, cuentas temporales
- Modelos de control de acceso: MAC, DAC, RBAC, rule-based, ABAC + time-of-day restrictions + least privilege
- Permission assignments

**Para el examen:** nada para memorizar todavía — es solo el índice de la sección. Cuando lleguen los videos con contenido, ahí sí entran los detalles de examen.

## Identity and Access Management (IAM) — resumen para Security+

un **sistema o software centralizado para administrar usuarios, contraseñas y sus permisos**.
### Los 4 procesos core de IAM

|Proceso|Qué hace|Ejemplo del video|
|---|---|---|
|**Identification**|El usuario reclama una identidad (username/email)|Login con username; también verificar que billing/shipping address coincidan en e-commerce para evitar fraude de pago|
|**Authentication**|Verifica esa identidad contra una base de datos de usuarios autorizados|Password, biometría o MFA después del username|
|**Authorization**|Determina qué permisos/nivel de acceso tiene el usuario ya autenticado|RRHH accede a legajos de personal; Finanzas solo a datos financieros|
|**Accounting**|Auditoría — logging de login/logout, acciones, cambios en el sistema|Ayuda a detectar incidentes, encontrar vulnerabilidades y dar evidencia en caso de breach|

### Otros conceptos clave de IAM

- **Provisioning**: crear la cuenta nueva y darle los permisos/accesos correspondientes (ej: empleado nuevo → email, acceso a red interna, sistemas necesarios para su puesto).
- **Deprovisioning**: sacar los derechos de acceso cuando ya no se necesitan (ej: empleado se va). Crítico para evitar acceso no autorizado o sabotaje de un ex-empleado como insider threat.
- **Identity proofing**: verificar la identidad del usuario **antes** de crear la cuenta (chequear datos contra fuente confiable, o pedir DNI/pasaporte).
- **Interoperability**: que distintos sistemas/dispositivos/apps trabajen juntos compartiendo info — típicamente vía estándares como **SAML** u **OpenID Connect** para auth/authz sin fricción entre sistemas.
- **Attestation**: validar periódicamente que las cuentas y sus derechos de acceso siguen siendo correctos y actualizados (auditorías/reviews regulares) para asegurar least privilege.

### Para el examen

- Memorizar bien el orden y la diferencia entre los 4 procesos: **Identification → Authentication → Authorization → Accounting** (a veces aparece como "AAA" en el examen, aunque acá suman Identification como cuarto paso previo).
- Trampa clásica: confundir **Identification** (afirmar quién sos) con **Authentication** (probarlo). Identification = username; Authentication = password/MFA/biometría.
- Otra trampa: **Provisioning vs. Deprovisioning** — el examen le da mucha bola al deprovisioning como control de seguridad (prevenir insider threats de ex-empleados).
- **Identity proofing** ≠ Authentication: proofing pasa _antes_ de crear la cuenta (validación inicial de identidad), authentication pasa _después_, cada vez que el usuario intenta loguearse.
- Si aparece **SAML** u **OpenID Connect** en una pregunta sobre compartir identidades entre sistemas → pensar interoperability/federation.
- **Attestation** = revisión periódica de accesos (piensa "auditoría de permisos", no autenticación).


**Multi-Factor Authentication (MFA)** 
**Objetivo de MFA**
Crear una **Defense in Depth** (defensa en capas) combinando dos o más categorías independientes de credenciales para verificar la identidad.

---

**Los 5 Factores de Autenticación**

Something You Know: Lo que sabés (Contraseña, PIN, preguntas de seguridad).

Something You Have: Lo que tenés (Token físico, App Authenticator, Smartcard).

Something You Are: Lo que sos (Huella, Face ID, Iris, Reconocimiento de voz, Dinámica de tecleo).

Somewhere You Are: Dónde estás (Ubicación GPS, Dirección IP, Red corporativa).

Something You Do: Lo que hacés (Acciones de comportamiento, patrones de movimiento).

---

**Tipos de Autenticación**

* **Single-Factor Authentication (SFA):** Utiliza una sola categoría de factor (ej. *Username* + *Password* + *PIN* sigue siendo SFA porque todo es *Knowledge Factor*).
* **Two-Factor Authentication (2FA):** Combina exactamente 2 factores de categorías **diferentes** (ej. *Password* + *SMS OTP*).
* **Multi-Factor Authentication (MFA):** Utiliza 2 o más factores diferentes. Todo 2FA es MFA, pero MFA puede incluir 3, 4 o 5 factores.

---

**Gestores de Contraseñas y Passkeys**

* **Password Managers:** Almacenan contraseñas complejas en un *encrypted vault* mediante una *Master Password*.
* **Passkeys (Passwordless Authentication):** Alternativa basada en **Public Key Cryptography**:
* **Private Key:** Se guarda de forma segura en el dispositivo del usuario (desbloqueada con biometría o PIN).
* **Public Key:** Se almacena en el servidor de autenticación.
* **Ventaja:** Altamente resistente a ataques de *Phishing* y *Server Data Breaches* (el servidor no guarda secretos).
Una **Passkey** es una tecnología para iniciar sesión **sin usar contraseñas** (_Passwordless_). En lugar de escribir una clave, usás el **desbloqueo de tu propio celular o computadora** (tu huella, Face ID o el PIN de la pantalla).

### 1. Las 5 Políticas de Contraseñas (Configuración en Windows / Group Policy)

Para configurar las políticas de contraseñas locales en Windows se utiliza la ruta: `gpedit.msc` $\rightarrow$ _Computer Configuration_ $\rightarrow$ _Windows Settings_ $\rightarrow$ _Security Settings_ $\rightarrow$ _Account Policies_ $\rightarrow$ _Password Policy_.

- **Password Length (Longitud):** Es el factor más crítico. El incremento de cada carácter aumenta la seguridad de forma exponencial (ej. pasar de 4 a 8 dígitos en un PIN amplía las combinaciones de $10^4 = 10.000$ a $10^8 = 100.000.000$). Se recomiendan entre **12 y 16 caracteres mínimo**.
    
- **Password Complexity (Complejidad):** Exige combinar letras mayúsculas, minúsculas, números y caracteres especiales. Amplía el conjunto de caracteres posibles por posición (de 10 dígitos a 72+ caracteres).
    
- **Password Reuse / Password History (Reutilización / Historial):** Define cuántas contraseñas nuevas deben crearse antes de poder reutilizar una antigua. Evita que los usuarios vuelvan a claves comprometidas. En Windows se puede configurar hasta un máximo de **24 contraseñas**.
    
- **Password Expiration / Maximum Password Age (Caducidad / Edad Máxima):** Fuerza al usuario a cambiar la contraseña tras un período determinado (ej. 90 días).
    
    - _Nota actual:_ El **NIST ya no recomienda** forzar la caducidad periódica, a menos que se utilicen gestores de contraseñas, ya que induce a los usuarios a crear patrones débiles o predecibles (ej. `Password1`, `Password2`).
        
- **Minimum Password Age (Edad Mínima):** Tiempo mínimo que debe pasar antes de que un usuario pueda volver a cambiar su contraseña (ej. 1 a 3 días). **Evita el fraude del historial:** impide que el usuario cambie la clave 24 veces seguidas en un minuto para volver a usar su contraseña preferida.
    

### 2. Gestores de Contraseñas (_Password Managers_)

Herramientas (_Bitwarden, 1Password, Dashlane_) que almacenan las credenciales en una bóveda cifrada protegida por una clave maestra (_Master Password_).

- **Password Generation:** Creación automática de claves largas, complejas y aleatorias.
    
- **Autofill:** Relleno automático de credenciales que evita errores de digitación y mitiga ataques de _Keylogging_.
    
- **Secure Sharing:** Compartir credenciales entre usuarios autorizados sin revelar el texto plano de la contraseña.
    
- **Cross-Platform Access:** Sincronización entre múltiples dispositivos (laptops, extensiones de navegador, smartphones).
    

### 3. Autenticación Sin Contraseña (_Passwordless Authentication_)

Elimina la necesidad de crear, recordar o ingresar contraseñas tradicionales, aumentando la seguridad y la experiencia del usuario.

- **Biometric Authentication:** Verificación de identidad mediante rasgos biológicos (_Huella dactilar, Face ID, Iris Scan_).
    
- **Hardware Tokens:** Dispositivos físicos (ej. YubiKey) que generan o contienen credenciales de inicio de sesión.
    
- **One-Time Passwords (OTP):** Códigos temporales enviados por correo o SMS (validez típica de 3 a 10 minutos).
    
- **Magic Links:** Enlaces temporales de un solo uso enviados por correo electrónico que inician sesión automáticamente al hacer clic.
    
- **Passkeys:** Estándar moderno basado en criptografía de clave pública integrado en el navegador/SO, donde el usuario autentica localmente usando su dispositivo.
    

### 💡 Puntos Clave para el Examen Security+

- **Longitud vs. Complejidad:** El examen favorece la **longitud** por sobre la complejidad. Una frase de paso larga (_passphrase_) es computacionalmente más difícil de romper mediante fuerza bruta que una contraseña corta compleja.
    
- **Recomendaciones Actuales del NIST sobre Contraseñas:**
    
    - **NO** forzar la caducidad rotativa periódica (evita patrones predecibles).
        
    - **NO** usar preguntas de seguridad basadas en conocimiento (fáciles de investigar mediante ingeniería social).
        
    - Bloquear activamente contraseñas comunes o filtradas (_dictionary attacks_).
        
- **Ataque para saltar el _Password History_:** Si la red exige un historial de 24 contraseñas pero **NO** tiene configurada una _Minimum Password Age_, el usuario puede cambiar la clave 24 veces en 2 minutos y volver a la original.
    
- **Passkeys en el Examen:** Identifícalas como la solución principal resistente a _Phishing_ y _Server Data Breaches_ que implementa la arquitectura _Passwordless_.

### Tipos de Ataques a Contraseñas

- **Fuerza Bruta (_Brute Force_):** Prueba **todas las combinaciones posibles** de caracteres secuencialmente (A, B... AA, AB). Es minucioso pero costoso computacionalmente para claves largas. Eficaz contra claves cortas o PINs (un PIN de 4 dígitos tiene $10^4 = 10.000$ opciones y se rompe en segundos).
    
      
    
- **Diccionario (_Dictionary Attack_):** Utiliza una lista (_wordlist_) de contraseñas conocidas o palabras comunes. Incluye variaciones típicas y **Leet Speak** (sustitución de letras por números/símbolos, ej. `P@$$w0rd`). Es más rápido que la fuerza bruta pero ineficaz contra claves complejas fuera del diccionario.
    
      
    
- **Pulverización de Contraseñas (_Password Spraying_):** Prueba **una o pocas contraseñas comunes** (ej. `Password123`) contra **múltiples cuentas** de usuario. Evita el bloqueo de cuentas (_account lockout_) que ocurriría al probar miles de claves contra un solo usuario.
    
      
    
- **Ataque Híbrido (_Hybrid Attack_):** Combina diccionario y fuerza bruta. Toma una palabra base de una lista (ej. `fabuloso`) y le aplica fuerza bruta a una sección de caracteres añadidos (ej. probar desde `fabuloso000001` hasta `fabuloso999999`).
    
      
    

### Mitigaciones y Herramientas Destacadas

- **Herramienta:** **John the Ripper** es un ejecutable de auditoría y descifrado de hashes (_offline_). Guarda los resultados en `john.pot` para no repetir el proceso.
    
      
    
- **Criptografía:** Algoritmos débiles como **MD5** son vulnerables a tablas de búsqueda (_lookup tables_). Se debe usar **SHA-256** (o superior) combinado con **Sal (_Salting_)** (cadena aleatoria agregada antes del _hash_) para inutilizar diccionarios y tablas _rainbow_.
    
      
    
- **Defensas Principales:** Aumentar la longitud/complejidad, aplicar **Autenticación Multifactor (MFA)**, limitar intentos de inicio de sesión, usar **CAPTCHAs** (_online_) y adoptar gestores de contraseñas.
    
      
    

### 💡 Puntos Clave para el Examen Security+

- **Diferencia Crítica:** Si prueban **10.000 claves contra 1 usuario**, es _Brute Force_ (causa _Account Lockout_). Si prueban **1 clave contra 10.000 usuarios**, es _Password Spraying_ (diseñado para saltar el _Account Lockout_).
    
      
    
- **Función de la Sal (_Salting_):** Su objetivo principal en el examen es **derrotar las tablas de búsqueda precomputadas (_Rainbow Tables_)** y garantizar que dos usuarios con la misma contraseña tengan hashes completamente diferentes.
    
      
    
- **Online vs. Offline Cracking:**
    
      
    - _Online:_ El atacante interactúa con la interfaz de inicio de sesión (se mitiga con CAPTCHA y _Lockout Policies_).
        
          
        
    - _Offline:_ El atacante ya robó la base de datos de hashes e intenta descifrarlos localmente en su máquina con herramientas como _John the Ripper_ o _Hashcat_ (no le afectan las políticas de bloqueo).
### Resumen: SSO y sus Tecnologías

- **SSO (Single Sign-On):** Es el **concepto u objetivo**. Permite al usuario autenticarse una sola vez y acceder a múltiples aplicaciones sin volver a ingresar su contraseña.
    
      
    
- **OAuth 2.0:** Protocolo estándar de autorización/autenticación para web, móviles y APIs. Utiliza **tokens en formato JSON** (ej. "Iniciar sesión con Google").
    
      
    
- **SAML:** Protocolo estándar para federación de identidades en entornos empresariales y servicios SaaS. Utiliza **mensajes en formato XML**.
    
      
    
- **LDAP / LDAPS:** **No es SSO**, es el servicio de directorio (la base de datos centralizada de usuarios y permisos) que los sistemas de SSO consultan por detrás. Usa los puertos **389 (LDAP)** y **636 (LDAPS cifrado)**.
    
      
    

### Tabla Comparativa

|**Tecnología**|**¿Es SSO?**|**Función principal**|**Formato / Puerto**|
|---|---|---|---|
|**SSO**|Es la meta|Permitir loguearse una sola vez|N/A (Concepto)|
|**OAuth**|**Sí**|Acceso a apps de terceros/móviles sin dar la clave|**JSON** (JWT)|
|**SAML**|**Sí**|Autenticación entre empresas y servicios en la nube|**XML**|
|**LDAP(S)**|**No**|Base de datos centralizada de usuarios y grupos|TCP **389** / **636**|

### 💡 Tip para el Examen Security+

- Si en la pregunta mencionan **XML** y **empresas/SaaS** $\rightarrow$ la respuesta es **SAML**.
    
      
    
- Si mencionan **JSON**, **tokens** o **APIs/móviles** $\rightarrow$ la respuesta es **OAuth**.
    
      
    
- Si preguntan por la **base de datos central de usuarios de red** $\rightarrow$ la respuesta es **LDAP**.

### ¿Qué es la Federación de Identidades?

Es un sistema que extiende el **SSO entre diferentes organizaciones o dominios independientes**. Permite a los usuarios acceder a sistemas de terceros usando sus credenciales corporativas u origen, basándose en una **relación de confianza preestablecida**.

---

### Componentes y Roles

* **IdP (Identity Provider / Proveedor de Identidad):** Sistema de origen que gestiona las credenciales y autentica al usuario (ej. la red de tu empresa o Google).
* **SP (Service Provider / Proveedor de Servicios):** Sistema o aplicación de destino al que el usuario quiere acceder (ej. un software SaaS de un proveedor externo).

---

### El Proceso en 6 Pasos

1. **Inicio:** El usuario solicita acceso en el SP.
2. **Redirección:** El SP redirige al usuario hacia el IdP de su red de origen.
3. **Autenticación:** El usuario ingresa sus credenciales directamente en el IdP.
4. **Aserción:** El IdP valida la identidad y genera un token/documento firmado (usando **SAML** u **OpenID Connect**).
5. **Devolución:** El usuario es redirigido de vuelta al SP llevando consigo la aserción.
6. **Acceso:** El SP verifica la firma de la aserción y le concede el acceso al usuario.

---

### Diferencia Clave: SSO Local vs. Federación

* **SSO Tradicional:** Funciona **dentro de una sola organización** (ej. iniciás sesión en Windows y accedés al correo interno y la impresora).
* **Federación:** Funciona **entre distintas organizaciones** (ej. iniciás sesión con la cuenta de tu empresa y accedés al sistema de un proveedor externo sin que ellos tengan que crearte un usuario).

---

### 💡 Puntos Clave para el Examen Security+

* **Reducción de Carga Administrativa:** La organización receptora (SP) **no administra cuentas ni contraseñas** para usuarios externos; confía totalmente en la autenticación del IdP.
* **Estándares Utilizados:** **SAML** (XML) y **OpenID Connect / OIDC** (capa de identidad sobre OAuth 2.0 que usa JSON).
* **Relación de Confianza (*Trust Relationship*):** Para que la federación funcione, el SP y el IdP deben intercambiar previamente certificados digitales o claves criptográficas.
SSO dentro de tu empresa (Empresa $\rightarrow$ Empresa externa): Usa SAML (XML).SSO en la web moderna / móvil (Usuario $\rightarrow$ Google / Apps): Usa OpenID Connect / OIDC (OAuth 2.0 + JSON).Ambos son las herramientas que hacen posible la Federación de Identidades.
Se llama federación porque organizaciones separadas "se alían" para confiar en las identidades de las demás, evitando que cada una tenga que crear y administrar sus propios usuarios desde cero.

### ¿Qué es PAM (*Privileged Access Management*)?

Es la solución de seguridad (políticas y herramientas) diseñada para **proteger, controlar y auditar las cuentas con altos privilegios** (como `admin`, `root` o administradores de dominio). Aplica el principio de **menor privilegio** para evitar filtraciones y abusos de control.

---

### Componentes Clave de PAM

* **Permisos Just-In-Time (JIT):** Los accesos administrativos se otorgan **solo en el momento exacto** en que se necesitan y se revocan automáticamente al terminar la tarea. Evita los privilegios administrativos permanentes (*standing privileges*).
* **Bóveda de Contraseñas (*Password Vault*):** Almacén digital seguro (protegido con MFA) donde se guardan y rotan las claves de cuentas críticas. Registra un historial auditable (*log*) de quién usó cada credencial y cuándo.
* **Cuentas Temporales / Provisionales:** Cuentas creadas para un propósito específico y con fecha de expiración automática (ideal para contratistas, auditores o proyectos puntuales).

---

### 💡 Puntos Clave para el Examen Security+

* **Reducción de la Superficie de Ataque:** Al usar JIT y cuentas temporales, un atacante no puede aprovechar credenciales privilegiadas "inactivas" porque estas no tienen acceso continuo.
* **Trazabilidad y No Repudio:** La bóveda de contraseñas obliga a los administradores a "solicitar" la clave, permitiendo saber exactamente qué persona física realizó una acción con una cuenta compartida (`root` o `Administrator`

Modelos de Control de Acceso (Access Control)
Son las reglas de diseño (políticas) que configurás en sistemas operativos, firewalls o servidores para definir qué usuarios acceden a qué recursos. Se pueden combinar entre sí.

Los 5 Modelos de Control de Acceso
MAC (Mandatory Access Control - Obligatorio):
strictly regulated
La clave: Basado en etiquetas de seguridad (Secreto, Alto Secreto).

Quién manda: El sistema central. El usuario no puede cambiar permisos de nada (estricto, uso militar/gobierno).

DAC (Discretionary Access Control - Discrecional):

La clave: A discreción del dueño del archivo.

Quién manda: El creador/propietario del recurso decide quién entra y quién no (ej. dar permisos en una carpeta de tu PC o Google Drive).

Role-BAC (Role-Based Access Control - Basado en Roles):

La clave: Permisos asignados por puesto o grupo laboral (Contabilidad, RRHH).

Quién manda: El administrador del sistema. Facilita la gestión ante altas y bajas de personal.

Rule-BAC (Rule-Based Access Control - Basado en Reglas):

La clave: Reglas estáticas "Si / Entonces" globales.

Quién manda: El administrador mediante firewalls o ACLs (ej. "Bloquear todo el tráfico web a las 20:00").

ABAC (Attribute-Based Access Control - Basado en Atributos):

La clave: Evalúa múltiples variables del momento (Rol del usuario + Hora + Ubicación/IP + Sensibilidad del archivo).

Es el más flexible y dinámico.

Conceptos Complementarios para el Examen
Restricciones Horarias (Time-based restrictions): Limitan el acceso según el horario (ej. permitir login solo de 8:00 a 18:00).

Principio de Mínimo Privilegio (Least Privilege): Otorgar solo los permisos mínimos necesarios para trabajar, ni uno más.

Acumulación de Permisos (Privilege Creep): Cuando un empleado cambia de puesto y acumula permisos viejos sin revocar. Viola el mínimo privilegio.


### Tipos de Cuentas, Controles y Permisos en Windows

* **Cuenta de Administrador Local:** Tiene control total del sistema (equivale a la "llave maestra"). Puede instalar software, cambiar configuraciones del sistema y modificar permisos.
* **Cuenta de Usuario Estándar:** Destinada al uso diario. Solo puede modificar sus propios archivos y carpetas en su área de usuario; **no puede instalar software global ni cambiar configuraciones del sistema**.
* **Cuenta Microsoft:** Cuenta en la nube (Online) que permite autenticarse y sincronizar servicios (Windows, Office 365, Xbox) entre múltiples dispositivos en lugar de usar una cuenta local.
* **UAC (*User Account Control*):** Mecanismo de seguridad en Windows que pide confirmación o credenciales administrativas antes de ejecutar acciones elevadas (identificado con el icono del **escudo**). Protege contra malware que intente ejecutarse como administrador en segundo plano.

---

### Permisos de Archivos y Carpetas (NTFS)

* **Ubicación:** Clic derecho sobre el archivo/carpeta $\rightarrow$ *Propiedades* $\rightarrow$ pestaña **Seguridad**.
* **Nivel de aplicación:** Se pueden configurar a nivel de **archivo individual** o a nivel de **carpeta** (los archivos dentro de una carpeta heredan los permisos establecidos en ella).
* **Niveles de permiso comunes:** Leer (*Read*), Escribir (*Write*), Ejecutar (*Execute*) y Control Total (*Full Control*).
* **Uso de Grupos:** En lugar de asignar permisos usuario por usuario, se agregan los usuarios a un grupo (ej. *Administradores*, *Usuarios*) y el usuario **hereda** los permisos asignados a ese grupo.

---

### 💡 Puntos Clave para el Examen Security+

* **UAC y Mínimo Privilegio:** UAC permite que incluso un administrador navegue en un estado de "usuario estándar" continuo, elevando sus privilegios solo cuando confirma la ventana contextual de UAC.