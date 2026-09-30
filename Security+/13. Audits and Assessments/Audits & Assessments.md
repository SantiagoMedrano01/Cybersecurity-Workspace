
Cubre el objetivo **5.5**: explicar tipos y propósitos de auditorías y evaluaciones.

**Audits**: evaluación sistemática de sistemas, aplicaciones y controles de seguridad para determinar eficiencia/eficacia. Suele hacerla una entidad independiente para una visión imparcial.

- **Internal**: hecha por el propio equipo de auditoría de la organización. Ej: revisar si la data protection policy está actualizada, completa, alineada a normativa, y si los empleados la cumplen.
- **External**: hecha por terceros. Ej: auditor externo evaluando cumplimiento de **PCI DSS** en una empresa de e-commerce (seguridad de red, encriptación, access control).
- Ayudan a garantizar compliance con normativas como **GDPR, HIPAA, PCI DSS**.

**Assessments**: análisis detallado de los sistemas de seguridad para identificar vulnerabilidades y riesgos — típicamente antes de implementar un sistema nuevo o cambios significativos. Tipos: **risk assessments, vulnerability assessments, threat assessments**.

Temas que van a ver en las próximas lecciones de esta sección:

- Internal audits & assessments (+ demo práctica)
- External audits & assessments (+ demo práctica)
- **Penetration testing** (pen test / ethical hacking): simular un ciberataque para encontrar vulnerabilidades explotables
- **Reconnaissance** según tipo de entorno (known, partially known, unknown) y tipo de recon (passive vs active)
- Demo de pen test con **Nmap** y **Metasploit** — escanear red, encontrar máquina vulnerable, explotar para conseguir acceso admin
- **Attestation of findings**: declaración formal por escrito de los resultados de una auditoría/evaluación

**Para el examen:** todavía nada específico para memorizar de esta intro, pero ya podés anotar las siglas/términos que vienen: **PCI DSS, GDPR, HIPAA, pen test, Nmap, Metasploit, attestation**. El próximo video con contenido real ya entra en detalle sobre internal vs external audits.


## Internal Audits & Assessments

### Internal Audits

Evaluaciones sistemáticas hechas por el propio equipo de auditoría de la organización, enfocadas en: eficacia de controles internos, cumplimiento normativo, integridad de sistemas/procesos. Áreas típicas: data protection, network security, access controls, incident response procedures.

**Ejemplo — Audit de password policy**: verificar que la política exige contraseñas complejas y cambios periódicos, y que los empleados la cumplen.

**Ejemplo — Audit de user access controls** (más detallado, pasos típicos):

1. Revisar policies/procedures de access control vs. mejores prácticas y requisitos normativos (**least privilege**, **separation of duties** — ya vistos en Standards)
2. Examinar los derechos de acceso otorgados a cada usuario — ¿coinciden con sus responsabilidades? ¿hay accesos excesivos/innecesarios?
3. Revisar procesos de otorgar/modificar/revocar accesos — ¿hay aprobación adecuada? ¿se revoca rápido cuando alguien se va o cambia de rol?
4. **Testear efectividad**: intentar acceder a sistemas/datos con una cuenta de permisos limitados — si logra acceder más allá de lo designado, es una debilidad de control
5. Documentar hallazgos → recomendar mejoras

### Conceptos clave asociados

**Compliance**: asegurar que sistemas y prácticas de seguridad se adhieren a normas/regulaciones/leyes. Las internal audits suelen ser _requeridas_ por leyes/regulaciones vigentes — cadencia típica: trimestral o anual, según el sector.

**Audit Committee**: grupo (típicamente miembros del board) que supervisa las actividades de auditoría y compliance de la organización. Responsabilidades: revisar financial reporting y controles internos, supervisar audits internas/externas, asegurar cumplimiento legal/regulatorio, resolver issues planteados por auditores.

### Internal Assessments

Diferentes de las audits: análisis en profundidad para **identificar y evaluar riesgos/vulnerabilidades potenciales**, típicamente antes de implementar sistemas nuevos o cambios significativos.

- **Self-assessments**: la propia organización evalúa su cumplimiento de normas/regulaciones — útil para detectar gaps antes de una audit formal.
- Organizaciones grandes pueden tener un equipo interno dedicado de assessment.

**Pasos típicos de un internal assessment** (ejemplo: lanzar una nueva web app):

1. **Threat modeling**: identificar amenazas potenciales (SQL injection, XSS, DoS)
2. **Vulnerability assessment**: escaneo automatizado + testing manual para encontrar vulnerabilidades conocidas
3. **Risk assessment**: evaluar impacto potencial de amenazas/vulnerabilidades — probabilidad de explotación, daño potencial, costo de mitigar
4. **Recomendar estrategias de mitigación**: fixes de código, controles adicionales, cambios de arquitectura

### Para el examen

- **Audit vs Assessment — la distinción clave**: Audit = evalúa **compliance y efectividad de controles existentes** (mirar hacia atrás/estado actual); Assessment = identifica **riesgos y vulnerabilidades potenciales** (mirar hacia adelante, antes de un cambio/lanzamiento). Trampa clásica de examen.
- **Audit committee**: recordar que suele ser del board y su rol es de _supervisión_, no ejecución directa de audits.
- **Self-assessment**: útil como preparación antes de una audit formal — puede aparecer en pregunta de "¿qué hacer antes de una auditoría externa?".
- El flujo **threat modeling → vulnerability assessment → risk assessment → mitigation** es candidato a pregunta de secuencia — memorizar el orden.
- Conecta con **least privilege / separation of duties** (Standards) y con el test de "cuenta con permisos limitados intentando acceder más allá" — es una técnica práctica de verificación de access control que puede aparecer en escenario.
## Internal Assessment — Demo práctica (Self-Assessment Checklist)

Este video es una demo mostrando un ejemplo real de checklist para self-assessment: la **Cybersecurity Self-Assessment Checklist** del MCIT (Minnesota Counties Intergovernmental Trust), una organización gubernamental que la creó para ayudar a sus miembros a identificar riesgos de datos/ciberseguridad.

### Formato de la checklist

- Header: completado por, título, fecha, firma
- Columnas por ítem: **Sí/No**, departamento/proveedor responsable, comentarios, action items, y a quién se le asigna cada action item
- Cada action item debe asignarse a una persona/grupo específico, con alguien haciendo seguimiento de que se implemente correctamente
- Idealmente se completa en grupo, con management, IT staff y profesionales de ciberseguridad juntos — para que los riesgos se entiendan bien en toda la organización

### Ejemplos de preguntas (primeras 5 del checklist)

1. ¿Tuvieron un incidente de ciberseguridad en los últimos 12 meses (hacking, malware, fraude, breach de PII, extorsión, litigio derivado)? — con subpregunta: ¿se identificaron y resolvieron las deficiencias?
2. ¿Tienen un **incident response plan** que cubra: responsabilidades, identificación, triage, notificación, investigación, eliminación de amenaza/vulnerabilidad, recuperación y continuidad del negocio?
3. ¿Tienen un **business continuity plan** que incluya: inventario de operaciones vitales, inventario de hardware/software esencial, identificación de terceros/proveedores/acuerdos, y agreements documentados con responsabilidades claras?
4. ¿Todos los dispositivos tienen antivirus/antimalware instalado y actualizado?
5. ¿Tienen una **mobile device policy**? ¿El personal fue entrenado en ella? ¿Define si se permiten dispositivos personales y bajo qué requisitos?

### Para el examen

- Esta demo es más ilustrativa que teórica — lo importante para el examen es el **concepto**: un self-assessment checklist es una herramienta de sí/no + action items + ownership, usada para dar una vista rápida de la postura de riesgo actual.
- Notá que las preguntas de ejemplo tocan temas ya vistos en lecciones anteriores: **incident response policy** (lección de Policies), **business continuity plan** (Policies), **mobile device policy** (conecta con BYOD/COPE/CYOD) — el examen puede combinar estos conceptos en un escenario de checklist.
- No hay términos nuevos de examen acá — es aplicación práctica de conceptos ya cubiertos. No hace falta memorizar detalles de esta checklist específica del MCIT.
## External Audits & Assessments

### External Audits

Evaluaciones sistemáticas hechas por entidades independientes (a diferencia de las internas, hechas por el propio equipo). Dan una perspectiva objetiva de la postura real de seguridad. Cubren: data protection, network security, access controls, incident response — buscan gaps para garantizar compliance con **GDPR, HIPAA, PCI DSS**.

Ejemplo: entidad financiera contrata auditor externo para evaluar cumplimiento **PCI DSS** trimestral/anualmente — revisa el cardholder data environment (firewalls, encriptación, access controls).

### External Assessments

Análisis detallado por entidad(es) independiente(s) para identificar vulnerabilidades y riesgos — combina scanning automatizado + testing manual. Tipos: risk, vulnerability, threat assessments (mismos tipos que las internas, pero hechos por terceros).

Ejemplo: organización de salud contrata empresa de ciberseguridad para vulnerability assessment de su sistema de historiales médicos (EHR) para cumplir **HIPAA** — reciben reporte con vulnerabilidades y estrategias de mitigación recomendadas.

### Regulatory Compliance

Objetivo de cumplir leyes/políticas/regulaciones relevantes. Regulaciones específicas por industria (**HIPAA** = salud, **PCI DSS** = tarjetas de pago) vs. generales (**GDPR** = todas las industrias). Muchas orgs usan un framework consolidado como el **NIST Cybersecurity Framework** como mecanismo principal, aplicando dentro de él los controles de cada normativa específica.

### Examinations

Inspecciones detalladas de infraestructura de seguridad hechas externamente — van más allá de una assessment normal: también testean al **personal clave** con exámenes estandarizados y verifican vigencia de certificaciones.

- Ejemplo: plantas de energía nuclear/centrales eléctricas — exámenes cada **1 a 5 años**, evaluando redes, equipos **ICS/SCADA**, procedimientos operativos, y testeando conocimiento de empleados según su rol.
- Nota: la mayoría de profesionales de ciberseguridad nunca pasan por un exam así — es típico de sectores altamente regulados (energía, nuclear).

### Independent Third-Party Audits

Auditorías de terceros independientes — dan perspectiva imparcial, ayudan a detectar debilidades que auditorías/assessments internas podrían pasar por alto. Generan confianza con clientes/stakeholders/reguladores. Muchas regulaciones (**GDPR, PCI DSS**) las **exigen periódicamente** como parte del compliance.

### Para el examen

- **Audit vs Assessment vs Examination** — jerarquía de profundidad: Assessment (identifica riesgos/vulnerabilidades) → Audit (evalúa compliance/efectividad de controles) → **Examination** (todo lo anterior + testea al personal y certificaciones — el más riguroso, típico de industrias críticas como nuclear/energía). Es candidato fuerte a pregunta de "¿cuál es más exhaustivo?" o matching de escenario.
- **ICS/SCADA** aparece acá como término nuevo asociado a examinations en infraestructura crítica — anotalo, puede reaparecer en otras secciones del curso (infraestructura crítica / OT security).
- **NIST Cybersecurity Framework** como "framework paraguas" para armonizar compliance — dato específico que puede aparecer en pregunta directa.
- **GDPR y PCI DSS exigen third-party audits** — recordar esto como ejemplo de regulación que impone auditoría externa obligatoria, no opcional.
- Esta lección + la anterior (Internal Audits & Assessments) cierran el objetivo 5.5 — vale repasarlas juntas antes del examen.
- "Una organización debe cumplir con PCI DSS, HIPAA e ISO 27001 al mismo tiempo. ¿Qué marco es ideal para mapear y gestionar de forma unificada todos estos requerimientos de cumplimiento?"

→ Respuesta: NIST Cybersecurity Framework (CSF).

 marco de trabajo desarrollado por el gobierno de EE. UU. (NIST) que proporciona directrices y buenas prácticas para gestionar y reducir el riesgo de ciberseguridad en cualquier tipo de organización
## Penetration Testing — Types

**Pentesting / ethical hacking** = simular un ciberataque contra un sistema para encontrar vulnerabilidades explotables.

### Physical Penetration Testing

Testea la seguridad física: cerraduras, tarjetas de acceso, cámaras. Técnicas: **tailgating** (colarse detrás de un empleado en una puerta segura), clonar access cards. Objetivo: identificar vulnerabilidades físicas y recomendar mejoras. Beneficios: identificar debilidades físicas, mejorar security awareness de empleados, prevenir acceso no autorizado.

### Offensive Penetration Testing (Red Teaming)

Enfoque proactivo/agresivo: buscar y **explotar activamente** vulnerabilidades usando las mismas técnicas que un atacante real. Ej: Red Teamer explota una vulnerabilidad conocida para acceso no autorizado, luego reporta el hallazgo para que se corrija antes de que un atacante real lo haga. Beneficios: simula ataques reales para entrenar a los defensores, y sirve para justificar/conseguir presupuesto de inversión en ciberseguridad mostrando vulnerabilidades reales.

### Defensive Penetration Testing (Blue Teaming)

Enfoque **reactivo**: fortalecer sistemas, detectar y responder a ataques, mejorar tiempos de respuesta a incidentes. Ej: monitorear la red buscando actividad inusual; si detecta un ataque, mitigar daño y reforzar el sistema. Beneficios: mejora incident response, hardening de sistemas, mejores capacidades de detección.

### Integrated Penetration Testing (Purple Teaming)

Combina ofensivo + defensivo en un mismo ejercicio: Red Team ataca mientras Blue Team detecta/responde, trabajando juntos.

- Si Blue Team detecta el ataque → Red Team escala a un ataque más avanzado para evadir detección
- Si Blue Team **no** detecta el ataque → Red Team guía a Blue Team explicando el ataque y cómo configurar mejor los sensores de red
- Objetivo: aprendizaje mutuo, colaboración, evaluación de seguridad completa

### Para el examen

- **Los "colores de equipo" son terminología clásica de examen**: Red Team = ofensivo/ataque, Blue Team = defensivo/detección-respuesta, Purple Team = ambos combinados/colaborativo. Memorizar esta asociación es prioritario — aparece muy seguido en Security+.
- **Physical pentest** suele confundirse con "social engineering" — acá el foco es específicamente en controles físicos (locks, badges, cámaras), aunque tailgating también es una técnica de social engineering; en el contexto de esta lección se enmarca como physical pentest.
- **Purple team NO es un equipo separado con gente distinta** — es la colaboración/comunicación entre Red y Blue, dato importante para no confundir en preguntas conceptuales.
- Ojo: esta lección menciona reconnaissance y tipos de entornos (known/partially known/unknown) como próximo tema del overview inicial — esperá la siguiente lección para eso.
## Reconnaissance in Penetration Testing

**Reconnaissance** = fase inicial de recolección de información sobre el sistema objetivo — analogía del video: un ladrón que "casea" una casa antes de robarla (observa horarios, sistemas de seguridad, etc.). Objetivo: planificar mejor el ataque, aumentar probabilidad de éxito, reducir riesgo de detección.

### Active vs Passive Reconnaissance

|Tipo|Cómo funciona|Riesgo/Beneficio|
|---|---|---|
|**Active**|Contacto directo con el sistema objetivo — ping, port scanning, intentos de conexión. Ej: usar **Nmap** para escanear puertos abiertos|Más información, pero **mayor riesgo de detección** (los defensores pueden ver el escaneo)|
|**Passive**|Recolectar info sin tocar el sistema objetivo — investigación online, OSINT, observar tráfico de red, bases de datos públicas. Ej: usar **WHOIS** para info de contacto de un dominio (útil para phishing)|Menos riesgo de detección, pero menos información obtenida|

### Tipos de entorno (determinan cuánto reconocimiento se necesita)

**Known environment** La organización da a los pentesters info detallada de antemano: diagramas de red, IPs, versiones de OS/apps, incluso credenciales. Objetivo: no descubrir activos, sino evaluar a fondo vulnerabilidades de activos ya conocidos. Simula un **insider threat** (empleado con conocimiento profundo del entorno). Puede requerir poco o ningún reconocimiento — ya tienen los detalles.

**Partially known environment** Enfoque híbrido: info limitada (ej: algunos rangos IP o endpoints, pero no todo). Simula un atacante con algo de conocimiento interno (ej: de un breach anterior) que aún necesita descubrir el resto del entorno. El reconocimiento es importante acá para llenar los vacíos de conocimiento (software, versiones, configuraciones).

**Unknown environment** Info mínima o nula — quizás solo el nombre de la empresa o dominio. Simula un atacante externo real. Empieza con reconocimiento exhaustivo para descubrir activos, luego identifica vulnerabilidades y vías de explotación. Da la visión más completa de la postura de seguridad externa de la organización.

### Para el examen

- **Known / Partially Known / Unknown** son términos oficiales de CompTIA que reemplazaron a "white box / gray box / black box" en versiones recientes del examen — memorizar la terminología nueva.
- **Known = insider threat simulation; Unknown = external attacker simulation** — asociación clave para preguntas de escenario.
- **Active vs Passive** es trampa clásica: active = contacto directo (mayor riesgo de detección); passive = sin contacto (OSINT, WHOIS). Nmap = ejemplo de active; WHOIS = ejemplo de passive — herramientas específicas que pueden aparecer en preguntas.
- Conecta con la lección anterior de pentesting (Red/Blue/Purple team): el reconocimiento es típicamente el primer paso que ejecuta el **Red Team** en un engagement ofensivo.
### Attestation of Findings

**Attestation** = validación/confirmación formal de una entidad que afirma la exactitud y autenticidad de cierta información. Clave en internal y external audits — sustenta la confiabilidad e integridad de datos, sistemas y procesos.

#### Attestation of Findings (contexto pentest)

Demuestra que la pentest realmente ocurrió y que los resultados son válidos, respaldados por evidencia. **No siempre es requerida** — depende del motivo del pentest:

- Si es solo porque la organización lo quiso → puede no requerirse
- Si es por compliance/regulación (**GLBA, HIPAA, Sarbanes-Oxley, PCI DSS**) → suele requerirse una **attestation letter** de la empresa que hizo la pentest

Una attestation letter debe incluir: resumen de hallazgos + prueba de que la evaluación se realizó durante un período determinado. La organización puede mostrar esta carta a terceros como evidencia de que hizo la evaluación de seguridad.

#### Attestation vs. Report — diferencia clave

||**Report**|**Attestation**|
|---|---|---|
|Contenido|Hallazgos + solución recomendada|Hallazgos **+ evidencia** de que ocurrieron|
|Ejemplo|"No usan MFA"|Demuestra _cómo_ se explotó (logs, datos, código de exploit) — aunque a veces la evidencia se **muestra** en la reunión sin dejarla como copia (ej: código propio del pentester)|

#### Otros tipos de attestation (integridad organizacional, no exclusivos de pentest)

- **Software attestation**: validar que el software no fue alterado/manipulado maliciosamente. Ej: verificación de firma digital criptográfica al instalar un update — si la firma es válida, confirma autenticidad del proveedor.
- **Hardware attestation**: validar integridad de componentes de hardware. Ej: **TPM** (Trusted Platform Module) almacena mediciones de configuración de hardware/firmware, verificadas en boot para detectar cambios inesperados.
- **System attestation**: validar el nivel de seguridad de un sistema. Ej: proveedor cloud certificando cumplimiento de **ISO 27001** o **SOC 2** — da confianza a clientes de que sus datos se manejan de forma segura.

#### Attestation en audits

- **Internal audit**: el auditor interno certifica exactitud de registros financieros, efectividad de risk management, cumplimiento de policies internas.
- **External audit**: entidad tercera independiente certifica estados financieros, cumplimiento regulatorio, eficiencia operativa — crucial para stakeholders (inversores, acreedores, reguladores).

#### Para el examen

- **Attestation vs Report** es la distinción más propensa a pregunta directa: report = hallazgos + recomendación; attestation = hallazgos + **evidencia formal** (a veces solo mostrada, no entregada).
- **TPM** es sigla técnica específica para hardware attestation — puede aparecer también en otros contextos de Security+ (boot integrity, secure boot).
- **ISO 27001 y SOC 2** son certificaciones de system attestation muy comunes en escenarios de cloud/vendor — asociar cada una a "estándar de seguridad de la información" (ISO 27001) y "reporte de controles de servicio" (SOC 2).
- Regulaciones que suelen requerir attestation letters: **GLBA, HIPAA, SOX (Sarbanes-Oxley), PCI DSS** — memorizar esta lista como contexto de "cuándo se requiere attestation formal".
- Esta lección cierra el **objetivo 5.5** (Audits & Assessments) completo. Vale la pena repasar las 6 lecciones del bloque (internal/external audits+assessments, pentesting types, reconnaissance, attestation) juntas antes del examen.
- **Report (Informe):** Te dice _qué está mal y cómo arreglarlo_.
    
- **Attestation:** Es la **carta firmada/certificado oficial** respaldado por evidencia (logs, firmas, capturas) que sirve como **prueba legal** ante reguladores de que la prueba realmente se hizo y es verídica.
![[Pasted image 20260814220154.png]]
![[Pasted image 20260814221836.png]]