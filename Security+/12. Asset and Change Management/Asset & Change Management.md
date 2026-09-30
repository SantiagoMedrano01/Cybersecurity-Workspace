Este video también es solo intro/overview de la nueva sección — **Asset & Change Management** (Domain 1 y 4).

## Asset & Change Management — overview de la sección

Cubre objetivos **1.3** (importancia de los change management processes y su impacto en seguridad), **4.1** (aplicar security techniques a computing resources) y **4.2** (implicaciones de seguridad de una gestión adecuada de hardware/software/data assets).

**Asset management** = proceso sistemático de desarrollar, operar, mantener y disponer de activos de forma rentable. En ciberseguridad, asegura que todo activo digital (hardware, software) esté identificado, catalogado y monitoreado → reduce vulnerabilidades.

**Change management** = enfoque estructurado para transicionar sistemas/personas/organización de un estado actual a uno futuro deseado. Asegura que modificaciones (updates, nuevos deployments) se hagan de forma controlada, evitando breaches o configuraciones erróneas.

Temas que van a ver en las próximas lecciones:

- **Acquisition & procurement**: proceso estructurado de obtener tecnologías/servicios de seguridad
- **Mobile asset deployment models**: BYOD, COPE, CYOD y otros
- **Asset management**: assignment & accounting, monitoring & tracking (ownership, classification, inventory checks, enumeration, **MDM**)
- **Asset disposal & decommissioning**: sanitization, destruction, certification, data retention policies
- **Importance of change management**: aprobación (change advisory board / CAB), ownership del cambio, stakeholder involvement, impact analysis
- **Change management processes**: maintenance windows, backout plan, testing de resultados, alineación con SOPs
- **Technical implications de cambios**: allow lists/deny lists, restricted activities, downtime, service/application restarts, legacy applications y sus dependencies
- **Documentación**: version control, actualizar diagramas, revisar policies/procedures si el cambio falla, actualizar tickets, transparencia y accountability

## Acquisition & Procurement

### Acquisition vs Procurement — la diferencia clave

- **Acquisition**: simplemente el proceso de _obtener_ bienes y servicios (el acto de comprar en sí).
- **Procurement**: el proceso _completo_ de sourcing y obtención — incluye todos los pasos previos que llevan hasta la adquisición (evaluación, aprobación, presupuesto, etc.).

### Opciones de compra (purchasing options)

**1. Company credit card**

- Para compras de bajo costo que se necesitan rápido (ej: tinta, tóner, papel).
- Cada empresa fija límites de transacción y qué tipo de ítems se pueden comprar, según el rol del empleado.

**2. Individual purchase (reembolso)**

- El empleado compra con su propia tarjeta y luego pide reembolso.
- Se usa cuando el empleado no tiene tarjeta corporativa, en situaciones de emergencia, o para viajes de negocio (vuelos, hoteles, comida) sin travel card asignada.
- También aplica cuando la organización es muy nueva y todavía no tiene tarjeta corporativa propia.

**3. Purchase Order (PO)**

- Documento formal emitido por el departamento de compras que autoriza una compra específica — usado para compras grandes/caras.
- El PO **no es un pago en sí** — es una promesa/obligación financiera de pagar.
- Se liquida según términos de pago: **Net 15, Net 30, Net 60** (días para efectuar el pago).

### Approval process (proceso de aprobación interno)

Antes de comprar (sobre todo hardware/software), se evalúa:

- Que la compra se alinee con objetivos/necesidades de la empresa
- Que haya presupuesto asignado suficiente
- **Seguridad y compatibilidad** con la infraestructura existente

Ejemplo real: **ITPR** (Information Technology Procurement Request) de la US Navy — proceso riguroso: evaluación de necesidad/beneficio → chequeo contra estándares de seguridad e interoperabilidad de la Navy → verificación de presupuesto/fuente de financiamiento → múltiples niveles de aprobación por decision-makers senior.

### Después de la aprobación (post-purchase)

Una vez adquirido el producto: evaluar compatibilidad con sistemas actuales, hacer security checks/configuración, entrenar a usuarios finales, integrar en los workflows existentes.

### Para el examen

- **Acquisition vs Procurement**: acquisition = el acto de comprar; procurement = todo el proceso end-to-end (sourcing + acquisition). Trampa clásica de definición.
- **PO no es pago**: memorizar esto — es una promesa de pago, no dinero transferido. Net 15/30/60 son los términos típicos que pueden preguntar.
- **Company card vs Individual purchase vs PO**: la clave para distinguir en un escenario es el **monto y la urgencia** — card = bajo costo/rápido; individual = reembolso, sin tarjeta asignada o emergencia; PO = compras grandes/formales.
- El caso **ITPR (Navy)** es un buen ejemplo mental de "approval process riguroso" — el examen puede describir un escenario similar y preguntar qué tipo de control es.
## Mobile Device Deployment Models

### BYOD (Bring Your Own Device)

El empleado usa su propio dispositivo personal para trabajo.

- ✅ Cómodo para el empleado (un solo dispositivo), barato para el empleador (no compra hardware)
- ❌ El dispositivo es propiedad y control del empleado → desafío de seguridad: la empresa no siempre puede meterlo en MDM, forzar patches/updates, o exigir configuraciones seguras
- Requiere controles de seguridad antes de permitir uso corporativo (ej: instalar security software específico)

### COPE (Corporate-Owned, Personally Enabled)

La empresa entrega el dispositivo, pero el empleado también puede usarlo para uso personal. Analogía del video: como un auto de empresa que también podés usar para mandados personales.

- ✅ Gestión simplificada (todos los dispositivos son los mismos modelos)
- ❌ Inversión inicial alta (hay que comprar dispositivos para todos), preocupación de privacidad del empleado (la empresa tiene acceso total para monitorear/revisar actividad, por ser dispositivo propio)

### CYOD (Choose Your Own Device)

Término medio: el empleado elige su dispositivo de una lista corta aprobada por la empresa. La empresa mantiene mayor control y estandarización que BYOD, pero da algo de elección (que COPE no da).

- ✅ Empleados tienen voz en la elección, IT solo soporta un número limitado de modelos
- ❌ Costo inicial alto (igual que COPE); típicamente **no** se permite uso personal → empleado termina cargando 2 celulares (uno laboral CYOD, uno personal)

### Cómo elegir el modelo — 3 criterios

1. **Costo**: BYOD parece barato pero tiene costos ocultos de seguridad/compatibilidad; COPE/CYOD tienen costo inicial alto pero menor costo de soporte continuo (menos variedad de modelos).
2. **Seguridad**: si es prioridad, **CYOD** es la mejor opción — mejor control + compatible con MDM.
3. **Satisfacción del empleado**: si querés dar libertad/elección, BYOD o CYOD > COPE.

### Para el examen

- **Cuadro comparativo mental** (muy propenso a pregunta de escenario):

||Ownership|Uso personal permitido|Control de seguridad de la empresa|
|---|---|---|---|
|**BYOD**|Empleado|Sí (es su dispositivo)|Bajo — la empresa no controla el dispositivo completamente|
|**COPE**|Empresa|Sí|Alto|
|**CYOD**|Empresa|Generalmente no|Alto (mejor para MDM)|

- **CYOD = mejor para seguridad** porque combina control corporativo con compatibilidad total con MDM — dato específico que puede aparecer en pregunta directa.
- **BYOD = mayor riesgo de seguridad** porque el dispositivo es propiedad del empleado (no se puede forzar MDM, patching, config).
- Si el escenario menciona "empleados cargan dos teléfonos" → pista de **CYOD** (o COPE mal usado).
- Memorizar bien las siglas y expandirlas: **BYOD** = Bring Your Own Device, **COPE** = Corporate-Owned, Personally Enabled, **CYOD** = Choose Your Own Device.
## Asset Management

**Asset management** = enfoque sistemático para gobernar y obtener valor de los elementos de los que la organización es responsable, a lo largo de su ciclo de vida. Aplica a activos tangibles (edificios, hardware) e intangibles (IP, reputación), pero esta lección se enfoca en activos tecnológicos/digitales.

Dos áreas principales: **assignment & accounting** y **monitoring & tracking**.

### Assignment & Accounting

**Ownership assignment**: cada activo se asigna a una persona o grupo (el "owner"). Determina quién es responsable de qué. Ej: una workstation de alto rendimiento → equipo de diseño gráfico; una licencia de software específica → una persona puntual. Ayuda a evitar ambigüedad y facilita troubleshooting/upgrades/reemplazos.

**Classification**: categorizar activos según función, valor u otros criterios definidos por la organización. Determina decisiones de mantenimiento, reemplazo o retiro. Ej: servidores con datos críticos = alto valor → mantenimiento estricto; laptop viejo guardado = bajo valor → candidato a reciclaje/disposal.

### Monitoring & Tracking

**Monitoring**: mantener un **inventario/registro** de cada activo — specs, ubicación, usuario asignado, historial de revisión.

**Tracking**: va más allá del monitoring — controla ubicación, estado y condición de activos físicos con software/tecnología especializada (ej: GPS tracking de un dispositivo específico).

**Enumeration**: identificar y contar activos, especialmente en adquisiciones grandes o retiros. Usa herramientas/scanners para detectar qué está conectado a la red, qué OS/software corre, y su estado de salud. Beneficios: inventario actualizado, detección de **dispositivos no autorizados**, decisiones informadas sobre patching/vulnerabilidades. Ejemplo del video: al adquirir otra empresa, lo primero es enumerar su red para ver qué activos tienen.

### MDM (Mobile Device Management)

Solución para gestionar y monitorear de forma segura smartphones, tablets, laptops de empleados. Capacidades clave:

- Aplicar políticas corporativas y estandarizar software
- **Remote lock / remote wipe** si se pierde/roba un dispositivo
- Push remoto de updates, patches y apps corporativas a miles de dispositivos simultáneamente
- Centraliza gestión → mejora eficiencia operativa y reduce riesgo de dispositivos inseguros/desactualizados

### Para el examen

- **Monitoring vs Tracking**: monitoring = mantener el inventario/registro (qué es, quién lo tiene); tracking = ubicación y estado físico en tiempo real (dónde está, cómo está). Trampa clásica de definición.
- **Enumeration** es un término técnico que puede aparecer también en contextos de pentesting/recon — acá el contexto es asset inventory, no confundir.
- **MDM = remote lock/wipe** es el dato más "accionable" de esta lección — probable pregunta de escenario ("empleado perdió el celular corporativo, ¿qué hacés?" → MDM remote wipe).
- Conectar esto con la lección anterior de BYOD/COPE/CYOD: MDM es la herramienta que hace viable especialmente **COPE y CYOD** (dispositivos propiedad de la empresa).
## Asset Disposal & Decommissioning

Guiado por **NIST SP 800-88** ("Guidelines for Media Sanitization"), que cubre sanitization, destruction y certification.

### Sanitization

Hacer los datos inaccesibles e irrecuperables (incluso con técnicas forenses avanzadas), sin necesariamente destruir el medio físico. Técnicas:

|Técnica|Cómo funciona|Nota clave|
|---|---|---|
|**Overwriting (clearing/purging)**|Sobrescribe datos con info aleatoria, repetido varias veces (1, 7, o 35 pases según clasificación)|Más clasificación = más pases|
|**Degaussing**|Campo magnético fuerte altera los dominios magnéticos del medio (HDD, cinta)|Destruye el dispositivo para almacenamiento futuro — no reutilizable; más completo que overwriting|
|**Secure Erase**|Rutina de borrado en el firmware del dispositivo que purga bloques de datos|Precursor de Cryptographic Erase; tenía fallas descubiertas con el tiempo|
|**Cryptographic Erase (CE)**|En vez de borrar los datos, se destruyen las **claves de cifrado** — datos quedan ilegibles|Muy rápido (30-60 seg vs horas), permite reutilizar/revender el dispositivo sin riesgo|

### Destruction

Va un paso más allá de sanitization: destruye el **dispositivo físico** para que ni el dispositivo ni los datos puedan recuperarse/reutilizarse. Métodos NIST: **shredding, pulverizing, melting, incinerating**.

- Usado en entornos de alta seguridad (ej: datos top-secret) donde sanitization no se considera suficientemente segura.

### Certification

Prueba formal (según NIST) de que los datos/hardware fueron eliminados de forma segura — crea un **audit trail**. Importante para organizaciones con controles regulatorios estrictos o datos sensibles (gov, financiero, médico).

### Data Retention Policies

El ciclo de vida de los datos va desde creación hasta destrucción (pasando por uso y archivo). Retention = decidir estratégicamente qué conservar y por cuánto tiempo — no es guardar todo indefinidamente.

Razones para retener:

- Requisitos regulatorios (ej: registros financieros, historiales médicos con plazos obligatorios)
- Análisis histórico, predicción de tendencias, litigios

Razones para NO retener todo:

- Costo de almacenamiento (tangible: servidores; intangible: complejidad de gestión)
- Mayor superficie de ataque para ciberamenazas — "cuanto más almacenás, más tenés que asegurar"

### Para el examen

- **Sanitization vs Destruction**: sanitization = los datos son irrecuperables pero el dispositivo puede sobrevivir/reutilizarse; destruction = el dispositivo físico también queda inutilizable. Trampa clásica.
- **Cryptographic Erase es el método moderno preferido** para SSDs/dispositivos modernos — reemplazó a Secure Erase. Punto muy propenso a pregunta directa ("¿qué método es más rápido y permite reutilizar el dispositivo?" → CE).
- **Degaussing NO funciona en SSD** (solo en medios magnéticos como HDD o cinta) — dato importante aunque no lo diga explícito el video, es candidato a trampa de examen.
- **NIST 800-88** es la referencia estándar que hay que asociar a "media sanitization" — puede aparecer como nombre de documento en preguntas.
- **Certification** = el "papel" que demuestra que hiciste bien la sanitización/destrucción — clave para compliance/auditorías.
## Change Management — Importance & Key Roles

### Por qué importa

El cambio es inevitable en entornos empresariales modernos, pero necesita precisión, planificación y un enfoque estructurado para evitar interrupciones. Sin gestión adecuada → caos, resistencia de empleados, sobrecarga de soporte. Ejemplo del video: migrar de una versión legacy de Office a O365 puede generar muchas llamadas a soporte si no hay training antes/durante/después.

Cualquier cambio, grande o chico, altera procesos existentes — la gestión del cambio busca integrar el cambio sin fricción, maximizando beneficios y minimizando disrupciones.

### Componentes clave del proceso

**CAB (Change Advisory Board)** Órgano de representantes de distintas áreas de la organización que evalúa las ramificaciones de un cambio propuesto antes de aprobarlo. Hace la **due diligence** del cambio: evalúa viabilidad, impacto potencial, y si se alinea con los objetivos de la organización.

**Change Owner** Persona (o equipo) responsable de iniciar el request de cambio — es el "advocate" del cambio: detalla por qué es necesario, beneficios potenciales, y desafíos posibles.

**Stakeholders** Cualquier persona con interés en el cambio propuesto (afectada directamente, o involucrada en su evaluación/implementación). Deben ser consultados/informados antes del cambio. Tipos, según ejemplo del video (update al motor de exámenes prácticos):

- **Technical stakeholders**: developers que manejan servidores y código
- **Business stakeholders**: equipo de soporte a estudiantes
- **End-user stakeholders**: los estudiantes mismos

**Impact Analysis** Evaluar consecuencias potenciales antes de ejecutar el cambio: ¿qué puede salir mal? ¿efectos inmediatos en procesos, reputación, usuarios? ¿impacto a largo plazo? ¿riesgos imprevistos? Objetivo: preparar a la organización tanto para mitigar riesgos como para maximizar beneficios.

### Para el examen

- **CAB** es la sigla clave de esta lección — memorizar que hace la evaluación/aprobación de cambios, no la ejecución.
- **Change Owner vs Stakeholder**: el owner _propone e impulsa_ el cambio; el stakeholder _se ve afectado o participa en la evaluación/implementación_ pero no necesariamente lo inició. Trampa clásica de confundir roles.
- **Tres tipos de stakeholders** (technical, business, end-user) es un patrón de clasificación que puede aparecer en preguntas de escenario — dado un ejemplo, identificar qué tipo de stakeholder es cada persona/rol mencionado.
- **Impact analysis** = la pregunta "¿qué puede salir mal?" antes de aplicar el cambio — se conecta con el backout plan y maintenance windows vistos en la lección de procedures.
## Change Management Processes

### Los 5 pasos del proceso de change management

1. **Prepare for the change**: entender el estado actual, reconocer la necesidad de transición, evaluar procesos existentes, identificar ineficiencias y desafíos potenciales, reunir recursos e involucrar stakeholders.
2. **Create a vision for the change**: definir claramente el estado futuro deseado, el "por qué" del cambio, y cómo se ve el éxito — sirve de guía y ayuda a generar aceptación entre stakeholders.
3. **Implement the change**: ejecutar el plan — puede requerir training, reestructuración de equipos, nuevas herramientas. Comunicación continua con stakeholders es clave para reducir resistencia.
4. **Verify the change**: medir efectividad comparando el nuevo estado contra los objetivos originales (encuestas, métricas de performance, entrevistas). Corregir discrepancias detectadas.
5. **Document the change**: registro histórico completo del proceso (necesidad inicial → implementación → verificación) + lessons learned para referencia futura.

### Áreas clave a considerar durante el proceso

**Maintenance windows** Ventana de tiempo reservada para aplicar cambios, minimizando impacto en operaciones normales. Ejemplo del video: ventana semanal sábados 12am-4am. No implica que solo se pueda cambiar una vez por semana — cambios de emergencia (ej: parche crítico de seguridad) pueden aprobarse e implementarse fuera de la ventana, en minutos/horas/días según criticidad.

**Backout plan (rollback plan)** Estrategia predefinida para revertir el sistema a su estado original si el cambio no sale como se esperaba — la "red de seguridad" ante complicaciones.

**Testing results** Validar que el cambio logró los resultados deseados y no generó nuevos problemas — se hace _después_ de implementar, no basta con aplicar y esperar que funcione.

**Standard Operating Procedures (SOP)** Instrucciones detalladas paso a paso sobre cómo ejecutar una tarea específica. Usar SOPs consistentemente al implementar cambios reduce errores e inconsistencias en la red.

### Para el examen

- **Los 5 pasos en orden** (Prepare → Vision → Implement → Verify → Document) son candidato fuerte a pregunta de secuencia — memorizar el orden exacto.
- **Maintenance window vs Emergency change**: la trampa es entender que tener ventana programada NO bloquea cambios urgentes — hay proceso de emergency change aparte, aprobado con criticidad variable.
- **Backout plan**: siempre asociado a "qué hacemos si el cambio falla" — término que ya vieron en la lección de Procedures, ahora reforzado acá.
- **SOP**: sigla que puede aparecer sola en el examen — significa consistencia y procedimiento estándar, no confundir con policy o standard (vistos en lecciones anteriores).
- Esta lección conecta directamente con la anterior (CAB, change owner, stakeholders, impact analysis) — juntas cubren el objetivo 1.3 completo.
## Technical Implications of Change

### Allow lists & Deny lists

Usadas por routers/firewalls para controlar acceso a recursos.

- **Allow list**: qué entidades tienen permitido acceder
- **Deny/block list**: qué entidades tienen prohibido acceder

Al proponer un cambio, siempre revisar si hay que modificar estas listas — un simple ajuste de IP puede otorgar o restringir acceso sin querer. Al actualizar un firewall: revisar que las reglas existentes migren correctamente al nuevo dispositivo — si no, puede haber fallos funcionales o brechas de seguridad.

### Restricted Activities

Tareas marcadas como restringidas por su impacto potencial en salud/seguridad del sistema (ej: acceder a una DB protegida, apagar un servidor demasiado sensible para reiniciar en horario normal). Verificar esto **antes** de aprobar un cambio, para evitar data leaks o contratiempos operativos.

### Downtime

Todo cambio conlleva riesgo de downtime. Hay que calcular el downtime potencial y sopesarlo contra los beneficios del cambio: ¿afecta a usuarios en horas pico? Por eso se programa dentro de una maintenance window cuando es posible.

### Service/Application Restarts

Muchos cambios (ej: instalar software de seguridad crítico) requieren reiniciar servicios/apps. Reiniciar algo simple en una PC personal no es lo mismo que reiniciar un **domain controller** u otro servidor en uso activo por todos los usuarios — hay que considerar no solo el tiempo de reinicio, sino datos que puedan perderse en tránsito y demoras asociadas.
![[Pasted image 20260814150410.png]]

- **Aplicas el parche:** Se instalan o sobrescriben los archivos en el disco.
    
- **Reinicias el servicio (o servidor):** El proceso en ejecución se cierra y vuelve a cargar desde el disco los nuevos archivos actualizados en la memoria RAM.
Basata con reiniciar el servicio que se aplico el parche no todo el servidor
### Legacy Applications

Software/sistemas antiguos que se siguen usando porque funcionan, aunque haya versiones más nuevas disponibles. Son especialmente sensibles a cambios porque:

- Reciben menos soporte
- Son menos flexibles/adaptables que apps modernas
- Incluso un update menor en otra parte del sistema puede causar malfunciones o crashes

### Dependencies

En sistemas interconectados, un cambio en un área puede tener efectos en cascada en toda la red — incluso en arquitectura de partners. Ejemplo: una app depende de una DB específica. **Mapear las dependencias existentes antes de aplicar un cambio** es clave, o un ajuste pequeño puede causar un corte grande sin que te des cuenta.

### Para el examen

- Estos 6 elementos (allow/deny lists, restricted activities, downtime, restarts, legacy apps, dependencies) son la checklist técnica que se evalúa durante el **impact analysis** (visto en la lección anterior) — conectá ambos conceptos.
- **Legacy applications** es un tema recurrente en Security+ — recordar que son frágiles ante cambios y requieren consideración especial, y suelen mencionarse junto a "vulnerabilities" en otros contextos del examen también.
- **Allow list vs Deny list**: trampa simple pero puede aparecer en escenario de firewall — allow list = whitelist (permitido), deny list = blacklist (bloqueado).
- **Dependencies mapping** antes de un cambio es un dato accionable candidato a pregunta de "mejor práctica antes de implementar un cambio".

## Documenting Changes

Documentar cambios crea un registro claro de qué se hizo, cuándo y por qué — asegura accountability y sirve de roadmap para referencia futura. Tres componentes: version control, actualización de documentación, y mantenimiento de registros.

### Version Control

Sistema que rastrea y gestiona cambios en documentos, software y otros archivos, permitiendo colaboración entre varios usuarios y **rollback a una versión anterior** cuando sea necesario. Es la red de seguridad que evita que los cambios generen más caos — permite volver a un estado más estable si algo falla o si se necesita ejecutar el backout plan. Objetivo: continuidad y estabilidad del entorno a lo largo del tiempo, no solo preservar historial.

### Actualizar la documentación

Cada cambio requiere:

- **Actualizar diagramas** (flowcharts, network diagrams) para que reflejen el estado actual real del sistema — omitir esto es "receta para el desastre" en redes grandes donde varios equipos dependen de la documentación (nadie puede confiar en su memoria en una red que abarca ciudad/país/mundo).
- **Revisar policies y procedures** si el cambio no salió como se esperaba — no solo arreglar el problema inmediato, sino ajustar policy/procedure para prevenir que se repita (mejora continua / iterative improvement).
- **Actualizar change requests / issue tickets** tras la implementación exitosa — no es mero trámite: crea un timeline claro de las acciones, informa a stakeholders, y deja un historial de cambios para referencia futura.

### Para el examen

- **Version control como mecanismo de rollback** es el punto más propenso a pregunta directa — conecta con el backout plan visto en la lección de procesos.
- **"Documentar no es solo administrativo"** — el examen puede plantear un escenario donde alguien se salta la documentación "porque total me acuerdo" y preguntar qué riesgo genera (config errors, malentendidos entre equipos).
- Esta lección cierra el ciclo completo de **Change Management**: importance (CAB, owner, stakeholders, impact analysis) → process (5 steps: prepare, vision, implement, verify, document) → technical implications (allow/deny lists, downtime, legacy apps, dependencies) → **documentation** (version control, diagrams, policy review, tickets). Vale la pena repasar las 4 lecciones juntas como un bloque antes del examen, ya que suelen aparecer preguntas que combinan conceptos de varias.