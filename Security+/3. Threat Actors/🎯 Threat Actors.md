# 🧭 Threat Actor Motivations

## Diferencia clave
* **Intent:** El objetivo específico del ataque.
* **Motivation:** La razón de fondo que empuja al actor a atacar.

## Las 10 motivaciones

1. **Data exfiltration:** Transferencia no autorizada de datos (IP, PII, secretos comerciales) para vender en la dark web, robo de identidad o ventaja competitiva.
2. **Financial gain:** La más común. Vía ransomware (cifrar datos y pedir rescate) o banking trojans (robar credenciales bancarias).
3. **Blackmail:** Obtener info sensible/comprometedora y amenazar con publicarla si no se paga (usualmente en cripto no rastreable). Incluye ransomware, doxxing y sextortion.
4. **Service disruption:** Interrumpir servicios (caos, mensaje político, o extorsión), típicamente vía DDoS.
   * *Ejemplo:* GitHub DDoS 2018 ~20 min de caída.
5. **Philosophical/political beliefs:** Es el hacktivism. Hacktivistas atacan por agenda política/social: defacement de sitios, filtración de datos. Suelen apuntar a gobiernos, finanzas y medios.
6. **Ethical reasons:** Hackers éticos (pen testers, bug bounty hunters). Actúan como atacantes maliciosos pero para encontrar y reportar vulnerabilidades antes de que las exploten.
7. **Revenge:** Empleados despedidos/descontentos que buscan dañar a la empresa (data breach, filtración, disrupción), o ataques contra entidades percibidas como "enemigas".
8. **Disruption/chaos:** Actores no autorizados que atacan por la emoción o para causar daño porque sí (*"some men just want to watch the world burn"* — cita de Alfred en The Dark Knight). Va desde malware hasta ataques a infraestructura crítica.
9. **Espionage:** Espiar individuos/orgs/naciones para obtener info sensible. Motivado por seguridad nacional (nation-state actors), ventaja comercial (empresa rival), o ventaja política (hacktivistas/estados).
10. **War:** Cyberwarfare como herramienta de conflicto entre naciones: interrumpir infraestructura, comprometer seguridad nacional, causar daño económico. Casi siempre asociado a nation-state threat actors.

> [!TIP] Para el examen Security+
> Suele preguntar *"¿qué motivación corresponde a este escenario?"* — prestá atención a distinguir:
> * **Financial gain vs Blackmail:** Ambos piden plata, pero blackmail usa amenaza de exposición.
> * **Hacktivism vs War:** Ambos son políticos, pero hacktivism es de individuos/grupos no estatales, mientras que war es nation-state contra nation-state.

---

# 🏷️ Threat Actor Attributes

Tres atributos clave para clasificar actores de amenazas:

### 1. Internal vs External
* **Internal threat actor:** Tiene acceso legítimo (empleado, contractor, business partner) y lo usa mal.
  * *Ejemplo:* Robar la fórmula de Coca-Cola con acceso propio y llevársela a Pepsi = internal threat actor, aunque el motivo sea beneficiar a un competidor externo. Lo que define la categoría es el acceso legítimo previo, no el destino final de la info.
* **External threat actor:** No tiene acceso autorizado, tiene que conseguirlo desde afuera (malware, social engineering). Pueden ser cybercriminals, hacktivists, competidores o nation-state actors.

### 2. Resources and funding
Determina la escala, frecuencia y sofisticación de los ataques.
* **Extremo bajo:** Un hacker individual con su propia PC y sus skills.
* **Extremo alto:** Nation-state, con herramientas avanzadas, equipo de agentes calificados, poder de cómputo y presupuesto grande.

### 3. Level of sophistication/capability
Escala de bajo a alto, según destreza técnica, complejidad de herramientas/técnicas, y capacidad de evadir detección.
* **Bajo (Script kiddies):** Conocimiento técnico limitado, usan herramientas/scripts prefabricados sin entender bien cómo funcionan.
* **Alto (Nation-state actors y APT groups):** Usan malware custom, zero-day exploits, técnicas avanzadas de evasión. Pueden penetrar redes bien defendidas y mantenerse indetectados por largos períodos si tienen tiempo y recursos.

> [!NOTE] Para el examen
> Fijate que un mismo actor puede cruzar categorías — por ejemplo un nation-state actor es típicamente *external + high resources + high sophistication*, mientras que un script kiddie suele ser *external + low resources + low sophistication*. Las preguntas de Security+ suelen dar un escenario y pedirte identificar el atributo (interno/externo) o el nivel de sofisticación, no solo el nombre del actor.

---

# 🧑‍💻 Unskilled Attackers (Script Kiddies)

## Definición
Individuo sin el conocimiento técnico para desarrollar sus propias herramientas de hacking o exploits. Depende de scripts y programas prefabricados por otros, y suele tener una comprensión muy básica de cómo funcionan esas herramientas.

## Perfil de atributos
* **Sophistication/capability:** Baja.
* **Resources/funding:** Bajos (típicamente un individuo, no un grupo organizado con presupuesto).

Pueden causar daño real explotando vulnerabilidades conocidas y sin parchear, usando herramientas ya armadas — no hace falta ser sofisticado para tirar abajo un sistema mal defendido.

## Motivación
* Principalmente **recognition** (notoriedad entre pares/comunidad online) y **disruption/chaos** (la emoción de causar quilombo).
* También **curiosidad** — usan el hacking para explorar y entender sistemas.
* A diferencia de hacktivistas o actores más sofisticados: rara vez están motivados por financial gain o political ideology.

## Estilo de targeting
Son **opportunistic**, no selectivos: van por targets fáciles (*"low-hanging fruit"*), no por objetivos de alto valor. Esto los diferencia de nation-state actors o APT groups, que sí eligen targets específicos de alto valor.

## Ejemplo de ataque típico
DDoS con herramientas simples tipo LOIC (*Low Orbit Ion Cannon*): cargás la IP del target, apretás un botón. Si consigue que otros se sumen (coordinados o no), pueden tumbar sin mucho esfuerzo la red de un usuario individual o una organización con seguridad débil.

> [!NOTE] Punto clave para el examen
> Aunque individualmente son poco peligrosos, el volumen los vuelve una amenaza considerable — muchos script kiddies atacando en simultáneo (o coordinados a través de un ataque tipo botnet/DDoS masivo) pueden generar daño significativo, pese a la baja sofisticación de cada uno por separado.

---

# ✊ Hacktivists

**Hacktivism:** Hacking + activism. Ciberataques usados para promover una causa política/social, no para lucro.

### Técnicas típicas
* **Website defacement:** "Graffiti electrónico".
* **DDoS:** Saturar sistemas para negar acceso.
* **Doxxing:** Exponer info privada (nombre, dirección, teléfono) para incitar acciones contra la víctima.
* **Data leaks:** Robar y publicar datos confidenciales de la organización.

### Atributos
* **Sofisticación:** Variable pero tiende a ser alta — algunos crean exploits custom contra sistemas bien defendidos.
* **Motivación:** Ideológica/política, no económica. Atacan a quienes perciben en contra de su causa (gobiernos, corporaciones) por censura, DDHH, medio ambiente, etc.

### Grupos clave para el examen
* **Anonymous:** Operation Payback (2010), DDoS contra MPAA/RIAA por antipiratería.
* **LulzSec:** "50 Days of Lulz" (2011), atacó Sony, CIA, FBI; mezcla de caos + oposición a censura/vigilancia.

> [!NOTE] Punto clave
> La agenda ideológica del hacktivista determina el target — no son oportunistas como los script kiddies, sino selectivos según su causa.

---

# 🕴️ Organized Crime

**Qué son:** Sindicatos criminales estructurados que llevaron el crimen organizado tradicional al ciberespacio (roles definidos, alto anonimato, alcance global).

### Atributos
* **Sophistication:** Muy alta (malware custom, ransomware, phishing avanzado).
* **Organización:** Bien financiados y organizados, altamente adaptables.
* **Tácticas:** Usan cryptocurrency, dark web, cell-site simulators para evadir detección.
* **Alcance:** Operan transnacionalmente → difícil de perseguir legalmente.
* **Motivación:** Casi siempre *financial gain* (data breaches, identity theft, fraud, ransomware). 
* **Targets:** SMBs y high-net-worth individuals.

> [!NOTE] Dato clave para el examen
> No son ideológicos, pero pueden ser contratados como mercenarios por gobiernos/entidades políticas — ahí "tocan" lo político sin que cambie su motivación real (sigue siendo plata).

### Ejemplos
* **FIN7:** Phishing avanzado, retail/hospitality.
* **Carbanak group:** Malware custom, robó +$1B a bancos.

---

# 🏛️ Nation-State Actors

## Definición
Grupos/individuos patrocinados por un gobierno para operaciones cibernéticas contra otras naciones, orgs o individuos. Suelen ser parte de organizaciones militares/de inteligencia, o entidades independientes con recursos estatales — así el gobierno mantiene *plausible deniability* (negación plausible).

## Atributos
* **Sophistication:** La más alta de todos los actores vistos — malware custom, zero-day exploits, operaciones coordinadas y complejas.
* **Resources:** Máximos — presupuesto y personal calificado.
* **Relación con APT:** Frecuentemente ligados al término **APT (Advanced Persistent Threat)**: ataque prolongado y selectivo donde el intruso obtiene acceso y permanece sin ser detectado por mucho tiempo, priorizando robo de datos/vigilancia por sobre daño inmediato.
  * *Ojo examen:* APT antes era sinónimo de nation-state, pero hoy también se usa para describir a grupos de organized crime con alta sofisticación.

## Motivación
Objetivos estratégicos a largo plazo, no económicos: inteligencia, disrupción de infraestructura crítica, influencia política, ciberespionaje (robo de IP, ventaja competitiva).

> [!WARNING] Excepción clave para el examen: North Korea
> Por aislamiento económico y sanciones, sus actores estatales sí atacan por *financial gain* — apuntan a bancos y exchanges de cripto para financiar al régimen.

## Concepto importante: False flag attack
Ataque diseñado para parecer que viene de otro grupo/fuente, buscando confundir la atribución.
* *Ejemplo:* Juegos Olímpicos de Invierno 2018 (Corea del Sur). Malware que inicialmente parecía norcoreano, pero investigación más profunda reveló que era Rusia imitando técnicas norcoreanas.

## Ejemplos clásicos (memorizar para el examen)
* **Stuxnet (2011):** Atribuido a EE.UU. + Israel. Sabotaje al programa nuclear iraní, dañando centrifugadoras de enriquecimiento de uranio. Explotó zero-days en Windows, y venció el air-gapping (red aislada) propagándose vía USB infectados.
* **Elecciones EE.UU. 2016:** Ciberataques y campañas de desinformación atribuidos a actores rusos, buscando influir en el proceso electoral.

---

# 📊 Comparación final

| Actor | Sophistication | Resources | Motivación |
| :--- | :--- | :--- | :--- |
| **Script kiddie** | Baja | Bajos | Recognition/chaos |
| **Hacktivist** | Alta (variable) | Variable | Ideológica |
| **Organized crime** | Muy alta | Altos | Financial gain |
| **Nation-state** | Máxima | Máximos | Objetivos estratégicos/geopolíticos (excepto NK: financial) |

---

# 🕳️ Insider Threats

## Definición
Amenazas cibernéticas que se originan dentro de la organización — current/former employees, contractors, o business associates con acceso legítimo a sistemas/datos.

## Por qué son peligrosos
Tienen *intimate knowledge* de la infraestructura interna (layout, procesos, controles de seguridad) que un atacante externo no tiene. 
* *Analogía:* Un extraño intentando entrar a un edificio vs. un empleado descontento que ya tiene badge, conoce el layout y sabe los procedimientos de seguridad — el insider parte con ventaja enorme.

## Capacidad: no depende solo del nivel de acceso
* Determinada por rol + nivel de acceso — un sysadmin con acceso extenso puede causar más daño potencial.
* Pero también depende del skill del individuo: un atacante hábil con cuenta de usuario regular (ej: intern o recepcionista) puede causar más daño que un sysadmin sin skill, aunque tenga menos privilegios por default.

> [!NOTE] Punto clave para el examen
> Acceso alto ≠ capacidad de daño alta si falta skill; skill alto + acceso bajo puede ser igual de peligroso.

## Formas que toma
* Data theft, sabotage, misuse of access privileges.
* Facilitar ataques externos (instalar malware, crear backdoors).
* Puede ser no intencional: ej. empleado sin entrenamiento que clickea un phishing link y compromete la red por descuido/falta de awareness.

## Motivaciones
* **Financial gain:** Vender datos sensibles.
* **Revenge:** Empleado despedido que se lleva/filtra información antes de irse.
* **Carelessness/unintentional:** Falta de training o awareness.

## 🛡️ Mitigación
* Zero Trust architecture.
* Robust access controls.
* Regular audits.
* Employee security awareness training.

"Insider threat" puede ser tanto intencional (Snowden) como accidental (click en phishing) — es a la vez un atributo (interno vs externo) y una categoría de threat actor con sus propias motivaciones y ejemplos.

---

# 🌑 Shadow IT

## Definición
Uso de sistemas, dispositivos, software, apps o servicios de IT sin aprobación explícita de la organización. También conocido como *stealth IT* o *client IT*.

## Por qué existe
Suele surgir cuando la *security posture* de la organización es demasiado estricta o el proceso es muy burocrático para las necesidades operativas.
* *Ejemplo:* Pedir un segundo monitor tardaba 45 días de aprobación → la gente termina comprando su propio monitor y conectándolo por su cuenta, generando shadow IT.

## Formas que toma
* **Hardware no aprobado:** Monitores, USB drives, external hard drives, keyboards, mouse, network adapters — cada uno puede introducir vulnerabilidades sin que nadie se dé cuenta.
* **Software/browser plugins:** Instalados sin aprobación para ganar eficiencia o comodidad.
* **Cloud storage no sancionado:** Dropbox, Google Drive usados para compartir documentos de trabajo sin que IT lo sepa.
* **BYOD (Bring Your Own Device):** Uso de smartphones/tablets/laptops personales para acceder a email o documentos corporativos.

## Riesgos principales
* Data breaches y data leakage.
* Non-compliance con regulaciones.
* System disruptions.
* Falta de standardization en la red → complica la gestión y planificación estratégica.
* **Lifecycle management inexistente:** Si el dispositivo/software falla, IT no puede repararlo porque ni sabe que existe.
* **Troubleshooting mucho más difícil:** Si malware se propaga vía software no autorizado, IT no sabe ni dónde buscar el origen.

> [!IMPORTANT] Punto clave para el examen
> El problema de fondo es simple: si IT no sabe que algo existe en la red, no lo puede asegurar (*"you can't secure what you don't know about"*). Eso es la esencia del riesgo de shadow IT.

## 🛡️ Mitigación
Balance entre flexibilidad/innovación y seguridad: políticas claras sobre qué cloud services están sancionados, procesos de aprobación más ágiles para reducir el incentivo a saltarse los controles.

---

# 🎯 Threat Vectors & Attack Surface

## Definiciones clave

> [!NOTE] Diferencia importante para el examen
> * **Threat vector = el "cómo":** El medio o camino por el cual un atacante gana acceso no autorizado o entrega un payload malicioso.
> * **Attack surface = el "dónde":** La suma de todos los puntos de entrada/salida potenciales que un atacante podría explotar (todas las vulnerabilidades combinadas).

**Mitigación de attack surface:** Restringir acceso, eliminar software innecesario, deshabilitar protocolos no usados. 
* *Ejemplo:* Usar email/IM amplía la attack surface (más vectores de phishing); dispositivos removibles o redes inseguras también la amplían.

## Los 6 threat vectors

1. **Message-based:** Email, SMS, IM. Vehículo típico del phishing: el atacante se hace pasar por entidad confiable para robar info o meter links/adjuntos maliciosos.
2. **Image-based:** Código malicioso embebido en archivos de imagen, se ejecuta al abrir/cargar la imagen.
   * *Ataque Stegano (2017):* Código malicioso oculto en píxeles de banners publicitarios, explotaba vulnerabilidades de Internet Explorer y redirigía a un exploit kit sin que el usuario notara nada.
3. **File-based:** Archivos maliciosos disfrazados de docs/programas legítimos (adjuntos, file-sharing, sitios web). Ejemplo: juegos "crackeados"/pirateados con malware embebido.
4. **Voice call-based (vishing):** Llamadas haciéndose pasar por entidad confiable (banco, IRS) para sacar info sensible por presión/miedo. Ejemplo: falso "IRS" pidiendo SSN o tarjeta bajo amenaza.
5. **Removable device-based:** USB drives, external HDDs/SSDs. Técnica clásica: **baiting** — dejar un USB infectado en un lugar visible (parking, lobby) esperando que alguien lo conecte. También vía social engineering para acceso físico directo.
6. **Unsecure networks:** Wireless, wired, Bluetooth sin protección adecuada:
   * **Wireless:** *Evil twin* — access point falso que imita la red legítima para interceptar tráfico.
   * **Wired:** Acceso físico permite *wiretapping, MAC cloning, VLAN hopping*.
   * **Bluetooth:** *BlueBorne* (conjunto de vulnerabilidades que permite tomar control del dispositivo, propagar malware, o hacer on-path attack sin interacción del usuario) y *BlueSmack* (DoS vía paquete L2CAP malformado que satura recursos y crashea el dispositivo).

> [!NOTE] Para el examen
> Memorizá bien la distinción threat vector vs attack surface (es pregunta clásica), y los nombres específicos de ataques (*Stegano, evil twin, BlueBorne, BlueSmack, baiting*) — CompTIA suele preguntar *"¿qué técnica es esta?"* dando un escenario.

### Ejemplo de aplicación
* **Attack surface:** Email corporativo + WiFi + puertos USB de las PCs + la app web + el sistema de mensajería interna. Todo eso junto es tu superficie de ataque.
* **Threat vector:** El atacante mandó un email de phishing con un link malicioso, y ese fue el camino específico que usó para entrar.

---

# 🍯 Deception & Disruption Technologies

## Concepto base: TTPs
**TTPs (Tactics, Techniques, and Procedures):** El patrón de comportamiento específico de un actor de amenazas. Conocer los TTPs de un atacante permite detectarlo, mitigarlo y contrarrestarlo. Las tecnologías de deception existen justamente para estudiar los TTPs de un atacante en un entorno controlado, sin arriesgar los sistemas reales.

## Las 4 deception technologies

1. **Honeypot:** Sistema/servidor señuelo que imita un sistema real con vulnerabilidades atractivas. Objetivo: no bloquear, sino recolectar info sobre métodos, motivos y TTPs. Sirve también contra insider threats. Se ubica en un *screened subnet* (segmento aislado accesible desde internet).
2. **Honeynet:** Una red completa de honeypots (servers, routers, switches) que imita una red real. Usado por orgs grandes/institutos de investigación para estudiar comportamiento en un entorno más controlado y complejo que un honeypot solo.
   > [!WARNING] Riesgo clave (examen)
   > Es un arma de doble filo — el atacante puede usar el honeynet para aprender cómo está configurada tu red de producción real.
3. **Honeyfile:** Archivo señuelo (doc, spreadsheet, imagen, ejecutable, lo que sea) que parece valioso pero contiene datos falsos + metadata oculta/watermarks. Al abrirse, dispara una alerta — y algunos incluso pueden enumerar la red del atacante. Se colocan con defensas débiles para que sean el objetivo "fácil" antes que los datos reales.
4. **Honeytoken:** Dato/recurso sin uso legítimo cuyo acceso se monitorea (cuenta falsa, URL falsa, registro de DB ficticio). Cualquier interacción = señal clara de compromiso, porque nadie legítimo debería tocarlo. Ejemplo clásico: crear una cuenta "Admin" o "Root" falsa — si alguien intenta loguearse ahí, es un atacante. Muy útil para insider threats.

## Disruption technologies (estrategias adicionales)
* **Fake DNS entries:** Entradas DNS falsas que llevan al atacante a sistemas trampa, hacen perder tiempo/recursos.
* **Decoy directories:** Carpetas falsas que alertan al sistema si alguien intenta acceder/modificarlas.
* **Dynamic page generation:** Contenido que cambia constantemente para confundir/ralentizar crawlers y bots automatizados.
* **Port triggering:** Puertos/servicios permanecen cerrados hasta detectar un patrón específico de tráfico saliente; luego se abren temporalmente. Mantiene servicios invisibles para atacantes que escanean puertos.
* **Fake telemetry data:** Cuando el sistema detecta un scan, responde con datos falsos (ej: reportar Windows 11 cuando en realidad corre macOS) para que exploits dirigidos a un sistema específico fallen.

> [!TIP] Para el examen
> * **Jerarquía de tamaño:** `honeytoken` < `honeyfile` < `honeypot` < `honeynet` (de dato individual a red completa).
> * **Trade-off del honeynet:** Gran fuente de inteligencia, pero también expone patrones de tu arquitectura real si el atacante es lo suficientemente perspicaz.