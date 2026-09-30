**Cyber Resilience & Redundancy** (Domain 3, objetivo 3.4). Resumen corto:

Objetivo **3.4**: explicar la importancia de resiliencia y recuperación en la arquitectura de seguridad.

- **Cyber resilience**: capacidad de una entidad de seguir entregando el resultado esperado a pesar de eventos cibernéticos adversos.
- **Redundancy**: tener sistemas/equipos/procesos adicionales que garanticen continuidad si los principales fallan.

Temas que van a ver en las próximas lecciones:

- **High Availability**: load balancing vs clustering; redundancia de power, conexiones, servidores, servicios, proveedores; ventajas de **multi-cloud systems**
- **Data redundancy**: dispositivos de almacenamiento redundantes trabajando juntos — distintos tipos de **RAID arrays**
- Demo práctica de configuración de RAID
- **Capacity planning**: personal, tecnología, infraestructura, y capacidad de escalar en picos de demanda
- **Powering data centers**: generadores, **UPS**, line conditioners, power distribution centers
- **Data backups**: on-site vs off-site, encriptación, snapshots, recovery, replication, journaling
- **BCDR** (Business Continuity/Disaster Recovery) plan: mantener operaciones ante eventos imprevistos
- **Redundant sites**: hot site, cold site, warm site, dispersión geográfica, cloud virtual sites, platform diversity
- **Testing de resiliencia y recuperación**: tabletop exercises, failover techniques, simulation, parallel processing

**Para el examen:** todavía nada específico para memorizar, pero ya podés anotar los términos clave que vienen: **RAID, UPS, BCDR, hot/warm/cold site, load balancing/clustering, failover**. La próxima lección con contenido real ya entra en High Availability.


## High Availability

**High availability (HA)** = capacidad de un servicio de estar continuamente disponible, minimizando downtime al mínimo posible. Se logra combinando: load balancing/clustering, redundancy, y (en cloud) multi-cloud.

### Uptime & "The Nines"

Disponibilidad se mide como **uptime %**.

|Nivel|% Uptime|Downtime máximo/año|
|---|---|---|
|**5 nines** (estándar de oro)|99.999%|~5 minutos|
|**6 nines** (algunos cloud providers)|99.9999%|~31 segundos|

La mayoría de las organizaciones necesitan más downtime que eso para mantenimiento (patches, reemplazo de discos, nuevos routers) — de ahí la importancia de diseñar arquitectura que permita mantenimiento sin perder disponibilidad general.

### Load Balancing vs Clustering ⚠️ (par clásico de examen)

||**Load Balancing**|**Clustering**|
|---|---|---|
|Qué hace|Distribuye carga de trabajo entre múltiples recursos para optimizar uso, maximizar performance, evitar sobrecarga de un solo recurso|Múltiples computadoras/storage/conexiones de red trabajando juntas como **un solo sistema** para dar mayor disponibilidad, confiabilidad y escalabilidad|
|Enfoque|Gestión de **tráfico/carga excesiva**|Mantener la app disponible ante **fallo de hardware** — elimina single points of failure|
|Analogía|Tráfico entra al load balancer → redirige a uno de varios servidores|Redundancia que "toma el relevo" si un componente/sistema falla|

Ambos pueden combinarse: load balancing gestiona condiciones normales, clustering entra en acción si hay fallo de componente/sistema.

### Redundancy

Duplicación de componentes/funciones críticas para aumentar confiabilidad. Áreas clave:

- **Power**: doble fuente de alimentación en el servidor, o **UPS**, generador de respaldo, o conexión a 2+ redes eléctricas
- **Connections**: múltiples conexiones cableadas, o cableada + inalámbrica
- **Servers/Services**: configurados en load-balanced o clustered architecture — copia de respaldo disponible si un servidor falla catastróficamente
- **Vendors**: usar 2+ proveedores para un servicio crítico (ej: DionTraining usa Stripe como principal + un procesador secundario de respaldo)
- Ejemplo clásico: 2 **domain controllers** (primary + secondary) — se puede reiniciar uno para patchear mientras el otro sigue sirviendo a los usuarios

Nota práctica: full hardware redundancy en todo es caro — hay que evaluar caso por caso si conviene redundancia por hardware o si alcanza con software/cloud.

### Multi-cloud

Distribuir datos/apps/servicios en varios cloud providers — mitiga single point of failure (si un proveedor cae, se transfiere la carga a otro). Beneficios adicionales: flexibilidad para escalar, optimización de costos (usar el proveedor más barato como principal, otro como backup), evitar **vendor lock-in** (mejor poder de negociación, facilidad de migrar). Requisito: mantener **data management, unified threat management y policy enforcement consistentes** en todos los entornos cloud para no perder seguridad/compliance.

### Para el examen

- **Load balancing vs Clustering** es la trampa #1 de esta lección: load balancing = distribuir tráfico/carga; clustering = eliminar single point of failure / mantener disponibilidad ante fallo de hardware. Suelen dar un escenario y preguntar cuál aplica.
- **Five nines (99.999%) = ~5 min/año de downtime** — número que puede pedirse textual en el examen. Six nines = 31 segundos/año.
- **Redundancy areas**: power, connections, servers, services, vendors — memorizar esta lista, es candidata a pregunta de "qué tipo de redundancia es X escenario".
- **UPS** ya había aparecido en la intro de sección — ahora tiene contexto: parte de power redundancy.
- **Multi-cloud vs vendor lock-in**: recordar que multi-cloud es también una estrategia de negociación/costos, no solo de resiliencia técnica — puede aparecer como distractor o como parte de la respuesta correcta en preguntas sobre "por qué usar multi-cloud".
## Data Redundancy — RAID

**RAID** = Redundant Array of Independent Disks — combina múltiples dispositivos de storage físico en un único dispositivo de storage lógico reconocido por el OS.

### Tipos de RAID (tabla comparativa)

|RAID|Keyword|Mín. discos|Tolerancia a fallos|Cómo funciona|
|---|---|---|---|---|
|**RAID 0**|Striping|2|**Ninguna**|Divide datos entre discos → mayor performance, cero redundancia|
|**RAID 1**|Mirroring|2|Pierde 1 disco|Copia idéntica en ambos discos → menor downtime posible|
|**RAID 5**|Striping con paridad|3|Pierde 1 disco|Datos + paridad distribuidos entre discos; reconstruye lo faltante (más lento durante rebuild)|
|**RAID 6**|Striping con doble paridad|4|Pierde 2 discos|Como RAID 5 pero con paridad duplicada|
|**RAID 10**|Striped array de mirrored arrays|4 (par)|Pierde hasta 2 discos*|RAID 1 + RAID 0 combinados|

*RAID 10: solo tolera perder 2 discos si **no son del mismo mirror set** — si pierde uno de cada par duplicado, sigue funcionando pero pierde su fault tolerance hasta reemplazar los discos.

### Detalles clave por tipo

- **RAID 0**: solo para performance (ej: edición de video de alta gama) — sin redundancia, no usar si te importa la data.
- **RAID 1**: mejor tolerancia con solo 2 discos, pero da solo 1 unidad lógica.
- **RAID 5**: si falla un disco, se puede hacer **hot-swap** (reemplazar el disco fallado mientras el servidor sigue corriendo) y el sistema reconstruye los datos usando striping + paridad.
- **RAID 6**: mejora de RAID 5 — doble paridad = soporta 2 fallos simultáneos.
- **RAID 10**: combina velocidad (striping) + redundancia (mirroring).

### Clasificación por nivel de resiliencia

|Término|Significado|RAIDs que aplican|
|---|---|---|
|**Fault resistant**|Soporta fallos de hardware sin perder datos, vía mirroring|RAID 1, RAID 10|
|**Fault tolerant**|Operación continua sin downtime; reconstruye datos rápido desde discos sanos (mirroring o striping con paridad)|RAID 1, 5, 6, 10|
|**Disaster tolerant**|Protección más amplia — dos zonas independientes con acceso completo a todos los datos|RAID 1, RAID 10|

### Para el examen

- **Memorizar la tabla completa de RAIDs** (keyword, mínimo de discos, tolerancia) es esencial — muy propenso a pregunta directa tipo "¿qué RAID necesita mínimo 4 discos y tolera 2 fallos?" → RAID 6.
- **RAID 0 = sin redundancia** es la trampa más común — la gente asume que "RAID" siempre implica redundancia, pero RAID 0 es la excepción.
- **Fault resistant vs Fault tolerant vs Disaster tolerant**: jerarquía de resiliencia creciente. Fault resistant = solo mirroring; fault tolerant = mirroring O parity; disaster tolerant = doble zona con full data access (solo RAID 1 y 10 llegan a este nivel). Trampa clásica de examen — memorizar qué RAIDs caen en cada categoría.
- **Hot-swap** (mencionado en RAID 5) es un término técnico que puede aparecer solo en el examen — reemplazar un disco fallado sin apagar el servidor.
- **RAID 10 = RAID 1 + RAID 0** — la lógica del nombre (mirror primero, luego stripe) puede ser la base de una pregunta conceptual.
## Capacity Planning

**Capacity planning** = esfuerzo de planificación estratégica para asegurar que la organización esté equipada para satisfacer demanda futura a tiempo y de forma costo-efectiva. Cuatro áreas clave: **people, technology, infrastructure, process**.

### People

Analizar skills y capacidad actual del personal, prever necesidades futuras (hiring, training, downsizing). Ejemplo: retail contrata 20 empleados temporales/part-time desde el 1° de octubre para la temporada navideña (nov-dic), y los da de baja después del 1° de enero cuando baja la demanda — minimiza costos operativos fuera de temporada alta.

### Technology

Conocer los recursos tecnológicos actuales (software/hardware), su ritmo de uso, y demanda futura potencial. Evaluar si la tecnología actual soporta el crecimiento esperado o si hace falta invertir en nuevas soluciones. Ejemplo: e-commerce necesita saber cuántos usuarios concurrentes soporta su plataforma antes de fallar — servicios cloud permiten **escalar recursos hacia arriba en demanda alta y reducirlos después** para bajar costos operativos.

### Infrastructure

Planificación de espacio físico y utilities (oficinas, warehouses, plantas, data centers). Ejemplo del video: centro de operaciones de red para el gobierno de EE.UU. en Medio Oriente — espacio de rack limitado, tenían que calcular consumo de energía y generación de calor por servidor, y rechazaron instalaciones que podían alojarse en la nube para priorizar servicios que necesitaban **baja latencia** local.

### Process

Optimizar procesos de negocio para soportar fluctuaciones de demanda (subidas o bajadas) — vía streamlining de workflows, automatización, o outsourcing. Ejemplo: call center que necesita crear 500 cuentas de usuario para empleados temporales de temporada navideña — hacerlo manualmente sería un cuello de botella de días; automatizarlo permite crear las 500 cuentas simultáneamente.

### Ejemplo integrador — Telemedicina en proveedor de salud

- **People**: entrenar personal existente en protocolos de telemedicina, contratar personal con experiencia en atención remota
- **Technology**: invertir en plataforma de telemedicina segura y compliant, escalar sistemas para más tráfico y storage de datos de salud
- **Infrastructure**: adaptar espacios físicos para consultas remotas (áreas privadas y silenciosas)
- **Process**: nuevos workflows para scheduling/realización/seguimiento de citas virtuales, procedimientos de manejo de datos de pacientes conforme a normativa de privacidad

### Para el examen

- **Las 4 áreas (people, technology, infrastructure, process)** son el dato central de esta lección — memorizar la lista completa, es candidato fuerte a pregunta de matching/escenario ("¿qué área de capacity planning es X ejemplo?").
- **Cloud scaling** (subir/bajar recursos según demanda) es el punto técnico más accionable — conecta con conceptos de elasticidad cloud vistos en otras partes del curso.
- El ejemplo de telemedicina es útil como plantilla mental para reconocer las 4 áreas en un escenario nuevo del examen.
- Capacity planning se conecta con la lección anterior de **High Availability** — ambas buscan que el sistema aguante demanda sin degradarse, pero capacity planning es más estratégico/de largo plazo (personal, presupuesto, espacio) mientras HA es más técnico/arquitectónico (load balancing, clustering, redundancia).
## Powering Data Centers

### Las 5 condiciones de energía

|Condición|Qué es|Ejemplo (base 120V EE.UU.)|
|---|---|---|
|**Surge**|Aumento pequeño e inesperado de voltaje|120V → 125-130V|
|**Spike**|Aumento transitorio corto y fuerte (causado por cortocircuito, breaker, apagón, rayo)|120V → 150-175V+|
|**Sag**|Disminución pequeña e inesperada de voltaje (opuesto de surge)|120V → 115-117V (sistemas siguen online, pero puede dañar hardware con el tiempo)|
|**Undervoltage event** (antes llamado _brownout_)|Caída de voltaje más profunda y sostenida|120V → 70-80V (sistemas se apagan, voltaje insuficiente)|
|**Power loss event** (blackout)|Pérdida total de energía por un período|Ej: se va la luz en casa por 1-2 min. **Importante**: al restaurarse, puede venir un spike que dañe sistemas|

### Sistemas de protección

**Line conditioner** Corrige fluctuaciones pequeñas (surge, sag, undervoltage moderado) para entregar energía limpia y estable. **No** protege contra pérdida total de energía.

**UPS (Uninterruptible Power Supply)** Provee energía de emergencia cuando falla la fuente normal. También hace line conditioning + batería de respaldo. Típicamente da solo **15-60 minutos** de energía — diseñado para prevenir pérdida de datos/daño de hardware en cortes cortos, **no** para cortes largos.

**Generator** Convierte energía mecánica en eléctrica (inducción electromagnética) para suministro de emergencia prolongado. 3 tipos:

|Tipo|Características|
|---|---|
|**Portable gas-powered**|Más barato, pequeño/portátil, ruidoso, requiere más mantenimiento, potencia limitada (no alcanza para todo un edificio)|
|**Permanently installed**|Diesel/propano/gas natural — backbone de la arquitectura de emergencia de un data center; puede alimentar todo un edificio por horas/días/semanas; se activa automáticamente|
|**Battery inverter**|Baterías (plomo-ácido, NiCd, litio) — más silencioso, menos mantenimiento, pero solo potencia mínima/corta duración — sirve para cubrir el gap hasta que arranque un generador de largo plazo|

**PDC (Power Distribution Center)** Hub centralizado que recibe energía y la distribuye por todo el data center. No es solo una "regleta gigante" — incluye protección de circuitos, monitoreo, load balancing. Se integra con UPS grandes y generadores permanentes para transición fluida en un power loss event.

Un **Power Distribution Center** (o PDU/tablero) no produce energía ni tiene baterías; es el "enchufe gigante inteligente" que toma la electricidad de la pared (venga de la red eléctrica, del UPS o del generador) y la reparte de forma limpia y organizada a cada servidor del rack. Si la luz de la calle se corta y no hay generador, el PDC se apaga al instante.
### Diseño típico de un data center grande

- **Rack-mounted UPS**: line conditioning + battery backup por rack de servidores (~10-15 min de autonomía)
- **PDUs** (Power Distribution Units): line conditioning + load balancing por rack, desde la fuente principal
- **Generadores de respaldo**: tardan **30-60 segundos** en arrancar y alcanzar velocidad máxima antes de poder alimentar los PDCs

### Para el examen

- **Las 5 condiciones de energía en orden de severidad** (surge < spike < sag < undervoltage event < power loss event) son el dato central — memorizar bien la diferencia entre **surge (leve, sube) vs spike (fuerte y corto, sube)** y **sag (leve, baja) vs undervoltage event (fuerte y sostenido, baja)**. Trampa clásica de examen.
- **UPS ≠ generador**: UPS = minutos de autonomía para cortes cortos / cubrir el gap hasta que arranque el generador; generador = horas/días para cortes largos. Suelen preguntar "¿qué usarías para un corte de 3 horas?" → generador, no UPS.
- **Dato numérico clave**: UPS da 15-60 min; generadores tardan 30-60 segundos en arrancar — candidatos a pregunta directa de tiempos.
- **PDC/PDU**: recordar que no es solo distribución pasiva — incluye circuit protection, monitoring, load balancing.
- Esta lección conecta con **High Availability** (power redundancy) y con la próxima de **backups** — todas construyen hacia el objetivo 3.4 de resiliencia.
## Data Backups

Regla de oro: nunca guardar todos los datos en un solo lugar. **Backup** = crear copias duplicadas de información digital para protegerla de pérdida, corrupción o indisponibilidad.

### On-site vs Off-site backups

||**On-site**|**Off-site**|
|---|---|---|
|Ubicación|Físicamente en tu propio centro de datos/oficina|Geográficamente separado de la fuente de datos|
|Ventaja|Restauración rápida y cómoda|Protege contra desastres físicos (incendio, inundación, ataque)|
|Riesgo|Vulnerable si el desastre afecta el edificio entero|—|
|Cómo se implementa hoy|Disco externo, cinta local|Cloud servers, o transporte físico de cintas/discos a instalación remota|

Ejemplo del video: SOC militar cerca de zona de misiles — backup nocturno on-site en cintas + envío semanal de copias a instalación a ~1000km. Hoy se haría vía conexión de alta velocidad en vez de enviar cintas físicamente.

### Frecuencia de backups — RPO

Pregunta clave: **¿cuántos datos estás dispuesto a perder?** Eso define tu **RPO (Recovery Point Objective)**.

- RPO de 1 hora → backups al menos cada hora
- RPO de 2 días → backups diarios/nocturnos son suficientes
- También considerar qué tan rápido cambian los datos: cambios diarios → backup diario; cambios frecuentes → backup cada 30-45 min
- Balance entre necesidad de datos actualizados vs. recursos (tiempo, storage, ancho de banda, costo)

### Encryption

Backups deben usar:

- **Encryption at rest**: cifra los datos al escribirlos en el dispositivo de storage
- **Encryption in transit**: protege los datos mientras se mueven hacia/desde el destino del backup

### Snapshots

	Copias puntuales que capturan un estado consistente — "foto congelada" de los datos. A diferencia del backup tradicional (copia todo), la snapshot **solo registra cambios desde la snapshot anterior** → más eficiente en storage y velocidad, permite capturas más frecuentes. Ideal para sistemas donde la consistencia es crítica (DBs, file servers) — permite volver a un "known good state" ante corrupción o borrado accidental.
**"point-in-time"** (un instante específico en el tiempo).
Un _snapshot_ es como tomar una "fotografía" del estado exacto del sistema en un segundo preciso para poder restaurarlo a ese momento si algo falla.

### Data Recovery

Objetivo final de toda estrategia de backup. Pasos clave:

1. **Seleccionar el backup adecuado** (snapshot, off-site, on-site según el caso)
2. **Iniciar la recuperación**: acceder al storage y comenzar la restauración
3. **Validar los datos**: verificar integridad/consistencia — que coincidan con el estado esperado
4. **Testear** el proceso completo regularmente (identifica cuellos de botella antes de un desastre real)
5. **Documentar y reportar**: registros detallados para análisis post-incidente y compliance
6. **Notificar** a stakeholders (IT, management, usuarios) según la naturaleza del incidente

**Práctica recomendada**: probar el data recovery al menos **una vez al mes**.

### Replication

Copias de datos en **tiempo real o casi real** (no periódicas como el backup) — mantiene los datos en 2+ lugares simultáneamente. Si un servidor falla, el otro sigue sin interrupción. Ideal para entornos de alta disponibilidad que no toleran downtime. Ventaja principal: continuidad de datos.

### Journaling

También llamado change tracking/logging — registro meticuloso de cada cambio en los datos a lo largo del tiempo. Permite recuperación granular a un punto específico en el tiempo. Muy útil en procedimientos legales o auditorías de compliance — permite rastrear y revertir cambios individuales manteniendo un audit trail detallado. Requiere atención a: granularidad de tracking, gestión de tamaño/retención del journal, y seguridad contra tampering.

### Para el examen

- **Snapshot vs traditional backup**: snapshot = solo cambios incrementales desde la última, más eficiente; backup tradicional = copia completa. Trampa clásica.
- **Backup vs Replication vs Journaling** — los tres se confunden:
    - **Backup** = copia periódica (horaria/diaria)
    - **Replication** = copia en tiempo real/near-real-time, para alta disponibilidad (no downtime)
    - **Journaling** = registro de cada cambio individual, para recuperación granular/auditoría
- **RPO** es el concepto central de esta lección — memorizar que define la frecuencia de backup según cuánta pérdida de datos es aceptable. (Nota: RTO — Recovery Time Objective — probablemente aparezca en la próxima lección de BCDR; no confundir RPO con RTO.)
- **Encryption at rest vs in transit**: par ya visto en otras lecciones, reforzado acá en contexto de backups.
- **Testear recovery mensualmente** es una buena práctica citada explícitamente — candidato a pregunta de "frecuencia recomendada de qué".

## Continuity of Operations Plan (BC/DR)

### BC vs DR — la diferencia central ⚠️

||**Business Continuity (BC) Plan**|**Disaster Recovery (DR) Plan**|
|---|---|---|
|Responde a|**Incidentes/eventos disruptivos** en general|**Desastres** específicamente|
|Relación|Plan más amplio/general|Subconjunto del BC plan|
|Cubre|Aspectos técnicos Y no técnicos del negocio|Cómo reanudar operaciones rápido tras un desastre|

Juntos suelen llamarse **BC/DR** o BCDR. La distinción práctica: si es un incidente/problema → BC plan; si es un desastre → DR plan.

### Ejemplos del video

**BC plan — ejemplo técnico**: si el domain controller sufre ransomware y nadie puede loguearse → evento disruptivo → activa incident response Y business continuity plans.

**BC plan — ejemplo de negocio (no técnico)**: Dion Training depende de pagos con tarjeta online. Tienen plan escalonado: procesador primario → si falla, cambian a secundario → si el primario no vuelve en X días, pasan a un contrato terciario (más caro, pero permite continuar indefinidamente).

**BC plan — ejemplo no técnico (disrupciones sociales)**: empresa de IT cerca de una ciudad grande de EE.UU. tuvo que crear una sección de su BC plan para protestas/disturbios (2020) — cómo operar si los empleados no pueden llegar a la oficina.

**DR plan — ejemplo (desastres naturales)**: Dion Training está en Orlando, Florida (zona de huracanes). Su DR plan cubre huracanes, incendios, inundaciones, terremotos. Decisiones concretas tomadas tras risk management analysis:

- Migraron toda su infraestructura a **AWS**, distribuida en varias regiones/availability zones → continuidad aunque Orlando sea afectado
- Dispersaron personal geográficamente (equipo de student success dividido entre EE.UU. y Filipinas) — si un lugar tiene un desastre (huracán en Florida, inundación en Filipinas), el otro cubre las operaciones

### Gobernanza del BC/DR Plan

- **Responsabilidad de la alta dirección (senior management)** — sin su apoyo, el plan se estanca y fracasa.
- Deben: establecer objetivos, nombrar un **Business Continuity Coordinator**, liderar el **Business Continuity Committee**.
- El comité debe incluir representantes de **múltiples departamentos** (tech, legal, seguridad, comunicaciones, etc.) — no es solo trabajo de IT.
- Funciones del comité: determinar prioridades de recuperación por tipo de evento, identificar/priorizar sistemas críticos, reportar a la alta dirección.
- **Definir el alcance (scope)** del plan es clave para evitar **scope creep** — depende del risk appetite y tolerancia al riesgo de la organización, definidos por la alta dirección.
- En organizaciones grandes, el plan se puede desglosar por función de negocio o región geográfica — pero todas las piezas deben ser coherentes entre sí.

### Para el examen

- **BC vs DR es la trampa #1** de esta lección — memorizar bien: BC = respuesta a eventos disruptivos en general (técnicos y no técnicos); DR = subset de BC enfocado específicamente en desastres. Esto refuerza (y corrige/alinea) lo visto en la lección de Policies (BC vs DR).
- **Senior management ownership** es un dato clave — el examen puede preguntar "¿quién es responsable de dirigir el desarrollo del BC plan?" → alta dirección, no IT solo.
- **Business Continuity Coordinator y Committee** son roles/estructuras específicas — memorizar sus nombres y que el comité es multi-departamental.
- **Scope creep** en el contexto de BC planning — recordar que definir el alcance es responsabilidad de la alta dirección basada en risk appetite/tolerance.
- Los ejemplos de Dion Training (multi-región AWS, personal disperso geográficamente) ilustran **redundancia geográfica** aplicada a continuidad de negocio — conecta con conceptos de High Availability y Multi-cloud vistos antes.
- Esta lección prepara el terreno para la próxima: **Redundant Site Considerations** (hot/warm/cold sites) — es la implementación técnica concreta del DR plan.
## Redundant Site Considerations

Un **sitio redundante** = ubicación/instalación de respaldo que puede asumir funciones/operaciones esenciales si el sitio principal falla.

### Los 3 tipos clásicos de sitios (tabla comparativa clave)

|Tipo|Equipamiento|Tiempo de activación|Costo|
|---|---|---|---|
|**Hot site**|Totalmente equipado — "uno de todo" (servidores, data mirroring en tiempo real, red, etc.)|Casi instantáneo, mínimo downtime|Muy alto|
|**Warm site**|Elementos fundamentales (electricidad, líneas telefónicas, conectividad, algunos escritorios) pero falta equipo final (laptops, monitores, teléfonos)|Días|Medio|
|**Cold site**|Cascarón vacío — baños, mesas, sillas básicas, sin red/teléfonos/computadoras|Semanas/meses (1-2 meses)|Bajo (solo alquiler)|

**Nota práctica**: la mayoría de las organizaciones NO usan hot site para todo el negocio — solo para funciones mission-critical 24/7 (ej: 50 de 500 empleados esenciales), combinando modelos híbridos (hot para lo crítico + warm/cold para el resto).

### Mobile Site

Puede ser hot, warm o cold — la diferencia es que usa **unidades portátiles independientes** (trailers, carpas) en vez de edificios fijos, entregadas al lugar y conectadas a power/internet. Ejemplo real: **DJC2** (Deployable Joint Command and Control) del ejército de EE.UU. — entregable en 72 horas, funcionalidad parcial en 24h adicionales, funcionalidad completa en hasta 7 días. Usado en el terremoto de Haití 2010.

### Virtual Site (5to tipo, moderno)

Implementa hot/warm/cold sites dentro de un entorno **cloud**:

- **Virtual hot site**: entorno totalmente replicado, accesible al instante en la nube
- **Virtual warm site**: recursos/datos parcialmente replicados, escalables rápido a full funcionalidad
- **Virtual cold site**: solo almacena datos/configs críticos en la nube, activa recursos solo ante desastre real (minimiza costo operativo)

Ventajas: escalabilidad rápida, costo-efectividad, mantenimiento fácil.

### Geographic Dispersion

Distribuir recursos/personal en distintas ubicaciones geográficas — reduce riesgo de que un solo evento (huracán, terremoto) afecte todo. Ejemplo del video (Dion Training): hot site completo solo para servidores (vía cloud), oficina pequeña + trabajo remoto para empleados, energía redundante (solar + batería + generador + red eléctrica), y **triple redundancia de internet** (microwave link → cable → cellular modem de respaldo).

### Platform Diversity

Usar distintos OS, equipos de red, o proveedores cloud en el sitio redundante vs. el principal — mitiga riesgo de que una vulnerabilidad que afecte una plataforma (ej: Cisco) tumbe ambos sitios a la vez.

- **Trade-off**: misma plataforma en ambos sitios = más fácil de soportar/mantener, pero vulnerable a un ataque que afecte esa plataforma específica; plataforma diversa = inmune a ese riesgo puntual, pero configuración más compleja y costosa de mantener.

### Para el examen

- **Hot / Warm / Cold site** es el trío más clásico de examen en toda esta sección — memorizar la tabla de equipamiento/tiempo/costo es prioritario.
- **Mobile site** ≠ un cuarto nivel de equipamiento — es una **variante de entrega** (portátil) que puede ser hot/warm/cold.
- **Virtual site** es la adaptación cloud de los 3 tipos clásicos — puede aparecer en preguntas modernas sobre DR en la nube.
- **Platform diversity trade-off** (soporte simple vs. resiliencia ante vulnerabilidad compartida) es un patrón de pregunta de "pros y contras" muy típico de Security+.
- Esta lección es la aplicación práctica y técnica del **DR plan** visto en la lección anterior de BC/DR — conectar ambos conceptos: BC/DR plan = la estrategia; redundant sites = dónde/cómo se ejecuta esa estrategia.
- Con esta lección casi se cierra el bloque completo de Cyber Resilience — queda pendiente la última: **Resilience and Recovery Testing** (tabletop exercises, failover, simulation, parallel processing).
## Resilience and Recovery Testing

**Resilience testing** = evalúa la capacidad de un sistema de resistir/adaptarse a eventos disruptivos. **Recovery testing** = evalúa la capacidad de restaurar operación normal tras el evento. Funcionan como "simulacro de incendio" para la red/organización. Cuatro métodos:

### Tabletop Exercise

Discusión basada en un escenario simulado entre stakeholders clave, para evaluar/mejorar preparación **sin desplegar recursos reales**.

- Formato: grupo sentado con un guion básico, un facilitador introduce un evento ("su domain controller fue comprometido por un actor de nation-state") — esto es un **exercise injection**
- Los stakeholders debaten qué haría cada equipo en respuesta
- Detecta gaps/fisuras en los planes → se ajustan y se incorporan a SOPs e incident response programs futuros
- Ventajas: barato, alto engagement, también funciona como team building

### Failover Test

Experimento controlado para verificar la transición fluida de un componente primario a uno secundario/backup en caso de fallo, asegurando funcionalidad ininterrumpida.

- Ejemplo del video: mover operaciones de un data center en la Costa Este a uno alternativo en la Costa Oeste — se prueba realmente el failover, no solo se discute
- Requiere más recursos/tiempo/energía que un tabletop
- Buena práctica: probarlo **al menos una vez al año**, con equipo pequeño en el sitio, y siempre tener un **plan de rollback** por si el failover no funciona como esperado

### Simulation

Representación artificial/generada por computadora de un sistema o escenario real, para imitar condiciones/fallos y evaluar la respuesta. A diferencia del tabletop (solo papel/discusión), en la simulación el personal **ejecuta realmente** sus acciones de respuesta en un entorno virtualizado.

- Ejemplo: red corporativa virtualizada en la nube donde un **Red Team** ataca mientras el **Blue Team** detecta/responde en tiempo real
- Permite evaluar no solo el plan sino también a las personas y su capacidad de trabajar bajo el caos de un ataque real
- Termina con feedback de observadores para que ambos bandos aprendan

### Parallel Processing

Replicar datos/procesos en un sistema secundario y ejecutar primario y secundario **simultáneamente**, para comprobar confiabilidad/estabilidad del sistema secundario sin interrumpir operaciones diarias.

- Requiere planificación meticulosa y ejecución cuidadosa
- En resilience testing: prueba manejo de múltiples fallos simultáneos (cortes de red, fallos de hardware, corrupción de datos)
- En recovery testing: evalúa qué tan bien se recupera un sistema de múltiples puntos de fallo

### Para el examen

- **Los 4 métodos en orden de "realismo/costo creciente"**: Tabletop (discusión, barato) → Failover test (transición real controlada) → Simulation (ejecución en entorno virtualizado, involucra Red/Blue team) → Parallel processing (sistemas corriendo en simultáneo). Buena forma de recordarlos por nivel de involucramiento real de sistemas.
- **Tabletop = "no se despliegan recursos reales"** es la frase clave que distingue tabletop de los otros 3 métodos — trampa clásica de examen.
- **Exercise injection**: término técnico específico de tabletop exercises — el evento/dato que el facilitador "inyecta" en el escenario.
- **Failover test necesita rollback plan** — conecta con el backout plan visto en change management.
- **Simulation usa Red/Blue Team** — conecta directamente con la lección de Pentest Types (Red/Blue/Purple Teaming).
- Punto conceptual de cierre: la resiliencia **no es un checkbox único** — es un proceso continuo de test → adapt → improve. Puede aparecer como distractor ("¿basta con hacer esto una vez?" → No).
- Esta lección cierra completamente el objetivo **3.4** (Cyber Resilience and Redundancy) — vale la pena repasar todo el bloque (HA, RAID, capacity planning, powering data centers, backups, BC/DR, redundant sites, testing) como una unidad temática grande antes del examen, dado que las preguntas de SY0-701 suelen combinar varios de estos subtemas en un mismo escenario.
## Resilience and Recovery Testing

**Resilience testing** = evalúa la capacidad de un sistema de resistir/adaptarse a eventos disruptivos. **Recovery testing** = evalúa la capacidad de restaurar operación normal tras el evento. Funcionan como "simulacro de incendio" para la red/organización. Cuatro métodos:

### Tabletop Exercise

Discusión basada en un escenario simulado entre stakeholders clave, para evaluar/mejorar preparación **sin desplegar recursos reales**.

- Formato: grupo sentado con un guion básico, un facilitador introduce un evento ("su domain controller fue comprometido por un actor de nation-state") — esto es un **exercise injection**
- Los stakeholders debaten qué haría cada equipo en respuesta
- Detecta gaps/fisuras en los planes → se ajustan y se incorporan a SOPs e incident response programs futuros
- Ventajas: barato, alto engagement, también funciona como team building

### Failover Test

Experimento controlado para verificar la transición fluida de un componente primario a uno secundario/backup en caso de fallo, asegurando funcionalidad ininterrumpida.

- Ejemplo del video: mover operaciones de un data center en la Costa Este a uno alternativo en la Costa Oeste — se prueba realmente el failover, no solo se discute
- Requiere más recursos/tiempo/energía que un tabletop
- Buena práctica: probarlo **al menos una vez al año**, con equipo pequeño en el sitio, y siempre tener un **plan de rollback** por si el failover no funciona como esperado

### Simulation

Representación artificial/generada por computadora de un sistema o escenario real, para imitar condiciones/fallos y evaluar la respuesta. A diferencia del tabletop (solo papel/discusión), en la simulación el personal **ejecuta realmente** sus acciones de respuesta en un entorno virtualizado.

- Ejemplo: red corporativa virtualizada en la nube donde un **Red Team** ataca mientras el **Blue Team** detecta/responde en tiempo real
- Permite evaluar no solo el plan sino también a las personas y su capacidad de trabajar bajo el caos de un ataque real
- Termina con feedback de observadores para que ambos bandos aprendan

### Parallel Processing

Replicar datos/procesos en un sistema secundario y ejecutar primario y secundario **simultáneamente**, para comprobar confiabilidad/estabilidad del sistema secundario sin interrumpir operaciones diarias.

- Requiere planificación meticulosa y ejecución cuidadosa
- En resilience testing: prueba manejo de múltiples fallos simultáneos (cortes de red, fallos de hardware, corrupción de datos)
- En recovery testing: evalúa qué tan bien se recupera un sistema de múltiples puntos de fallo

### Para el examen

- **Los 4 métodos en orden de "realismo/costo creciente"**: Tabletop (discusión, barato) → Failover test (transición real controlada) → Simulation (ejecución en entorno virtualizado, involucra Red/Blue team) → Parallel processing (sistemas corriendo en simultáneo). Buena forma de recordarlos por nivel de involucramiento real de sistemas.
- **Tabletop = "no se despliegan recursos reales"** es la frase clave que distingue tabletop de los otros 3 métodos — trampa clásica de examen.
- **Exercise injection**: término técnico específico de tabletop exercises — el evento/dato que el facilitador "inyecta" en el escenario.
- **Failover test necesita rollback plan** — conecta con el backout plan visto en change management.
- **Simulation usa Red/Blue Team** — conecta directamente con la lección de Pentest Types (Red/Blue/Purple Teaming).
- Punto conceptual de cierre: la resiliencia **no es un checkbox único** — es un proceso continuo de test → adapt → improve. Puede aparecer como distractor ("¿basta con hacer esto una vez?" → No).
- Esta lección cierra completamente el objetivo **3.4** (Cyber Resilience and Redundancy) — vale la pena repasar todo el bloque (HA, RAID, capacity planning, powering data centers, backups, BC/DR, redundant sites, testing) como una unidad temática grande antes del examen, dado que las preguntas de SY0-701 suelen combinar varios de estos subtemas en un mismo escenario.

-Resumen facil
- **Tabletop:** Solo hablar/discutir sentados en una mesa ("¿Qué haríamos si...?"). Cero código, cero máquinas.
    
- **Simulation:** Practicar en un entorno virtual/laboratorio. Las personas ejecutan acciones reales pero en un sistema aislado.
    
- **Parallel Processing:** Correr el sistema secundario junto al primario procesando datos reales para validar que funciona igual de bien.
    
- **Failover Test:** Transferir la operación real de un servidor/sitio a otro (se apaga o desvía el principal y el secundario toma el mando).