# ⚖️ Tensión Seguridad vs. Comodidad

La seguridad es difícil de mantener no solo por hackers externos, sino por amenazas internas (*insider threats*). Existe un conflicto constante: a mayor *usability*, menor es el *security posture* (y viceversa).

## Definiciones básicas
* **Information Security:** Protege los datos en sí.
* **Information Systems Security:** Protege los dispositivos y redes que procesan esos datos.

---

## 🛡️ Modelo CIA / CIANA

> [!INFO] CIA Triad & CIANA
> * **CIA Triad:** *Confidentiality* (acceso solo autorizado), *Integrity* (datos intactos) y *Availability* (recursos accesibles).
> * **CIANA:** Añade *Non-repudiation* (no poder negar una acción) y *Authentication*.

### 🔑 Modelo AAA
* **Authentication:** Verificar identidad.
* **Authorization:** Permisos de acceso.
* **Accounting:** Registro y auditoría de actividades.

---

## 🏰 Security Controls y Zero Trust
Se presentan categorías de controles (*technical, management, operational, physical*) y tipos (*preventive, deterrent, detective, corrective, compensatory, directive*), además del modelo Zero Trust (verificar siempre por defecto mediante el *control plane* y el *data plane*).

---

## ⚠️ Threat vs Vulnerability vs Risk

* **Threat:** Cualquier factor externo (*cyberattacks, natural disasters*) fuera de nuestro control directo que puede causar daño a un sistema. No se pueden evitar por completo, pero se busca minimizar su impacto.
* **Vulnerability:** Debilidad interna en el diseño, implementación o configuración de un sistema (*software bugs, misconfigurations, unpatched systems*). La organización sí tiene control sobre ellas para corregirlas o mitigarlas.
* **Risk:** Es la intersección donde se cruzan una *threat* y una *vulnerability*. Si hay una amenaza pero no existe una vulnerabilidad asociada (o viceversa), no existe un riesgo real.

### Risk Management
Consiste en tomar decisiones diarias para manejar el riesgo sobre los sistemas. Sus cuatro respuestas principales son:

| Respuesta | Descripción | Ejemplo |
| :--- | :--- | :--- |
| **Mitigate** | Reducir el impacto o la probabilidad | Aplicar parches de seguridad regularmente |
| **Transfer** | Trasladar el riesgo | Contratar un *cyberinsurance* para que una aseguradora cubra los costos financieros |
| **Avoid** | Evitar la actividad riesgosa | Prohibir el uso de memorias USB en la empresa |
| **Accept** | Asumir el riesgo | Mantener un servidor antiguo secundario sin actualizar si el costo de cambio supera el daño posible |

> [!TIP] Objetivo Principal
> Garantizar la *service continuity*, proteger los datos y mantener la postura de seguridad global minimizando las vulnerabilidades o aplicando medidas paliativas.

---

## 🤐 Confidentiality (primer pilar de la CIA Triad)

> [!NOTE] Concepto Clave
> Proteger la información frente al acceso o divulgación no autorizados (*"keeping data secret"*).

### Importancia
* Protege la privacidad personal (*personal privacy*).
* Mantiene la ventaja competitiva (*competitive advantage*).
* Garantiza el cumplimiento normativo (*regulatory compliance* como PII o PHI).

### Métodos para garantizarla
* **Encryption:** Convertir *plaintext* a *ciphertext* mediante claves (método principal).
* **Access Controls:** Restringir permisos de usuario (lectura/escritura).
* **Data Masking:** Ocultar parte de los datos sensibles (ej. mostrar solo los últimos 4 dígitos de una tarjeta).
* **Physical Security:** Cerraduras, biometría en *server rooms* o cámaras para proteger dispositivos e información física.
* **Training & Awareness:** Capacitación continua para prevenir el *human error* y la negligencia.

---

## ✅ Integrity (segundo pilar de la CIA Triad)

> [!NOTE] Concepto Clave
> Garantizar que los datos y sistemas se mantengan exactos, completos e inalterados (*"accurate and unchanged"*) desde su estado original, a menos que sean modificados intencionalmente por alguien autorizado.

### Importancia
* **Data Accuracy:** Asegura que las decisiones empresariales se tomen con información correcta.
* **Maintain Trust:** Previene la pérdida de confianza de los usuarios si los datos sufren alteraciones.
* **System Operability:** Evita fallos, comportamientos inesperados o caídas del sistema causadas por datos corruptos o alterados.

### Métodos para garantizarla
* **Hashing:** Genera un valor o huella digital (*hash digest*) de tamaño fijo. Cualquier cambio mínimo en los datos cambia drásticamente el resultado (técnica principal).
* **Digital Signatures:** Cifra el *hash digest* usando una *private key* para validar tanto *integrity* como *authenticity*.
* **Checksums:** Verifica la integridad de los datos durante su transmisión mediante la comparación de valores antes y después del envío.
* **Access Controls:** Restringe permisos para que solo personal autorizado pueda modificar o escribir datos.
* **Periodic Audits:** Revisiones sistemáticas de logs y operaciones para detectar discrepancias o cambios no autorizados.

---

## 🟢 Availability (tercer pilar de la CIA Triad)

> [!NOTE] Concepto Clave
> Garantizar que la información, sistemas y recursos estén operativos y accesibles para los usuarios autorizados cuando los necesiten (*"always available"*).

### 📊 Métrica de disponibilidad ("Nines of Availability")

| Nivel | Downtime anual permitido |
| :--- | :--- |
| **99% (Two Nines)** | Más de 3.5 días |
| **99.9% (Three Nines)** | Hasta 8.76 horas |
| **99.999% (Five Nines)** | Gold standard — máximo 5.26 minutos |

### Importancia
* **Business Continuity:** Evita pérdidas financieras por minutos de inactividad.
* **Customer Trust:** Previene la migración de clientes a la competencia por falta de acceso al servicio.
* **Reputation:** Mantiene la imagen y credibilidad de la organización a largo plazo.

### Redundancy (método principal)
Duplicación de componentes críticos para evitar *single points of failure*.
* **Server Redundancy:** *Load balancing* o *failover* entre múltiples servidores.
* **Data Redundancy:** Almacenamiento distribuido (RAID, backups locales y cloud).
* **Network Redundancy:** Múltiples proveedores e itinerarios de red.
* **Power Redundancy:** Generadores y sistemas UPS (*Uninterruptible Power Supply*).

---

## ✍️ Non-repudiation

> [!IMPORTANT] Definición
> Medida de seguridad que evita que una entidad niegue su participación en una comunicación o transacción digital.  
> **Mecanismo principal:** Se logra mediante el uso de *digital signatures*, las cuales combinan un hash del mensaje cifrado con la *private key* del usuario a través de *asymmetric encryption*.

### Importancia
* **Authenticity:** Confirma la identidad del emisor y previene la *impersonation*.
* **Integrity:** Valida que el contenido no haya sido alterado en tránsito.
* **Accountability:** Aporta trazabilidad e imputabilidad irrefutable sobre las acciones realizadas.

> [!QUESTION] ¿Por qué la integridad es un pilar propio y no solo un complemento?
> * **Podés tener integrity SIN non-repudiation:** Un archivo subido a un servidor puede incluir un hash público. Cualquiera puede descargarlo y verificar su *integrity*. Pero ese hash no te dice quién creó el archivo ni te impide negar su autoría.
> * **NO podés tener non-repudiation SIN integrity:** Para probar legal o técnicamente que firmaste un documento, el sistema debe demostrar primero que el documento no cambió un solo bit. Si el contenido cambió, la firma digital se rompe y el *non-repudiation* se destruye.

---

## 🪪 Authentication

**Definición:** Medida de seguridad que verifica que un usuario o entidad sea quien dice ser.

### Factores de autenticación
* **Knowledge factor** (*something you know*): Username, password.
* **Possession factor** (*something you have*): ID card, código enviado al smartphone.
* **Inherence factor** (*something you are*): Facial recognition, fingerprint.
* **Action factor** (*something you do*): Dinámica de escritura, forma de caminar.
* **Location factor** (*somewhere you are*): Geofencing, restricciones por país.

> [!TIP] MFA / 2FA
> La combinación de dos o más factores aumenta la seguridad para evitar el *unauthorized access*, proteger la privacidad del usuario y validar el uso legítimo de *shared resources*.

---

## 🎟️ Authorization

**Definición:** Determina los permisos y privilegios concedidos a una identidad autenticada, definiendo qué acciones puede realizar dentro de un sistema (*what you can do*).

### Conceptos clave
* **RBAC / Rule-Based / Attribute-Based:** Mecanismos que definen los accesos según la función, reglas o atributos del usuario.
* **Backend systems:** Áreas restringidas del sistema a las que solo acceden roles específicos como *system administrators*.

### Importancia
* **Protect sensitive data:** Restringe la información confidencial solo a personal autorizado.
* **Maintain system integrity:** Evita modificaciones accidentales o maliciosas.
* **Streamline user experience:** Muestra a cada usuario únicamente las opciones e interfaces relevantes para su rol.

---

## 📋 Accounting

**Definición:** Se centra en monitorear, rastrear y registrar detalladamente todas las actividades de los usuarios o entidades dentro de un sistema (*what you did*).

### Conceptos clave
* **Audit trail:** Registro cronológico de eventos para rastrear cambios o anomalías.
* **Compliance:** Cumplimiento de regulaciones legales mediante registros exhaustivos.
* **Forensic analysis:** Investigación post-incidente para entender cómo ocurrió una brecha de seguridad.
* **Resource optimization:** Seguimiento del uso de recursos para mejorar el rendimiento y reducir costos.
* **User accountability:** Fomento de la responsabilidad al saber que las acciones son rastreables.

### Tecnologías principales
* **Syslog servers:** Centralizan y agregan registros (logs).
* **Network analyzers:** Capturan y analizan el tráfico de red (ej. Wireshark).
* **SIEM:** Correlaciona y analiza alertas de seguridad en tiempo real.

---

## 🗂️ Security Control Categories

Clasifican los controles para lograr una postura de seguridad integral (*holistic approach*).

* **Technical controls:** Mecanismos de software y hardware automatizados (*firewalls, antivirus, encryption, IDS*).
* **Management controls:** Gobernanza y estrategia alineadas al riesgo (*risk assessments, security policies*).
* **Operational controls:** Procedimientos del día a día y procesos humanos (*backups, account reviews, awareness training*).
* **Physical controls:** Medidas tangibles en el mundo real (*cámaras, biometría, guardias, destrucción de documentos*).

---

## 🎛️ Security Control Types

| Tipo | Descripción | Ejemplo |
| :--- | :--- | :--- |
| **Preventive** | Bloquean la amenaza antes de que ocurra | Firewalls, access controls |
| **Deterrent** | Desaniman al atacante visibilizando consecuencias | Warning banners, carteles de advertencia |
| **Detective** | Monitorean e identifican actividades sospechosas | IDS, video surveillance |
| **Corrective** | Mitigan el daño y restauran el sistema | Antivirus aislando malware, backups |
| **Compensating** | Soluciones alternativas si no aplica el primario | Usar VPN sobre WPA2 en legacy systems |
| **Directive** | Dictan comportamientos mediante normas | Acceptable Use Policy (AUP) |

---

## 🏯 Zero Trust

### La idea central
Antes, la seguridad de redes funcionaba como un castillo medieval (*perimeter-based security*). Hoy, debido a la **deperimeterization** (trabajo remoto, nube, dispositivos móviles), el perímetro se diluyó y las murallas no alcanzan.

> [!WARNING] El Mantra de Zero Trust
> **"Never trust, always verify"** — No se confía en nada y se verifica todo, siempre. Da igual si el usuario está dentro o fuera de la red física.

### Las dos partes de la arquitectura

#### 1. Control plane (Define las reglas)
* **Adaptive identity:** Verificación continua y contextual.
* **Threat scope reduction:** Principio de *least privilege*.
* **Policy-driven access control:** Reglas según el rol.
* **Secured zones:** Zonas aisladas.
* **Componentes:** *Policy engine* (libro de reglas) y *policy administrator*.

#### 2. Data plane (Ejecuta las reglas)
* **Subject/system:** Quien pide acceso.
* **Policy enforcement point:** Concede o deniega el acceso según el *control plane*.

> [!NOTE] Control Plane vs Data Plane
> * **Control plane:** El cerebro que decide mediante reglas y contexto.
> * **Data plane:** El músculo que ejecuta. Aplica a rajatabla la orden del *control plane* (permite o bloquea).

---

## 📊 Gap Analysis

**Definición:** Proceso para evaluar la diferencia entre el estado actual y el estado deseado con el fin de identificar mejoras.

> [!INFO] Pasos del Análisis
> 1. Definir el alcance.
> 2. Recolectar datos del estado actual.
> 3. Analizar y detectar los *gaps*.
> 4. Armar plan de acción (metas + timeline).

### Tipos
* **Technical gap analysis:** Infraestructura (redes, cifrado, Zero Trust).
* **Business gap analysis:** Procesos de negocio (gestión de datos, forecasting).

> [!TIP] En la práctica
> Se alimenta de un *vulnerability assessment* y genera como *output* un **POA&M** (*Plan of Action and Milestones*) que prioriza recursos y plazos.