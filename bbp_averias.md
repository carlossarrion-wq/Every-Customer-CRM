---
id: bbp_averias
namespace: ns_bbps
title: "BBP — Proceso Averías (Incidencias en la red)"
type: business_blueprint
process_group: SAC
customer_journey: "Incidencias en la red - Averías"
tags: [BBP, averias, incidencias-red, COR, ADMS, proceso-negocio, aviso-averia, reenganche, electrodependiente, notificaciones-proactivas, TO-BE]
source_documents:
  - "G07_E235 Business Blueprint (Averias) V6.pdf"
related_docs: [bbp_sac_general, bbp_danos, bbp_sac_calidad, bbp_operaciones_red, bbp_pds]
status: available
priority: tier1_critical
created: "2026-09-02"
---

# BBP — Proceso Averías (Incidencias en la red)

> **Business Blueprint (proceso de negocio).** Describe el *customer journey* de **Gestión de Incidencias en la red — Averías**: la gestión del contacto del cliente por una avería, la evaluación guiada, la creación y seguimiento del aviso de avería, la coordinación con el COR y la comunicación con el cliente hasta el cierre.
>
> Este proceso **arranca donde termina el SAC General** (`bbp_sac_general`): comienza cuando una solicitud ya ha sido clasificada dentro de "Incidencias en la red — Averías". Describe **qué hace** el proceso; cuando se apoya en el sistema de gestión se habla de **"nuevo CRM"** o **"sistema CRM"**.

---

## 1. Resumen ejecutivo

### 1.1 Qué es la gestión de averías

La gestión de **Incidencias en la red — Averías** cubre el tratamiento de las interrupciones o anomalías del suministro eléctrico, tanto las reportadas por el cliente como las detectadas de forma remota por UFD. El modelo TO-BE establece un proceso homogéneo, trazable y centrado en la calidad del servicio, que va desde la identificación de un incidente como avería hasta su resolución y comunicación al cliente.

Se distinguen tres elementos que se gestionan de forma coordinada:
- **Incidencia técnica en red** (gestionada en **ADMS**, sistema maestro de la información de incidencias).
- **Aviso de Avería** (registro operativo para el tratamiento técnico, gestionado en ADMS).
- **Caso** (en el nuevo CRM): punto de relación y seguimiento con el cliente. El CRM centraliza la relación con el cliente y consume del maestro (ADMS) la información necesaria. El Caso equivale funcionalmente a la SR actual, pero amplía su alcance a la gestión end-to-end.

### 1.2 Alcance

Desde el inicio del contacto (una vez clasificado como avería) hasta la finalización de la gestión: cierre del Caso informativo o, cuando procede actuación técnica, cierre del Aviso de Avería o de la incidencia masiva. Incluye recepción y registro, evaluación y aplicación de reglas, gestión y seguimiento del aviso, mecanismos de reintento/contingencia, resolución, cierre coordinado del Caso y comunicaciones proactivas.

**Exclusiones:** la gestión inicial del contacto (en `bbp_sac_general`), los procedimientos técnicos internos de resolución en campo y los propios del COR, el diseño tecnológico, el árbol de evaluación detallado (a Diseño), y el contenido concreto de mensajes y plantillas.

---

## 2. Actores y roles de negocio

| Rol | Responsabilidad |
|---|---|
| **Cliente / tercero** | Reporta el evento de avería, aporta información y recibe comunicaciones. Puede ser el titular, un tercero afectado o un interlocutor válido (incluidos cuerpos de seguridad, emergencias, autoridades). |
| **Plataforma Telefónica / AVI / Canales Digitales (PDyAT)** | Punto inicial de contacto y **gobierno del proceso** end-to-end. Recepción y preclasificación, ejecución del flujo guiado, y puede generar directamente el Aviso de Avería. |
| **COR (Centro de Operación de Red)** | Gestión **técnica** de las averías y operación de la red en tiempo real. Recibe los avisos vía SGI/ADMS, los gestiona, deriva a las brigadas (EECC) mediante Flexi-App y resuelve el aviso, garantizando la trazabilidad técnica. |
| **OpdS (Operaciones de Punto de Suministro)** | Actuaciones operativas sobre el punto de suministro; asume, en horario comercial, los reenganches asociados a procesos de corte. |
| **EECC (Empresas Colaboradoras de Campo) / brigadas** | Ejecutan la intervención física y reportan el resultado al COR (p. ej. vía Flexi-App). |
| **Unidad de Procesos** | Marco funcional y normativo; mejora continua. |

---

## 3. Canales de entrada

Agente Virtual (AVI), plataforma telefónica, chat web, WhatsApp y PDS. Además, **detección remota por el COR** (sin llamada del cliente) y **avisos de terceros** o **sin CUPS** (incidencias no asociables a un suministro identificado).

En canales digitales, el AVI actúa como primer nivel: si resuelve, crea el Caso con trazabilidad y lo cierra; si no, deriva a un agente con el contexto (resumen + datos capturados).

---

## 4. Flujo del proceso TO-BE

1. **Entrada del caso**: registro de la interacción, identificación del cliente y asociación del CUPS afectado cuando aplique. Verificación automática de **reiteraciones/recontactos** (mismo CUPS en ventana temporal, incidencias previas marcadas como resueltas, patrones detectados por AVI). El caso se marca como reiterado (automática o manualmente) elevando su prioridad.
2. **Gestión del Caso y evaluación guiada**: el agente accede al Caso con la Ficha 360 de Cliente y de CUPS, resumen de la interacción previa y *quick actions*. Ejecuta el **flujo guiado de averías**: una batería de preguntas, unas resueltas automáticamente por el sistema (existencia de aviso/incidencia en curso, electrodependiente, telegestionado, estado del contador, impago…) y otras manuales por el agente (situación de peligro, corte justificado/erróneo, suministro en la calle, ICP desarmado, etc.).
3. **Contraste con incidencias/avisos existentes**:
   - **Sin incidencia ni aviso** → evaluación normal; si procede, se crea aviso nuevo y se mantiene el Caso abierto.
   - **Incidencia activa sin aviso previo** → se informa al cliente, se crea el aviso asociado y se vincula el Caso.
   - **Incidencia activa con aviso en curso** → no se crea nuevo aviso ni nuevo caso; se registra la interacción y se contabiliza la reiteración. Excepción de seguridad: si hay riesgo o agravamiento, se escala el aviso existente (no se crea uno nuevo) notificando al COR.
4. **Resultado del flujo guiado**:
   - **A. Resolución sin aviso**: rearme de ICP con recuperación de servicio, instrucciones al cliente, o incidencia atribuible a la instalación particular → se cierra el Caso.
   - **B. Creación de Aviso de Avería**: electrodependiente, situación de seguridad, contador incorrecto, corte por impago erróneo o incidencia atribuible a la red UFD → se crea el aviso técnico para el COR y se mantiene el Caso.
   - **C. Emergencia**: riesgo para personas/bienes → se crea aviso, se mantiene Caso y el agente realiza **llamada telefónica directa al COR** (obligatoria, trazada por el sistema).
5. **Gestión técnica, seguimiento y cierre**: el COR asume la gestión operativa; los cambios de estado del aviso/incidencia se reflejan en el Caso vía integración con ADMS. La resolución es **oficial solo cuando la confirma el COR** en los sistemas corporativos (ADMS); el CRM no cierra ni comunica cierre basándose en estados intermedios (p. ej. Flexi-App). El cierre del Caso es **semiautomático** (el sistema propone, el agente confirma), salvo cierres automáticos definidos (p. ej. resolución end-to-end por AVI). Una incidencia que afecta a múltiples CUPS permite el **cierre coordinado masivo** de todos los casos vinculados.
6. **Encuesta**: tras el cierre, encuesta específica de averías (diferenciada de la encuesta de atención).

### 4.1 Subproceso de Reenganches

Reenganches de suministro asociados a procesos de corte (normalmente por impago) derivados de contacto del cliente. El sistema consulta el estado del suministro (SAP IS-U, ZEUS, SGC/SAGE) y la existencia de **Orden de Servicio (O/S)** de reenganche, y calcula el **Fuera de Plazo (FH)** = fecha de generación de la O/S + 24 h vs. fecha actual. Escenarios: **dentro de plazo** (sin aviso al COR, caso en curso asignado a OpdS en horario comercial), **fuera de plazo** (aviso al COR, caso prioritario abierto hasta resolución; el fin de semana se considera fuera de plazo), y **electrodependiente** (aviso directo al COR sin O/S previa, caso prioritario).

### 4.2 Subproceso de Notificaciones Proactivas

Ante incidencias con impacto en colectivos de clientes: identificación de CUPS y clientes afectados, y orquestación de la comunicación desde el CRM (ejecución de envíos por el gestor de comunicaciones del CRM). Activadores: interrupciones señaladas por el COR en ADMS (masivas o con impacto relevante) y **preavisos de cortes programados con 3 días de antelación** (planificación en SEPLO; ADMS dispone de la información en tiempo real en el momento del corte). No genera Caso (es comunicación iniciada por la compañía). Debe respetar el estado de suscripción del cliente (opt-in/opt-out) y evitar solapamientos.

---

## 5. Estados del Caso/Aviso

Estados citados en el proceso: **En curso**, **En evaluación**, **Pendiente de actuación técnica**, **Cerrado**, **Pendiente de información**, **Pendiente de transmisión** (aviso registrado durante contingencia), **Reiterado**. Estados del contador telegestionado: **Estado 0 / 1 / 2** y "Activo con suministro cortado". Los estados y subestados (sucesos) definitivos se concretan en la fase de Diseño.

---

## 6. Reglas de negocio

- **RN-AV-01** — El proceso arranca solo cuando la solicitud ya ha sido clasificada como "Incidencias en la red — Averías" (la gestión inicial está en SAC General).
- **RN-AV-02** — Los Agentes Virtuales (AVI) deben integrarse con el nuevo CRM, ingestando la información para la apertura/registro del caso y consumiendo datos del CRM en tiempo real vía APIs estándar.
- **RN-AV-03** — El flujo de evaluación de averías sigue el árbol de decisión vigente (con actualizaciones a definir en Diseño), incluyendo casos de recuperación inmediata del suministro sin generar aviso (p. ej. rearme del ICP en contadores en Estado 2).
- **RN-AV-04** — Las reiteraciones sobre incidencias o avisos existentes se gestionan como contactos informativos, se registran y contabilizan, y **no** generan nuevos avisos salvo cambio relevante en la situación.
- **RN-AV-05** — Casos que requieren actuación operativa no técnica (p. ej. corte por impago con reenganche en horario comercial) se mantienen en curso y se desvían a **OpdS**, sin generar Aviso de Avería técnico.
- **RN-AV-06** — Plazo regulatorio de reposición del reenganche: la O/S + 24 h define el **Fuera de Plazo (FH)**; el fin de semana se considera fuera de plazo.
- **RN-AV-07** — Ante incidencias técnicas que impidan la gestión estándar, se aplican reintentos configurables; la **contingencia** no se activa de forma automática, sino por decisión operativa consensuada entre Plataforma y COR, y de forma generalizada (nunca por caso individual).
- **RN-AV-08** — Toda la información mínima obligatoria del cliente debe estar completa antes de avanzar a la siguiente etapa.
- **RN-AV-09** — Solo se avanza en la resolución cuando los criterios de evaluación de avería están validados según las normas internas.
- **RN-AV-10** — Todas las acciones y decisiones críticas se registran para asegurar visibilidad y trazabilidad (registro de actividades, visión 360).
- **RN-AV-11** — Todas las acciones del agente se ejecutan desde un único punto centralizado (consola).
- **RN-AV-12** — Las averías con impacto alto o urgencia definida se priorizan sobre casos estándar, respetando los SLA; existe una priorización por tipos de aviso.
- **RN-AV-13** — Los casos gestionados mediante formularios de contingencia deben registrar la información mínima; a la vuelta a la normalidad, los avisos/casos de contingencia se incorporan (masiva o bajo demanda) al circuito estándar para mantener la trazabilidad.
- **RN-AV-14** — Cada caso tiene responsable y plazos de seguimiento alineados con SAC General.
- **RN-AV-15** — Las comunicaciones al cliente se registran de forma trazable en el historial.
- **RN-AV-16** — La resolución se considera oficial únicamente cuando la confirma el COR vía ADMS; el CRM no actualiza a resuelto ni comunica cierre por estados intermedios.
- **RN-AV-17** — La llamada al COR, cuando el flujo la indica, es **obligatoria** para el agente (no discrecional) y queda trazada automáticamente.
- **RN-AV-18** — Excepción controlada: se puede forzar la creación de un Aviso aunque el flujo concluya que no procede, siempre con justificación explícita y trazable (idealmente motivo estructurado).
- **RN-AV-19** — El cierre del Caso solo se produce una vez la incidencia está resuelta, nunca antes; para clientes electrodependientes se aplica tratamiento diferenciado y prioritario.

---

## 7. Sistemas de negocio implicados

| Sistema | Rol |
|---|---|
| **ADMS (Advanced Distribution Management System)** | Sistema maestro de incidencias y avisos de avería; fuente de estados que el CRM consume. |
| **Nuevo CRM** | Centraliza la relación con el cliente, gestiona el Caso, las comunicaciones y la trazabilidad. |
| **FlexiApp** | Información de avance de la contrata/brigada (aviso en cola, salida, llegada, resolución); estados intermedios no oficiales. |
| **SEPLO** | Registro y planificación de cortes programados. |
| **GMO (Grid Metering Operations)** | Gestión de contadores telegestionados (OpdS); lectura y envío de órdenes (p. ej. rearme de ICP). |
| **ZEUS / SAP IS-U / SGC-SAGE / SCTD** | Estado del suministro, operaciones de corte/reenganche y comunicación con comercializadoras. |
| **AVI** | Asistente virtual; primer nivel de atención. |
| **PDS** | Canal de entrada adicional y visor de información en tiempo real al cliente. |
| **Gestor de Comunicaciones (CRM)** | Ejecución de los envíos de comunicaciones (proactivas y de cierre). |

> Nota sobre indemnizaciones: el BBP no fija importes ni fórmulas; la única referencia económica es un "warning por abono/compensación en factura" visible al agente. El foco regulatorio explícito es el plazo de reposición de 24 h.

---

## 8. Comunicaciones con cliente

- **Proactivas**: notificación en incidencias masivas/relevantes y preaviso de cortes programados (3 días); seguimiento periódico mientras la incidencia siga abierta, evitando contactos innecesarios.
- **De hito**: inicio de trabajos, asignación de brigada, restablecimiento, resolución definitiva.
- **De cierre**: comunicación final de resolución + encuesta.
- Cada comunicación se registra en la ficha del cliente (fecha/hora, canal, contenido, estado de envío) y respeta la suscripción (opt-in/opt-out).

---

## 9. Gobierno, riesgos y dependencias

- **Gobierno**: proceso con propiedad distribuida — Atención al Cliente (contacto y parte administrativa), COR (gestión técnica), Unidad de Procesos (marco funcional). Matriz RACI definida por actividad.
- **Riesgos** (con mitigación): caída del CRM (supervisión de la salud de la plataforma), caída de ADMS (colas de mensajes, consulta alternativa, reintentos y reconciliación), y caída de los sistemas de lectura de factores del flujo (timeouts y *fallback* controlado, estado "pendiente de información").
- **Dependencias**: proceso **SAC General** (vías de entrada/salida y comunicaciones) y proceso **PDS** (entrada adicional y visor en tiempo real).

---

## Referencias

- **Documento fuente:** `G07_E235 Business Blueprint (Averias) V6.pdf` (E235, v6, 19/03/2026). Texto extraído en `ns_bbps_raw/G07_bbp_averias.txt`.
- **Proceso paraguas:** `bbp_sac_general`.
