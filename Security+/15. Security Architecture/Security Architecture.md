Otra intro de sección — **Security Architecture** (Domain 3 y 4, objetivos 3.1 y 4.1). Resumen corto:

## Security Architecture — overview de la sección

- **Objetivo 3.1**: comparar/contrastar implicaciones de seguridad de distintos modelos de arquitectura.
- **Objetivo 4.1**: dado un escenario, aplicar técnicas de seguridad comunes a recursos de computación.

**Security architecture** = diseño, estructura y comportamiento del entorno de seguridad de la información — abarca hardware, software, procesos y personas.

Temas que van a ver en las próximas lecciones:

- **On-premise vs Cloud**: infraestructura local propia vs. servicios entregados vía internet (servers, storage, DB, networking, software, analytics)
- **Cloud security**: vulnerabilidades de servidores físicos compartidos, seguridad inadecuada de entornos virtuales, gestión de acceso, falta de updates, single points of failure, malas prácticas de auth/encriptación, políticas poco claras, data remnants
- **Virtualization & Containerization**: tipos, ventajas/riesgos, vulnerabilidades como **VM escape** y **resource reuse**
- **Serverless computing**: el cloud provider gestiona la asignación de servidores dinámicamente — devs solo se enfocan en el código
- **Microservices**: arquitectura de servicios pequeños e independientes, cada uno con una función de negocio específica
- **Network architecture**: separación física (air gaps) vs lógica, subnetting
- **SDN** (Software Defined Networking): gestión de red dinámica y programática
- **IaC** (Infrastructure as Code): aprovisionar la stack tecnológica vía software en vez de configuración manual
- **Centralized vs Decentralized architectures**: ventajas/riesgos de cada una
- **IoT**: redes de dispositivos físicos con sensores/software/conectividad
- **ICS y SCADA**: Industrial Control Systems (manufactura, transporte, energía, utilities); SCADA = subconjunto de ICS para procesos como electricidad, gas, agua, aguas residuales
- **Embedded systems**: sistemas informáticos dedicados a 1-2 funciones específicas, integrados en un dispositivo físico completo

**Para el examen:** nada específico todavía para memorizar, pero anotá las siglas que vienen: **SDN, IaC, ICS, SCADA, IoT**. Esta sección es densa — combina temas de virtualización/cloud con OT/infraestructura crítica. El próximo video con contenido real entra en On-premise vs Cloud.

## On-Premise vs Cloud

### Cloud Computing

Entrega de servicios de computación vía internet: servidores, storage, DB, networking, software, analytics, AI. Ventajas: innovación más rápida, recursos flexibles, economías de escala. Ej: Netflix usa cloud (AWS) para streaming a millones de usuarios.

### Conceptos clave del cloud

**Responsibility Matrix** (a.k.a. Shared Responsibility Model) Define cómo se divide la responsabilidad entre proveedor y cliente. Ej en **IaaS**: el proveedor gestiona infraestructura; el cliente maneja OS, middleware, runtime, datos y apps. Varía según el modelo de servicio (IaaS/PaaS/SaaS) y el contrato — pero siempre es parte del acuerdo.

**Third-party vendors** Proveen servicios especializados que mejoran funcionalidad/seguridad/eficiencia de soluciones cloud (gestión de costos, seguridad, analytics). Ej: VMware CloudHealth para gestión de costos/gobernanza/seguridad multi-cloud.

**Hybrid solutions** Combinan on-premise + private cloud + public cloud, moviendo cargas de trabajo entre entornos según necesidad. Consideraciones: seguridad de datos, compliance, interoperabilidad, costo. Ejemplo del video: proveedor de salud aloja datos de pacientes on-premise (por **HIPAA**) pero usa cloud para tareas de menor sensibilidad como email hosting o analytics (para escalar según demanda y optimizar costos).

### On-Premise

Infraestructura física en las instalaciones propias — la empresa mantiene hardware, software y recursos. Ejemplo: bufete de abogados pequeño mantiene todo local por la naturaleza sensible de documentos legales — control total, acceso inmediato, sin exposición a terceros o brechas asociadas a cloud.

### 11 Consideraciones clave al elegir cloud vs on-premise

|Consideración|Qué significa|Ejemplo del video|
|---|---|---|
|**Availability**|Poder acceder al sistema cuando se necesita|AWS SLA de 99.99% uptime en S3/EC2|
|**Resilience**|Capacidad de recuperarse de fallos y seguir operando|GCP mantiene datos accesibles aunque fallen 2 data centers simultáneamente|
|**Cost**|Costos iniciales bajos pero costos recurrentes pueden crecer|AWS pay-as-you-go vs reserved instances para reducir costo a largo plazo|
|**Responsiveness**|Velocidad para adaptarse a cambios de demanda|Azure autoscaling para picos de tráfico en e-commerce|
|**Scalability**|Capacidad de manejar carga de trabajo creciente|Netflix escala con AWS sin invertir en infraestructura física|
|**Ease of deployment**|Rapidez de implementación|Shopify — tienda online en minutos, sin servidores físicos|
|**Risk transference**|Parte del riesgo se transfiere al proveedor, pero cliente sigue responsable de sus datos/apps|Salesforce maneja infra del CRM; la empresa sigue responsable de user access y protección de datos|
|**Ease of recovery**|Facilidad para recuperar datos/backups|Dropbox — recuperar archivos borrados accidentalmente|
|**Patch availability**|El proveedor libera parches regularmente, cliente no gestiona esto|Office 365 recibe updates automáticos de Microsoft|
|**Inability to patch**|A veces no se puede aplicar un parche (compatibilidad, falta de control del entorno)|App legacy en la nube incompatible con nueva versión|
|**Power**|El cliente no gestiona consumo eléctrico — lo maneja el proveedor|Reduce costos y elimina gestión de energía para el cliente|
|**Compute**|Cantidad de recursos computacionales disponibles (CPU, memoria, storage)|AWS ofrece desde instancias pequeñas hasta alto rendimiento|
![[Pasted image 20260818103944.png]]
### Para el examen

- **Responsibility Matrix / Shared Responsibility Model** es el concepto más importante de esta lección — memorizar que varía según IaaS/PaaS/SaaS (en IaaS el proveedor solo gestiona infraestructura; en SaaS gestiona casi todo excepto datos/acceso de usuario).
- **Risk transference ≠ risk elimination**: la trampa clásica — el cliente SIEMPRE sigue siendo responsable de datos y control de acceso, sin importar cuánto se tercerice.
- **On-premise = control total pero caro/difícil de escalar; Cloud = flexible/escalable pero menos control directo; Hybrid = balance** — patrón de comparación que suele aparecer en preguntas de escenario (dado un caso, elegir el modelo correcto — ej: salud con HIPAA → hybrid, con datos sensibles on-prem).
- Las **11 consideraciones** son candidatas a preguntas de matching/definición — vale la pena memorizar la lista completa, aunque no todas tienen el mismo peso; las más citadas en exámenes reales suelen ser availability, scalability, cost y risk transference.
## Cloud Security — Vulnerabilities & Mitigations

Punto central: **cloud security es responsabilidad compartida** — el proveedor asegura la infraestructura subyacente, el cliente asegura sus propios datos y apps (conecta directo con la Responsibility Matrix vista en la lección anterior).

### 7 Vulnerabilidades clave y sus mitigaciones

**1. Shared physical server vulnerabilities** Múltiples usuarios comparten el mismo servidor físico — si un usuario se ve comprometido, potencialmente afecta a otros en el mismo servidor.

- Mitigación: **hypervisor protection**, **secure multi-tenancy**, escaneo periódico de vulnerabilidades y patching.

**2. Inadequate virtual environment security** Virtualización es la base del cloud, pero seguridad débil en VMs → acceso no autorizado, breaches.

- Mitigación: plantillas de VM seguras, patching/updates regulares, monitoreo de actividad inusual, **network segmentation** para aislar VMs y limitar movimiento lateral de atacantes.

**3. User access management** Gestión inadecuada de acceso → contraseñas débiles, permisos excesivos, falta de monitoreo.

- Mitigación: políticas de contraseñas fuertes, **MFA**, **least privilege**, monitoreo de actividad de usuarios.

**4. Lack of updated security measures** Entornos cloud son dinámicos — no mantenerse al día deja el sistema vulnerable a nuevas amenazas.

- Mitigación: patching regular, revisión/actualización periódica de políticas de seguridad, mantenerse informado sobre amenazas y best practices.

**5. Single points of failure** Dependencia de recursos/procesos específicos — si fallan, corte total del sistema.

- Mitigación: redundancia y **failover procedures**, múltiples servidores/data centers/proveedores cloud, testing periódico de failover.

**6. Poor authentication and encryption practices** Auth débil = acceso no autorizado; encriptación débil = datos expuestos en tránsito/reposo.

- Mitigación: MFA, algoritmos de encriptación fuertes, prácticas seguras de gestión de claves.

**7. Unclear policies** Falta de guías/procedimientos claros (data handling, access control, incident response) → confusión e inconsistencia → vulnerabilidades. Ej: sin política clara de manejo de datos, empleados pueden no saber cómo almacenar/compartir/eliminar datos correctamente.

- Mitigación: políticas de seguridad claras y completas, revisadas/actualizadas periódicamente, comunicadas efectivamente, reforzadas con training/awareness programs.

**8. Data remnants** Datos residuales que quedan tras procesos de eliminación/borrado — no se eliminan por completo por procedimientos inadecuados, políticas de backup, o problemas técnicos. Riesgo: pueden ser recuperados y explotados.

- Mitigación: métodos de eliminación segura (overwriting), gestión segura de backups, verificar eliminación completa tras el borrado.

### Para el examen

- **Shared responsibility model** vuelve a aparecer acá — es tema recurrente en toda la sección de cloud (ya visto en On-premise vs Cloud), reforzar que el cliente SIEMPRE es responsable de sus propios datos/apps sin importar el modelo.
- **Data remnants** conecta directamente con la lección de **Asset Disposal & Decommissioning** (sanitization/destruction con NIST 800-88) — mismo concepto aplicado ahora al contexto cloud.
- **Hypervisor protection / secure multi-tenancy** son términos técnicos específicos de virtualización cloud que pueden aparecer solos en el examen.
- **Network segmentation** para aislar VMs es la mitigación clave contra movimiento lateral — concepto que reaparece en varios contextos de seguridad de redes.
- Esta lista de 8 vulnerabilidades es un buen candidato a pregunta de matching ("¿qué vulnerabilidad describe este escenario?") — vale la pena memorizar cada una con su mitigación asociada, ya que el patrón "problema → solución" es muy típico de Security+.
- Conecta con la próxima lección de **Virtualization & Containerization**, que profundiza en VM escape y resource reuse (mencionadas en la intro de la sección).
## Virtualization & Containerization

**Virtualization**: emula servidores, cada uno corriendo su propio OS dentro de una VM. **Containerization**: alternativa liviana — encapsula una app dentro de su propio entorno operativo (contenedor), sin necesidad de un OS completo por unidad.

### Hypervisors — Type 1 vs Type 2

||**Type 1 (bare metal / native)**|**Type 2 (hosted)**|
|---|---|---|
|Cómo corre|Directo sobre el hardware, como un OS|Dentro de un OS estándar (Windows/Mac/Linux)|
|Ejemplos|Hyper-V, XenServer, VMware ESXi/vSphere|VirtualBox, VMware Workstation|
|Performance|**Más rápido/eficiente** — no gasta recursos corriendo un OS de escritorio completo primero|Más lento — corre sobre un OS host|

### Containerization

Ejecuta apps en espacios de usuario aislados ("contenedores") que comparten el **kernel del OS host**. Cada contenedor incluye la app + sus dependencias, pero comparte OS/binaries/libraries del host. Ventajas: eficiencia, velocidad, portabilidad, escalabilidad, aislamiento, consistencia. Tecnologías populares: **Docker, Kubernetes, Red Hat OpenShift**.

### Vulnerabilidades de virtualización

**VM Escape** Un atacante sale de una VM aislada e interactúa directamente con el **hypervisor** subyacente — desde ahí podría moverse a otras VMs en el mismo servidor físico. Técnica muy difícil de ejecutar, pero crítica. Mitigación: alojar VMs junto a otras del **mismo nivel de clasificación/red/segmento**.

**Privilege Escalation** Usuario se otorga permisos de nivel superior (root/admin) — en un hypervisor esto es catastrófico porque puede afectar a **todas las VMs invitadas** alojadas ahí. Ejemplo real citado: bug de VMware que permitía escalar privilegios en cualquier guest OS del hypervisor. Mitigación: mantenerse al día con hotfixes/service packs.

**Live Migration Attacks** Cuando una VM se mueve de un host físico a otro (**live migration**), un atacante posicionado entre ambos servidores puede hacer un **man-in-the-middle** y capturar datos **no encriptados** en tránsito.

**Resource Reuse** Recursos del sistema (memoria, storage, CPU) se reasignan a nuevas tareas sin borrarse/resetearse correctamente → info sensible de una tarea anterior puede exponerse a la nueva. Especialmente relevante en entornos multi-tenant como cloud, donde un usuario podría acceder a datos de otro.

**Container-specific risk** Todos los contenedores comparten **un solo OS** — si el atacante explota ese OS, **todas las apps** en ese OS quedan comprometidas.

**VM Sprawl** (mencionado al final) VMs creadas/desplegadas sin supervisión adecuada → se pierden de vista, sin patches ni updates correctos.

### Cómo asegurar las VMs

- Mantener OS y apps actualizados (igual que en un servidor físico normal)
- Antivirus + software firewall por VM, contraseñas fuertes
- **Patchear el hypervisor** (tipo 1, 2, o basado en contenedores) apenas salga un parche
- **Limitar conexiones** entre VMs y máquinas físicas (cables de red virtualizados, recursos compartidos) — una VM infectada debe quedar aislada; conexiones a shared resources (ej: file server de red) pueden romper el aislamiento
- Minimizar/eliminar features innecesarias → reduce superficie de ataque
- **Distribuir VMs entre varios servidores físicos** para evitar que una VM comprometida (consumiendo recursos excesivos) cause DoS a otras VMs del mismo host
- Vigilar **VM sprawl** — trackear y patchear todas las VMs activas
- **Encriptar los archivos que alojan las VMs** para proteger contra acceso no autorizado en el servidor

### Para el examen

- **Type 1 vs Type 2 hypervisor**: bare metal (directo en hardware, más rápido) vs hosted (corre dentro de un OS). Trampa clásica — memorizar los ejemplos específicos (Hyper-V/ESXi = tipo 1; VirtualBox = tipo 2).
- **VM Escape** es el término más "famoso" de esta lección y muy citado en el examen — memorizar que implica llegar al hypervisor, no solo a otra VM directamente.
- **Live migration attack = MITM en tránsito de datos no encriptados** — patrón de pregunta de escenario.
- **Resource reuse vulnerability** conecta con conceptos de **data remnants** (ya visto en cloud security y en asset disposal) — mismo problema conceptual (datos no borrados correctamente) aplicado a memoria/recursos compartidos en tiempo real.
- **Container = single shared OS = single point of compromise** — diferencia clave vs VMs (que tienen OS independientes) — candidato a pregunta de "¿por qué contenedores son más vulnerables a X que las VMs?".
- Esta lección profundiza justo en las vulnerabilidades que la intro de sección mencionó (VM escape, resource reuse) — buen momento para repasar la intro de Security Architecture junto con esta lección.

## Serverless Computing

**Serverless** ≠ sin servidores (sí hay servidores) — significa que la gestión de servidores, DBs y parte de la lógica de infraestructura se traslada al **cloud provider**. Los desarrolladores solo escriben las funciones/código de su aplicación.

Modelo base: **FaaS (Function as a Service)** — los devs escriben y despliegan funciones individuales que se **disparan por eventos** y solo corren cuando son necesarias (a diferencia del modelo tradicional donde la app corre continuamente en un servidor, sin importar la demanda).

### Ejemplos

- **AWS Lambda**: subís tu código, Lambda maneja provisioning, ejecución y auto-scaling según demanda.
- **Google Cloud Functions**: funciones simples de propósito único conectadas a eventos de servicios cloud — se disparan cuando ocurre el evento observado.

### Beneficios

- **Reducción de costos operativos**: pago solo por tiempo de cómputo consumido — cero costo cuando el código no corre.
- **Auto-scaling**: el proveedor escala precisamente según el tamaño de la carga de trabajo.
- **Foco en el producto**: los devs no gestionan servidores/runtimes (cloud u on-prem) → reduce time-to-market de nuevas apps.

### Riesgos / desafíos

- **Vendor lock-in**: servicios serverless suelen usar interfaces propietarias de un proveedor específico → dificulta cambiar de proveedor o usar multi-cloud, limita flexibilidad y puede aumentar costos a largo plazo.
- **Inmadurez de best practices**: campo relativamente nuevo — aunque prácticas tradicionales de desarrollo aplican, hay consideraciones únicas de serverless aún en evolución.

### Para el examen

- **Serverless no significa "sin servidores"** — es la trampa conceptual #1, memorizar que el nombre es engañoso: el proveedor gestiona los servidores, no que no existan.
- **FaaS (Function as a Service)** es el término técnico específico del modelo serverless — puede aparecer solo en el examen.
- **Vendor lock-in** conecta con conceptos ya vistos: multi-cloud (High Availability) se presenta como mitigación a este mismo problema — el examen puede preguntar "¿qué estrategia mitiga el vendor lock-in de serverless?" → multi-cloud architecture.
- **Modelo de costo "pay per execution"** vs el modelo tradicional "servidor corriendo 24/7" es un buen punto de comparación para preguntas de costo-eficiencia.
- Esta lección es corta y conceptual — conecta directamente con la próxima: **Microservices**, que profundiza en la arquitectura de aplicaciones descompuestas en servicios pequeños (frecuentemente desplegados de forma serverless).
## Microservices

**Microservices** = estilo arquitectónico que estructura una app como una colección de **servicios pequeños y autónomos**, modelados en torno a un dominio de negocio. Cada servicio corre un proceso único, se comunica vía un mecanismo ligero bien definido, y es **independiente** de los demás — opuesto a la **arquitectura monolítica** tradicional, donde todos los componentes están interconectados/interdependientes.

### Ejemplo real — Netflix

Empezó como app monolítica en los 2000s. Al crecer y sumar streaming de video, la arquitectura monolítica no soportaba la carga → caídas frecuentes del sistema completo. Solución: dividieron la app monolítica en microservicios independientes — uno para recomendaciones, otro para onboarding de clientes, otro para video encoding, etc.

### Ventajas

- **Scalability**: cada servicio escala independientemente según su propia demanda (útil cuando ciertos módulos reciben más tráfico que otros)
- **Flexibility**: cada microservicio puede escribirse en distinto lenguaje, usar distinta tecnología de storage, y ser gestionado por equipos diferentes — permite usar la mejor tecnología para cada caso
- **Resilience**: si un servicio falla, no tumba todo el sistema — el aislamiento reduce el riesgo de fallo sistémico
- **Deployment/update velocity**: cada servicio se despliega y actualiza independientemente → updates más rápidos y frecuentes, menor riesgo por deployment

### Desafíos

- **Complexity**: gestionar comunicación entre servicios, consistencia de datos, testing de sistemas distribuidos
- **Data management**: cada microservicio puede tener su propia DB → dificulta mantener consistencia de datos entre servicios
- **Network latency**: más comunicación entre servicios = más latencia = tiempos de respuesta más lentos
- **Security**: más servicios comunicándose por red = **mayor superficie de ataque**

### Para el examen

- **Microservices vs Monolithic** es la comparación central — memorizar bien: monolítico = todo interconectado/interdependiente, un fallo puede tumbar todo; microservices = independientes, aislados, un fallo no afecta al resto (resilience).
- **Trade-off clave**: más flexibilidad/resiliencia a cambio de más complejidad y **mayor superficie de ataque** — candidato a pregunta de "¿cuál es la desventaja de seguridad de microservices?".
- El caso **Netflix** es el ejemplo estándar de la industria para esta arquitectura — puede aparecer como ejemplo en preguntas de escenario.
- Esta lección conecta con **Serverless** (lección anterior) — microservices y serverless suelen combinarse en la práctica (cada microservicio desplegado como función serverless), aunque son conceptos distintos: microservices = cómo se estructura la app; serverless = cómo se gestiona/ejecuta la infraestructura.
- Conecta también hacia adelante con temas de **network architecture** (próxima lección según el overview) — la latencia y comunicación entre microservicios es justamente un tema de diseño de red.
## Network Architecture — Physical vs Logical Separation

**Network infrastructure** = hardware, software, servicios e instalaciones necesarias para soportar/gestionar/operar la red empresarial. Un aspecto crítico: la **separación de componentes**, lograda física o lógicamente.

### Physical Separation (Air Gapping)

Medida de seguridad que aísla un sistema **completamente** de otras redes (locales e internet) — elimina todas las conexiones directas/indirectas, incluyendo WiFi, Bluetooth y otras conexiones inalámbricas.

Ejemplos:

- Redes militares/gubernamentales con información clasificada
- **ICS** (Industrial Control Systems) en infraestructura crítica (plantas de energía, tratamiento de agua) — protegidos con air gap contra ataques cibernéticos/físicos que podrían causar daño en el mundo real

**Limitación importante**: los sistemas air-gapped **no son infalibles** — ataques sofisticados como **Stuxnet** demostraron que se pueden comprometer (típicamente vía medios físicos, como USB infectado). Aun así, sigue siendo uno de los métodos más seguros si se combina con medidas de seguridad física estrictas.

### Logical Separation

Crea límites **dentro** de una red para restringir acceso a ciertas áreas — se logra con firewalls, **VLANs** y otros dispositivos que controlan tráfico según reglas/políticas.

Ejemplos:

- **VLANs** en red corporativa: segregan tráfico entre departamentos (ej: HR no puede ver tráfico de Marketing aunque compartan la misma red física) — mejora seguridad, gestiona tráfico, reduce congestión
- **Screened subnet** (via firewalls): subred lógica que aloja servicios de cara al exterior, separada de la red interna — si un atacante compromete esa subred, no puede moverse fácilmente hacia la red interna

**Trade-off**: más flexible y fácil de implementar que la separación física, pero **menos segura** — si los firewalls/dispositivos de red están mal configurados, pueden ser explotados para acceso no autorizado.

### Para el examen

- **Air gap vs VLAN**: air gap = aislamiento físico total (más seguro, menos flexible); VLAN/logical = aislamiento vía configuración de red (más flexible, menos seguro si está mal configurado). Trampa clásica de comparación.
- **Stuxnet** es el ejemplo histórico estándar de que "air gap no es 100% seguro" — muy citado en el examen como caso de que un sistema air-gapped fue comprometido (típicamente por medios físicos/USB).
- **Screened subnet** es terminología moderna que reemplazó a "DMZ" en el vocabulario actualizado de CompTIA — recordar que es una subred lógica de servicios expuestos, separada de la red interna.
- **VLAN** ya es un término conocido de redes en general — acá se refuerza su uso específico como mecanismo de **logical separation** por motivos de seguridad, no solo de organización de tráfico.
- Esta lección es la base conceptual antes de profundizar en **SDN** (próxima lección del overview) — conecta directamente: SDN permite gestionar estas separaciones lógicas de forma programática y dinámica.
## Software-Defined Networking (SDN)

**SDN** = enfoque de gestión de red que permite configuración dinámica, programática y eficiente, con visión centralizada de toda la red. Reduce la complejidad de arquitecturas de red estáticas/inflexibles al **desacoplar las funciones de control y de reenvío (forwarding)** — el control de la red se vuelve directamente programable y la infraestructura subyacente se abstrae para apps y servicios de red.

### Los 3 planos de SDN
### Los 3 planos explicados fácil

1. **Application Plane (Las Reglas de Negocio):** Es la decisión de alto nivel.
    
    - _Ejemplo real:_ La app de mapas detecta un accidente y pide: _"Hay una ambulancia, denle prioridad hacia el hospital"_.
        
2. **Control Plane (El Cerebro Central):** Es el programa en la central que decide cómo ejecutar esa orden.
    
    - _Ejemplo real:_ Analiza todas las calles y calcula la ruta más rápida, cambiando las luces de los semáforos de esa ruta a verde.
        
3. **Data Plane (Los Músculos):** Son los dispositivos físicos (switches/routers) que solo obedecen y mueven los datos.
    
    - _Ejemplo real:_ El semáforo de la esquina que simplemente se pone en verde cuando la central se lo ordena y deja pasar los autos.
    - ### ¿Por qué existe SDN?

Antes, si querías cambiar una regla en una empresa con 100 routers, tenías que entrar a **cada uno de los 100 routers por separado** a escribir comandos. Con SDN, entras a **un solo software central**, cambias la regla, y este se la manda automáticamente a los 100 routers en un segundo.
### ¿Cómo funciona en la realidad?

El "superrouter" **no reemplaza las cajas físicas**, solo reemplaza su "inteligencia":

1. **Los routers físicos siguen existiendo** en cada piso o edificio para conectar los cables, pero ahora son equipos "bobos" y baratos (solo ejecutan el **Data Plane**).
    
2. **El software central (el "superrouter")** es un programa que corre en un servidor y les envía instrucciones digitalmente a todas esas cajas por la red (controla el **Control Plane**).


|Plano|Función|Analogía|
|---|---|---|
|**Data Plane** (forwarding plane)|Maneja los paquetes — los mueve de un lugar a otro, basado en protocolos (IP, Ethernet). Envía/recibe datos reales a través de switches/routers|El "músculo" — ejecuta el movimiento de datos|
|**Control Plane**|Decide **a dónde** se envía el tráfico|El "cerebro" — en SDN está **centralizado** (a diferencia de redes tradicionales donde cada router tiene su propio control plane)|
|**Application Plane**|Aloja las apps de red que interactúan con el controlador SDN, dándole instrucciones sobre qué hacer|La "voz" — le dice al controlador qué hacer, y el controlador manipula la red en consecuencia|
![[Pasted image 20260818105022.png]]
### Diferencia clave con redes tradicionales

En arquitecturas tradicionales, **cada router tiene su propio control plane** (descentralizado). En SDN, el control plane está **centralizado** en un controlador único que dicta el flujo de tráfico de toda la red — la hace más manejable y flexible.

### Ejemplos reales

- **Google B4 Project**: SDN para gestionar las redes de sus data centers — controla el flujo de datos para uso eficiente de ancho de banda a escala global.
- **AT&T Domain 2.0 Initiative**: transformar la red de AT&T en SDN para reducir costos y aumentar eficiencia — automatiza tareas de gestión de red, reduciendo intervención manual.

### Para el examen

- **Los 3 planos (data, control, application)** son el dato central de esta lección — memorizar función de cada uno. Trampa clásica: confundir data plane (mueve paquetes) con control plane (decide rutas).
- **Centralización del control plane** es la característica distintiva de SDN vs. networking tradicional — candidato fuerte a pregunta directa ("¿qué diferencia a SDN de una arquitectura de red tradicional?").
- **Desacoplamiento de control y forwarding** es la definición técnica formal de SDN que puede aparecer textual en el examen.
- Conecta con la lección anterior (Network Architecture — physical/logical separation): SDN es la tecnología que permite implementar separación lógica (como VLANs) de forma **programática y centralizada**, en vez de configurar cada dispositivo individualmente.
- Los ejemplos de Google/AT&T son ilustrativos — no hace falta memorizar detalles de "B4" o "Domain 2.0" en sí, solo entender que son casos reales de SDN a gran escala.
## Infrastructure as Code (IaC)

**IaC** = automatizar el aprovisionamiento y gestión de recursos de IT mediante archivos de definición legibles por máquina / scripts, en vez de configuración manual de hardware o herramientas interactivas. Práctica clave del movimiento **DevOps**, usada frecuentemente junto con cloud computing.

La infraestructura se define en archivos de código (versionables, testeables, auditables) usando lenguajes de alto nivel como **YAML, JSON**, o DSLs como **HCL** (HashiCorp Configuration Language — usado por Terraform).

### Idempotencia — concepto central

Capacidad de una operación de producir **el mismo resultado sin importar cuántas veces se ejecute**. Un script idempotente crea infraestructura idéntica cada vez, sin importar el estado inicial — crucial para mantener consistencia y confiabilidad entre entornos.

### Objetivo principal: eliminar "snowflake systems"

Un **snowflake** = configuración/construcción diferente a cualquier otra — carece de consistencia y puede introducir riesgo. IaC busca eliminar esta inconsistencia mediante configuraciones reproducibles y estandarizadas.

### Ventajas

- **Speed & efficiency**: aprovisionamiento/desaprovisionamiento rápido de recursos
- **Consistency & standardization**: todos los entornos configurados de la misma forma → menos errores/inconsistencias
- **Scalability**: fácil replicar configuraciones al escalar operaciones
- **Cost savings**: automatización reduce tiempo/recursos en troubleshooting manual de configuración
- **Auditability & compliance**: al estar en código, se puede versionar y auditar — facilita tracking de cambios y compliance

### Desafíos

- **Learning curve**: requiere nuevo skillset y cambio de mentalidad — los equipos deben aprender a escribir/testear/mantener IaC
- **Complexity**: el código puede volverse complejo a medida que crece la infraestructura — se mitiga con modularización y buena documentación
- **Security risks**: mal gestionado, puede exponer datos sensibles en archivos de código o introducir configuraciones inseguras sin querer

### Para el examen

- **Idempotencia** es el término técnico más importante de esta lección — memorizar la definición exacta: mismo resultado sin importar cuántas veces se ejecute la operación. Muy propenso a pregunta directa de definición.
- **Snowflake system** es terminología específica de IaC que puede aparecer como distractor o en pregunta de "¿qué problema busca resolver IaC?" → eliminar snowflakes/inconsistencia de configuración.
- **HCL** (HashiCorp Configuration Language) — asociarlo con Terraform si te suena de otro contexto; es un lenguaje específico de IaC que puede aparecer nombrado.
- **Security risk de IaC**: datos sensibles expuestos en archivos de código (ej: credenciales hardcodeadas en un script) — conecta con buenas prácticas de secret management, tema recurrente en Security+.
- Esta lección conecta con **SDN** (lección anterior) — ambas comparten la filosofía de "todo programable/automatizado en vez de configuración manual", aplicado a redes (SDN) vs. infraestructura completa (IaC).
- Según el overview de la sección, las próximas lecciones cubren **Centralized vs Decentralized architectures**, luego **IoT**, **ICS/SCADA**, y **Embedded systems** — cerrando el bloque de Security Architecture.

## Centralized vs. Decentralized Architectures — resumen para Security+

### Centralized Architecture

Sistema donde todas las funciones de procesamiento, datos y aplicaciones se gestionan desde una única ubicación o autoridad (ej: mainframe, servidor central o data center).

  

- **Ventajas:**
    
      
    - **Efficiency and Control:** Gestión unificada, mantenimiento sencillo y asignación eficiente de recursos.
        
          
        
    - **Consistency:** Alta coherencia de datos al estar centralizados en un único punto (_single source of truth_).
        
          
        
    - **Cost-Effectiveness:** Menores costos de infraestructura y mantenimiento general.
        
          
        
- **Riesgos:**
    
      
    - **Single Point of Failure (SPOF):** Si cae el servidor central, se interrumpe todo el sistema (_downtime_ total).
        
          
        
    - **Scalability Bottlenecks:** Dificultad para absorber incrementos de carga a medida que crece la organización.
        
          
        
    - **Security Risks:** Objetivo muy atractivo para atacantes; si cae el nodo central, se compromete toda la información.
        
          
        

### Decentralized Architecture

Distribuición de las funciones del sistema entre múltiples nodos o ubicaciones independientes, sin una autoridad ni control centralizado único.

  

- **Ventajas:**
    
      
    - **Resilience:** Alta tolerancia a fallos; la caída de un nodo no detiene la operación global.
        
          
        
    - **Scalability:** Facilidad para escalar horizontalmente agregando nuevos nodos según la demanda.
        
          
        
    - **Flexibility:** Soporta de manera natural entornos de trabajo remoto y equipos distribuidos.
        
          
        
- **Riesgos:**
    
      
    - **Security Risks:** Mayor superficie de ataque (_attack surface_); cada nodo o conexión remota es un punto de entrada potencial.
        
          
        
    - **Management Complexity:** Dificultad para coordinar, monitorear y mantener múltiples nodos en paralelo.
        
          
        
    - **Data Inconsistency:** Desafíos de sincronización que pueden generar inconsistencia en los datos entre nodos.
        
          
        

### Centralized vs. Decentralized

|**Criterio**|**Centralized Architecture**|**Decentralized Architecture**|
|---|---|---|
|**Control**|Alto y unificado|Distribuido entre nodos|
|**Punto de fallo**|**Single Point of Failure (SPOF)**|Resiliente (sin SPOF único)|
|**Superficie de ataque**|Concentrada en el núcleo|Amplia (múltiples puntos de entrada)|
|**Consistencia de datos**|Alta y garantizada|Desafíos de sincronización|
|**Escalabilidad**|Limitada (vertical)|Flexible (horizontal)|

### Para el examen

- **Términos a memorizar:** **Single Point of Failure (SPOF)** asociado inmediatamente a _Centralized_, y **Attack Surface / Multi-node Resilience** asociado a _Decentralized_.
    
      
    
- **Lo que suelen preguntar:** Escenarios donde piden elegir la arquitectura según la prioridad de la organización.
    
      
    - Si la prioridad es **control, consistencia e infraestructura simple** $\rightarrow$ **Centralized**.
        
          
        
    - Si la prioridad es **resiliencia, escalabilidad y soporte remoto** $\rightarrow$ **Decentralized**.
        
          
        
- **Trampa común:** Confundir la seguridad de ambos. La arquitectura _centralizada_ impacta más gravemente si la vulneran (**High Impact**), pero la _descentralizada_ presenta un riesgo con más puntos de entrada vulnerables (**High Likelihood / Extended Attack Surface**).
## Internet of Things (IoT) — resumen para Security+

### Componentes clave del ecosistema IoT

Dispositivos físicos integrados con sensores, software y conectividad para intercambiar datos de forma autónoma.

  

- **Hub / Control System:** Punto central que conecta, procesa, analiza y envía órdenes a los dispositivos (ej: Amazon Echo, Google Home, apps móviles).
    
      
    
- **Smart Devices:** Objetos cotidianos con capacidad de procesamiento (ej: frigoríficos inteligentes, robots industriales, drones agrícolas).
    
      
    
- **Wearables:** Dispositivos inteligentes corporales para monitoreo de métricas o experiencias inmersivas (ej: smartwatches, fitness bands, VR/AR headsets).
    
      
    
- **Sensors:** Componentes que detectan variables del entorno (temperatura, movimiento, humedad, presión) y las convierten en datos procesables.
    
      
    

### Riesgos y amenazas

- **Weak Default Configurations:** Uso de credenciales por defecto (ej: `admin/admin`) ampliamente conocidas y fáciles de vulnerar.
    
      
    
- **Misconfigured Network Services:** Puertos abiertos, servicios innecesarios o tráfico sin cifrar que amplían la **Attack Surface**.
    
      
    
- **Mitigación clave:** Segmentar la red ubicando los dispositivos IoT en una **VLAN separada** (_Network Segmentation_) para aislar un eventual compromiso.
    
      
    

### Para el examen

- **Términos a memorizar:** **Default Credentials**, **Attack Surface**, **Network Segmentation / Isolated VLAN**.
    
      
    
- **Lo que suelen preguntar:** Escenarios sobre cómo asegurar dispositivos IoT en una red corporativa o del hogar. La respuesta correcta casi siempre implica cambiar credenciales por defecto y **segmentar la red**.
    
      
    
- **Trampa común:** Pensar que parchear o instalar antivirus en el dispositivo es la solución principal; la mayoría de los dispositivos IoT no permiten instalar software de seguridad, por lo que la protección recae en la **segmentación de red**.
## ICS and SCADA Systems — resumen para Security+

### Conceptos clave de automatización industrial

Sistemas utilizados en infraestructuras críticas (electricidad, agua, petróleo, gas, manufactura). Originalmente diseñados para operar en entornos aislados (_air-gapped_), hoy están más expuestos a ciberamenazas por la digitalización.

  

- **Industrial Control Systems (ICS):** Categoría general de sistemas de control industrial para monitorear y gestionar procesos de producción. Incluye:
    
      
    - **Distributed Control Systems (DCS):** Controlan procesos de producción complejos dentro de una misma ubicación geográfica/planta.
        
          
        
    - **Programmable Logic Controllers (PLC):** Dispositivos de hardware para automatizar tareas y procesos específicos (ej: líneas de montaje, robótica).
        
          
        
- **Supervisory Control and Data Acquisition (SCADA):** Tipo de ICS diseñado para monitorear y controlar procesos industriales **geográficamente dispersos** a gran escala (ej: red eléctrica nacional, oleoductos).
    
      
    

### Vulnerabilidades y mitigaciones

- **Vulnerabilidades:**
    
      
    - **Legacy Software / Lack of Updates:** Sistemas obsoletos sin parches de seguridad por temor a interrumpir la operación.
        
          
        
    - **Unauthorized Access / Malware Attacks:** Infiltración en la red operativa (OT) que permite manipulación directa de hardware.
        
          
        
    - **Physical Threats:** Daños o manipulación directa del hardware en sitios remotos.
        
          
        
- **Mitigaciones clave:**
    
      
    - **Network Segmentation / Air-Gapping:** Separar la red corporativa (IT) de la red industrial (OT).
        
          
        
    - **Strict Access Control & MFA:** Autenticación fuerte y principio de menor privilegio (_least privilege_).
        
          
        
    - **Firewalls & IDS / Patching:** Uso de firewalls industriales, monitoreo activo de intrusiones y parches regulares cuando sea factible.
        
          
        

### ICS vs. SCADA

|**Criterio**|**Industrial Control Systems (ICS) / DCS**|**SCADA**|
|---|---|---|
|**Alcance geográfico**|Local (una sola planta o instalación)|Extenso / Disperso geográficamente|
|**Casos de uso**|Fábricas, líneas de ensamblaje, refinerías|Redes eléctricas, distribución de agua, oleoductos|
|**Componentes básicos**|**PLC**, unidades de control local|Estaciones maestras, RTUs (_Remote Terminal Units_), PLCs|

### Para el examen

- **Términos a memorizar:** **Operational Technology (OT)**, **SCADA** (sistemas dispersos), **PLC** (control de hardware específico), **Air-Gap** (aislamiento físico/lógico).
    
      
    
- **Lo que suelen preguntar:** La distinción clave según el alcance geográfico (SCADA para áreas extensas/distribuidas; DCS/PLC para plantas locales) y la necesidad de aislar entornos de **OT** (_Operational Technology_) de **IT** mediante **Air-Gapping** o segmentación.
    
      
    
- **Trampa común:** Asumir que la solución inmediata para un sistema SCADA o ICS es actualizar el sistema operativo o aplicar parches masivos; en entornos industriales, el parcheo puede detener servicios críticos, por lo que la **segmentación de red** y el **control de acceso físico/lógico** suelen priorizarse en las preguntas de examen.
## Embedded Systems — resumen para Security+

### Embedded Systems & RTOS

Sistemas informáticos especializados diseñados para realizar funciones dedicadas dentro de estructuras mecánicas o eléctricas más grandes (ej: electrónica de consumo, automoción, dispositivos médicos).

  

- **Real-Time Operating System (RTOS):** Sistema operativo optimizado para procesar datos en tiempo real sin retrasos en búfer (_unbuffered delay_), garantizando una ejecución puntual y predecible. Crucial para aplicaciones sensibles al tiempo (ej: navegación aérea, dispositivos médicos de soporte vital).
    
      
    

### Riesgos y vulnerabilidades

- **Hardware Failures & Software Bugs:** Entornos operativos hostiles e imperfecciones de código que comprometen la disponibilidad o seguridad.
    
      
    
- **Legacy Systems:** Larga vida útil que deriva en hardware y software obsoletos, aumentando el riesgo de explotación.
    
      
    
- **Inability to Patch:** Dificultad para aplicar parches debido a requerimientos de alto _uptime_, inaccesibilidad física o falta de mecanismos nativos de actualización.
    
      
    

### Estrategias de mitigación

- **Network Segmentation:** Dividir la red en subredes para limitar el movimiento lateral de un atacante y aislar dispositivos comprometidos.
    
      
    
- **Wrappers (ej: IPsec):** Encapsulamiento de tráfico para proteger los datos en tránsito entre puntos no confiables, ocultando el contenido mediante cabeceras seguras.
    
      
    
- **Firmware Code Control & Secure Boot:** Uso de firmas criptográficas y mecanismos de _Secure Boot_ para asegurar que solo se ejecute firmware verificado y autorizado.
    
      
    
- **Over-the-Air (OTA) Updates:** Mecanismos de actualización remota planificados para solventar la falta de acceso físico, requiriendo canales seguros para evitar la inyección de malware.
    
      
    

### Embedded Systems vs. RTOS

|**Criterio**|**Embedded System**|**Real-Time Operating System (RTOS)**|
|---|---|---|
|**Definición**|Dispositivo/hardware completo diseñado para una función específica|Sistema operativo que gestiona los recursos de hardware con tiempos de respuesta garantizados|
|**Enfoque principal**|Integración hardware/software dedicada|Procesamiento de datos sin latencia ni retrasos de búfer|
|**Ejemplo**|Pacemaker, cámara digital, ECU de automóvil|OS de sistemas de aviación, OS de bombas de insulina|

### Para el examen

- **Términos a memorizar:** **RTOS**, **Secure Boot**, **Network Segmentation**, **Over-the-Air (OTA) Updates**, **Wrappers (IPsec)**.
    
      
    
- **Lo que suelen preguntar:**
    
      
    - Escenarios sobre dispositivos médicos o automotrices que requieren tiempos de respuesta exactos sin latencia $\rightarrow$ **RTOS**.
        
          
        
    - Cómo proteger un dispositivo _embedded_ que no se puede parchear fácilmente $\rightarrow$ **Network Segmentation** o uso de **Wrappers**.
        
          
        
    - Cómo garantizar la integridad del código al encender el dispositivo $\rightarrow$ **Secure Boot** mediante firmas digitales en el firmware.
        
          
        
- **Trampa común:** Asumir que la mejor opción frente a un fallo de seguridad en un sistema _embedded_ es aplicar un parche inmediato; en la práctica y en el examen, muchos de estos sistemas **no se pueden parchear fácilmente** (_Inability to patch_), por lo que las medidas de compensación (como la **segmentación de red**) son la respuesta correcta.