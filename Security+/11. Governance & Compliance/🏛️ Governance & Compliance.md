
> [!info] Objetivos que cubre **5.1** (elementos de governance efectiva) y **5.4** (elementos de compliance efectivo).

- **Governance** = gestión global de infraestructura, políticas y operaciones de IT, alineada a objetivos del negocio y requisitos normativos. Motivos clave: risk management, strategic alignment, resource management, performance measurement.
- **Compliance** = adherencia a leyes, regulaciones, normas y políticas aplicables. Motivos clave: legal obligations, trust & reputation, data protection, business continuity.

### Temas que vienen en las próximas lecciones

- **GRC** (Governance, Risk, Compliance) y cómo se relacionan con guidelines, policies, standards, procedures
- Estructuras de governance: boards, committees, government entities, centralized vs decentralized
- **Policies**: AUP (acceptable use), information security policy, business continuity, disaster recovery, incident response, change management, SDLC
- **Standards**: password standards, access control standards, physical security standards, encryption standards
- **Procedures**: change management, onboarding/offboarding, playbooks
- Consideraciones de governance: regulatory, legal, industry, local/regional, national, global
- Compliance monitoring & reporting: due diligence vs due care, attestation & acknowledgement, internal vs external compliance, automation
- Consecuencias de non-compliance: fines, sanctions, reputational damage, loss of license, contractual impacts

---

## 🧱 Governance en GRC

> [!info] Es el liderazgo y las reglas internas que armás en tu empresa para que la tecnología responda a los objetivos del negocio. **No es un software**, es cómo organizás la seguridad y la infraestructura de tu organización.

### Jerarquía: Las 4 Reglas de Juego

_(De lo más general a lo más específico / de menos a más obligatorio)_

|#|Nivel|Descripción|¿Obligatorio?|Ejemplo|
|---|---|---|---|---|
|1|**Policy**|La meta general de alto nivel|✅ Sí|"Las cuentas de la empresa deben ser seguras para evitar accesos no autorizados."|
|2|**Standard**|La regla técnica exacta|✅ Sí|"Toda contraseña debe tener mínimo 12 caracteres, símbolos y MFA activado."|
|3|**Procedure**|El paso a paso operativo para cumplir el estándar|✅ Sí|"1. Entrá a portal.empresa.com → 2. Clic en 'Perfil' → 3. Cambiá la clave."|
|4|**Guideline**|Recomendación de buenas prácticas|❌ No|"Te sugerimos usar un gestor de contraseñas como Bitwarden."|

### Monitoring & Review (proceso continuo)

El framework **no es fijo**, se adapta a cambios:

- **Monitoring**: detectar fallas o cambios (ej: irse a la nube, trabajo remoto, nuevas leyes de datos).
- **Review**: actualizar las policies, standards y procedures según lo detectado.

> [!example] Aplicación práctica Governance es la **dirección y control**, el **gobierno interno de la organización**. La empresa quiere proteger los datos de los clientes (Objetivo de Negocio). Por lo tanto, la Governance establece que el equipo de IT DEBE usar cifrado obligatorio (Standard/Policy) y el Cybersecurity Committee del Board va a auditar que esto se cumpla (Oversight).

> [!warning] Tips clave para el examen
> 
> - **Pregunta de obligatoriedad**: la única **opcional / no obligatoria** es la **Guideline**. Las demás se deben cumplir.
> - **Jerarquía de orden**: memorizá la escala: `Policy` (visión) → `Standard` (regla) → `Procedure` (paso a paso).
> - Definición rápida: "pasos detallados" → **Procedure**; "regla o métrica específica" → **Standard**; "intención o compromiso de alto nivel" → **Policy**.
> - Governance es un proceso dinámico (Monitor → Review), nunca un documento estático.

---

## 🏢 Governance Structures

### Boards

Grupo de personas elegidas por los shareholders para supervisar la gestión de la organización. Fijan dirección estratégica, establecen policies y toman decisiones clave. Ej: board de una tech company con expertos del sector para orientar sobre tendencias tecnológicas.

### Committees

Subgrupos del board, cada uno con foco específico — permiten atención más detallada a áreas complejas.

- **Audit committee**: supervisa el proceso de financial reporting.
- **Governance committee**: asegura que el board funcione bien y respete los principios de governance.
- **Cybersecurity committee**: gestiona riesgos de ciberseguridad (típico en tech companies).

### Government entities

Aplican leyes y regulaciones que las organizaciones deben cumplir, sobre todo públicas/reguladas. Ej: **FTC** (Federal Trade Commission, EE.UU.) — protección al consumidor y competencia, impacta prácticas de governance.

### Centralized vs Decentralized structures

||Centralized|Decentralized|
|---|---|---|
|**Autoridad**|Concentrada en niveles altos de management|Distribuida por toda la organización|
|**Ventaja**|Decisiones coherentes, líneas de autoridad claras|Decisiones más rápidas, responde mejor a necesidades locales|
|**Desventaja**|Lenta para responder a necesidades locales/departamentales|Puede generar inconsistencias|
|**Ejemplo típico**|Empresa grande (políticas uniformes)|Startup tech (agilidad e innovación)|

> [!warning] Para el examen
> 
> - **Board vs Committee**: board = dirección estratégica general; committee = subgrupo enfocado en un área específica (audit, governance, cybersecurity).
> - **Government entities** como la FTC son ejemplo clásico de regulador externo que impacta governance — puede aparecer con otros nombres (SEC, etc.) en preguntas.
> - **Centralized vs decentralized**: la trampa típica es memorizar bien qué trade-off tiene cada uno — centralized = consistency pero lento; decentralized = agilidad pero riesgo de inconsistencia. El tamaño/tipo de organización (empresa grande vs startup) suele ser la pista en el enunciado para identificar cuál conviene.

---

## 📜 Key IT Policies

### Acceptable Use Policy (AUP)

Documento que define qué pueden y no pueden hacer los usuarios con los sistemas/recursos de la organización. Objetivo: proteger de problemas legales y amenazas de seguridad. Ej: prohibir sitios peligrosos, software no autorizado, uso personal de recursos de la empresa.

### Information Security Policy

La piedra angular de la postura de seguridad. Describe cómo se protegen los activos de información (internas y externas). Abarca: data classification, access control, encryption, physical security. Ej: datos sensibles encriptados en tránsito y en reposo, acceso solo para personal autorizado. Objetivo: **CIA** (confidentiality, integrity, availability).

### Business Continuity Policy

Cómo la organización sigue operando (operaciones críticas) durante y después de una interrupción. Estrategias ante cortes de luz, fallos de hardware, desastres naturales.

### Disaster Recovery Policy

Relacionada con BC pero enfocada específicamente en **recuperar sistemas y datos** tras un desastre: backup/restore de datos, recuperación de hardware/software, sitios de procesamiento alternativos. Ej: backups periódicos en ubicación externa (offsite), testeados regularmente.

### Incident Response Policy

Plan para gestionar incidentes de seguridad: detectar, reportar, evaluar, responder, aprender (lifecycle clásico). Ej: a quién notificar en un data breach, cómo contener/investigar, cómo prevenir recurrencia.

### SDLC Policy (Software Development Lifecycle)

Guía cómo se desarrolla software: requirements → design → coding → testing → deployment → maintenance. Incluye secure coding practices, code review, testing standards.

### Change Management Policy

Rige cómo se gestionan cambios en sistemas/procesos para minimizar riesgo de interrupciones. Procedimientos para solicitar, testear, aplicar y revisar cambios.

> [!warning] Para el examen
> 
> - **BC vs DR — la trampa clásica**: Business Continuity = mantener las _operaciones del negocio_ funcionando; Disaster Recovery = recuperar específicamente _sistemas y datos IT_. DR es un subconjunto/complemento técnico de BC.
> - **AUP**: memorizar que define uso aceptable/no aceptable de recursos — suele aparecer en preguntas sobre onboarding o sanciones por mal uso.
> - **Incident Response**: memorizar el flujo — detect → report → assess → respond → learn (lessons learned al final es clave, suelen preguntarlo).
> - **SDLC**: si aparece una pregunta sobre "dónde meter seguridad en el desarrollo", la respuesta es: en todas las fases (shift-left / secure by design), no solo al final.
> - **Change Management**: la palabra clave es "controlado y coordinado" — reduce riesgo de downtime no planificado.

### Formalización de las policies

- **Approval**: las _Policies_ tienen que estar aprobadas y **firmadas por la alta dirección** (_executive management_) para que tengan validez y peso. Si no las firma la dirección, son solo sugerencias.
- **Acknowledgement**: cuando entrás a trabajar a una empresa, te hacen firmar (o tildar una casilla digital) que leíste y aceptás la _Acceptable Use Policy_. Si la violás, te pueden sancionar o despedir.
- **Version Control**: tienen fecha de creación, fecha de última revisión (_Review_) y número de versión (v1.0, v2.0), porque como vimos con _Monitoring & Review_, son documentos vivos que cambian con el tiempo.

---

## ⚙️ Standards

> [!info] Si la **Policy** es el documento que dice _"Las cuentas y la infraestructura de la empresa deben ser seguras"_, los **Standards** son los **documentos con las reglas técnicas exactas y obligatorias** que se deben configurar en los sistemas para cumplir esa política.

### 4 configuraciones/reglas técnicas obligatorias típicas

1. **Password Standards** (Reglas de Contraseñas): qué requisitos le exigís al sistema de login (ej. mínimo 12 caracteres, ponerle _salt_ a las claves en la base de datos).
2. **Access Control Standards** (Reglas de Permisos): cómo le das acceso a los empleados a las carpetas y sistemas:
    - **DAC**: el creador del archivo decide quién lo ve.
    - **MAC**: el sistema decide según la sensibilidad (ej. Top Secret vs. Público).
    - **RBAC**: el sistema da permisos según el puesto (ej. "Contador", "Soporte IT").
3. **Physical Security Standards** (Reglas de Seguridad Física): qué barreras físicas usás para proteger los servidores (cámaras CCTV, tarjetas de acceso, extintores en el Data Center).
4. **Encryption Standards** (Reglas de Cifrado): qué algoritmo matemático obligás a usar para proteger los datos:
    - **AES (Simétrico)**: para cifrar discos duros y archivos guardados (_Data at Rest_).
    - **RSA (Asimétrico)**: para cifrar comunicaciones e Internet (_Data in Transit_ / PKI).

### Conceptos clave que confunden en el examen

> [!warning] Least Privilege vs Separation of Duties
> 
> - **Least Privilege**: a vos te doy _solo_ lo que necesitás para tu trabajo (ni un permiso más).
> - **Separation of Duties**: una sola persona _no puede hacer todo el proceso_. Ej: quien crea la factura no puede ser la misma persona que aprueba el pago.

> [!tip] Hashing/Salting no es Cifrado El cifrado se puede revertir (desencriptar); el _hash_ es una huella digital de una sola vía (irreversible) para guardar claves en la DB de forma segura.

> [!note] En resumen Son los **parámetros y reglas técnicas concretas** (PDFs/Documentos técnicos) que el equipo de IT e Infraestructura tiene que configurar en los servidores, routers y sistemas de la empresa.

---

## 🪜 Procedures

> [!info] Secuencias sistemáticas de pasos para lograr un resultado concreto — garantizan consistencia, eficacia y compliance. Ejemplos generales: procedimiento de evacuación de emergencia (rutas, puntos de encuentro, roles), procedimiento de backup de datos (incremental diario, full semanal, testing de integridad).

### Change Management Procedure

1. **Identificar** la necesidad de cambio y evaluar impacto potencial
2. **Planificar**: cómo se implementará, quién participa, qué recursos se necesitan
3. **Implementar** — a menudo por etapas (staged rollout) para poder resolver problemas rápido
4. **Revisar** post-cambio: evaluar éxito y extraer lessons learned

**Buenas prácticas clave**:

- Testear cambios significativos antes de aplicarlos
- Tener siempre un **backout/rollback plan** por si el cambio sale mal
- Programar cambios disruptivos en una **maintenance window**
- Documentar todo el proceso para futuros cambios

### Onboarding / Offboarding

- **Onboarding**: integrar nuevos empleados — orientation, training, integration activities. Objetivo: productividad y compromiso rápido.
- **Offboarding**: gestionar la salida de un empleado — recuperar assets de la empresa, **desactivar accesos a sistemas**, exit interviews. Objetivo: transición fluida + recolectar feedback para mejorar la organización.

### Playbooks

Guía paso a paso detallada para detectar y responder a un tipo específico de incidente (o proceso), garantizando consistencia sin importar quién lo ejecute. Incluye: recursos necesarios, pasos a seguir, resultados esperados. Usado en incident response de ciberseguridad, atención al cliente, etc. — donde velocidad y consistencia son críticas.

> [!warning] Para el examen
> 
> - **Orden de change management** (identify → plan → implement → review) es candidato fuerte a pregunta de secuencia — memorizar el orden.
> - **Trampa clásica de offboarding**: el paso que más preguntan es la **desactivación de accesos** — si no se hace a tiempo, queda una cuenta activa = riesgo de seguridad (insider threat post-salida).
> - **Backout plan / rollback**: término específico que puede aparecer como opción de respuesta — es el plan para revertir un cambio fallido.
> - **Playbook**: no confundir con policy/standard/procedure genérico — es específico para un tipo de incidente/tarea, orientado a acción rápida y repetible (pensar "runbook" de IT ops o incident response).

---

## 🌍 Governance Considerations

### Regulatory

Las organizaciones deben cumplir regulaciones que varían por industria y ubicación (protección de datos, privacidad, medioambiente, laboral). Ejemplo clave: **GDPR** (UE) — impacta cómo se recolectan/almacenan/usan datos personales de ciudadanos UE. Incumplimiento → multas, sanciones, daño reputacional. Requiere programas de compliance con auditorías periódicas y training.

### Legal

Relacionadas con regulaciones pero más amplias: contract law, propiedad intelectual, corporate law. Ejemplo: labor laws (salario mínimo, horas extra, salud/seguridad, anti-discriminación). Riesgo principal: **litigation** (breach of contract, product liability, conflictos laborales) → requiere legal strategy y recursos sólidos.

### Industry

Normas y mejores prácticas de un sector específico — no son legalmente vinculantes pero influyen en expectativas de clientes/socios/reguladores. Ejemplo: **Agile** (Scrum, Kanban) en desarrollo de software — no es requisito legal pero es estándar de facto; no adoptarlo = desventaja competitiva.

### Geographic (Local → Global)

|Nivel|Ejemplo|
|---|---|
|**Local**|Ordenanzas municipales (ej: zonificación que prohíbe fábricas en zonas residenciales)|
|**Regional**|**CCPA** (California) — derecho a saber qué datos se recolectan, derecho a borrarlos, derecho a opt-out de venta de datos|
|**Nacional**|**ADA** (EE.UU.) — accommodations razonables para empleados/clientes con discapacidad|
|**Global**|**GDPR** (UE) — aplica a cualquier empresa que procese datos de ciudadanos UE, sin importar dónde tenga su sede|

Desafío clave: **conflict of laws** entre jurisdicciones — las leyes de protección de datos varían mucho globalmente, requiere enfoque flexible y conocimiento profundo de cada jurisdicción.

> [!warning] Para el examen
> 
> - **GDPR aparece dos veces** en el material: como ejemplo de regulatory Y de global — la clave es que aplica extraterritorialmente (empresa fuera de la UE pero procesa datos de ciudadanos UE = igual debe cumplir). Es un punto de examen muy común.
> - **CCPA vs GDPR**: CCPA = regional (California, EE.UU.), GDPR = global/UE. No los confundas si dan un escenario con "residente de California" vs "ciudadano de la UE".
> - **ADA**: recordar que es nacional (EE.UU.) y trata sobre discapacidad — accessibility + reasonable accommodations.
> - Memorizar la jerarquía **local → regional → national → global** y poder ubicar un ejemplo dado en el nivel correcto.

---

## 📡 Compliance Monitoring & Reporting

### Compliance Reporting

|Tipo|Qué es|Ejemplo|
|---|---|---|
|**Internal**|Recolección/análisis de datos para verificar cumplimiento de políticas internas propias, hecho por auditoría interna o compliance dept|Institución financiera: revisar transacciones sobre cierto umbral, verificar que fueron aprobadas por compliance officer|
|**External**|Demostrar cumplimiento a entidades externas (reguladores, auditores, clientes) — suele ser obligatorio por ley/contrato|Farmacéuticas reportando a la **FDA** sobre cumplimiento de **GMP** (Good Manufacturing Practices)|

### Compliance Monitoring

Revisión y análisis periódico de las operaciones para asegurar cumplimiento de leyes, regulaciones y políticas internas. Componentes:

> [!warning] Due diligence vs Due care ⚠️ (definición estándar de Security+)
> 
> - **Due diligence** = la investigación/research que se hace para _identificar_ riesgos potenciales (ej: investigar leyes y regulaciones comerciales de un país antes de expandirse ahí)
> - **Due care** = las _acciones_ que se toman para mitigar esos riesgos una vez identificados (ej: entrenar empleados en la nueva normativa, contratar asesoría legal local)

**Attestation & Acknowledgement**:

- **Attestation**: declaración formal de la parte responsable de que los procesos/controles cumplen. Ej: developers atestiguando que siguieron los protocolos de seguridad de datos.
- **Acknowledgement**: reconocimiento y aceptación de esos requisitos por todas las partes involucradas. Ej: firmar un compliance agreement.

**Internal vs External monitoring**:

- **Internal**: revisión periódica propia de operaciones vs. políticas internas (ej: manufactura revisando sus propios procesos de producción)
- **External**: auditorías de terceros vs. normas externas (ej: auditor externo verificando **ISO 9001**)

### Automation en Compliance

Sistemas automatizados agilizan recolección de datos, mejoran precisión, dan monitoreo en tiempo real.

- Ejemplo salud: sistema que flaggea accesos no autorizados a historiales de pacientes → detección rápida de violaciones **HIPAA**
- Ejemplo banca: sistema que detecta transacciones sospechosas de lavado de dinero automáticamente

> [!warning] Para el examen
> 
> - **Due diligence vs due care** es un par clásico y muy preguntado en Security+: **diligence = investigar/identificar** (antes), **care = actuar/mitigar** (después).
> - **Attestation vs Acknowledgement**: attestation = "yo certifico que cumplí"; acknowledgement = "yo reconozco/acepto el requisito". Se confunden en examen.
> - **Internal vs External reporting/monitoring**: la clave es quién lo hace y para quién — interno = tu propia auditoría para tus propias políticas; externo = terceros/reguladores para normas externas.
> - Ejemplos con siglas reales (FDA, GMP, HIPAA, ISO 9001) son candidatos a aparecer en preguntas de escenario — asociar cada sigla a su contexto (salud = HIPAA, farma = FDA/GMP, calidad = ISO 9001). **Todas esas siglas son regulaciones o normas externas**.

---

## ⚖️ Consequences of Non-Compliance

### Fines

Sanciones económicas cobradas por reguladores por violar leyes.

> [!example] CompTIA Exam Classic **GDPR** → multa de hasta **€20 millones o el 4% del ingreso global anual** (el monto que sea mayor). _Caso_: British Airways multada con £183M tras un _data breach_.

### Sanctions

Sanciones del gobierno para bloquear operaciones (no solo pedir dinero), como confiscar ingresos o suspender la actividad temporalmente.

> [!example] China amonestó y frenó operaciones de empresas bajo su _Cybersecurity Law_.

### Reputational Damage

Pérdida de confianza del público y clientes. Impacta directamente en el valor de la empresa en la bolsa y en las ventas.

> [!example] Equifax (2017) Expuso datos de 147 millones de personas → sus acciones cayeron más del 30%.

### Loss of License

Revocación del permiso legal para operar en un mercado o industria regulada (finanzas, salud, etc.).

> [!example] El estado de Nueva York le revocó la _BitLicense_ a una empresa de criptomonedas por fallas de ciberseguridad.

### Contractual Impacts

Consecuencias legales entre entes privados. No cumplir normas de seguridad violó un acuerdo firmado (_Breach of Contract_), permitiendo al cliente demandarte o cancelar el contrato.

> [!example] Si tenés un contrato para dar servicio a un banco y sufrís un _breach_ por no tener cifrado, el banco te rescinde el contrato y te demanda por daños.

> [!warning] Para el examen
> 
> - **GDPR fine formula** (€20M o 4% de facturación global, el mayor) es un número que suelen pedir textual — memorizalo.
> - **Casos reales asociados a cada consecuencia** son un patrón de examen (dan el caso, preguntan qué tipo de consecuencia es, o viceversa):
>     - British Airways / GDPR → **fines**
>     - China cybersecurity law → **sanctions**
>     - Equifax → **reputational damage**
>     - Bitcoin company NY → **loss of license**
> - Estas 5 consecuencias (fines, sanctions, reputational damage, loss of license, contractual impacts) cierran el Domain 5 objetivo 5.4 — es candidato a pregunta tipo "cuál NO es una consecuencia de non-compliance" o matching.