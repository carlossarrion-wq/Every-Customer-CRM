---
id: bbp_canales_atencion_derived
namespace: ns_bbps
title: "Canales de Atención (derivado, transversal) — sustento de US-02.2"
type: business_blueprint
process_group: SAC
derived: true
tags: [canales, omnicanal, CTI, telefonia, videoasistencia, chat, WhatsApp, SMS, US-02, transversal, derivado, SAC]
related_docs: [bbp_sac_general, bbp_comunicaciones_derived, bbp_encuestas_derived]
status: available
priority: tier2_important
source_documents:
  - "Anexo G_RFP (apartado 6.2.2 Canales de Atención)"
  - "BBP E235 SAC General (G06): interacción multicanal, canal preferido (RN-SAC-20), omnicanalidad"
created: "2026-09-02"
version: "1.0"
---

# Canales de Atención (derivado, transversal)

> 🧩 **BBP DERIVADO (sufijo `_derived`).** No es una depuración 1:1 de un BBP E235 de la RFP: el pliego pide los Canales de Atención en **US-02.2** pero **no los aisló en un BBP propio** (el requisito está embebido en el BBP de SAC General). Este documento —**elaborado por IBM**— consolida ese requisito transversal para dar soporte funcional a US-02.2.
>
> Describe **qué canales** de atención existen, cómo se integran con el CRM y qué principios operativos aplican, con independencia de la tecnología. Los sistemas de canal concretos se documentan en `../ns_systems_asis/`.

---

## 1. Alcance

Modelo de **atención multicanal integrado** con el CRM y con la gestión de casos (SR) de SAC General. Cubre los canales de entrada, interacción y conversación, con enfoque **omnicanal** (continuidad y conservación de contexto entre canales) y apoyo de IA cuando aporta valor.

**Relación con otros BBP:**
- `bbp_sac_general` — proceso paraguas que consume los canales para la gestión de casos (interacción multicanal, canal preferido RN-SAC-20, omnicanalidad §8).
- `bbp_comunicaciones_derived` — el envío saliente de comunicaciones por canal (catálogo G46).
- `bbp_encuestas_derived` — encuestas por canal al cierre.

## 2. Canales en alcance (US-02.2)

| Canal | Descripción funcional | Apoyo IA | Sistema (AS-IS / oferta) |
|-------|-----------------------|----------|--------------------------|
| **Videoasistencia remota** | Atención remota con soporte visual (asistencia avanzada, validaciones); trazabilidad e integración en el flujo de atención vinculado a la SR | — | Capacidad del CRM (a definir en diseño) |
| **Telefonía y CTI** | Integración del canal telefónico con el CRM: llamadas entrantes/salientes, enrutamiento y distribución; screen pop y click-to-call; registro de la interacción de voz vinculada al caso | Clasificación inicial, apoyo al agente, priorización | `../ns_systems_asis/sys_11_Amazon_Connect_CTI.md` (Amazon Connect) |
| **Mensajería / chat digital** | Chat, mensajería instantánea y canales equivalentes; conversación entrante y saliente | Mensajería asistida por IA (1er nivel) | `../ns_systems_asis/sys_08_sistemas_externos.md` (WhatsApp Meta) |
| **SMS** | Avisos cortos, OTP y fallback de canal | — | `../ns_systems_asis/sys_08_sistemas_externos.md` (Gateway SMS) |
| **Canal digital (PDS)** | Punto de entrada y visualización del canal on line (área privada) | — | `bbp_pds.md` (US-05) |

> El canal de **voz (CTI)** es un sistema preexistente (Amazon Connect); **WhatsApp** y **SMS** son proveedores externos; todos se integran vía **MuleSoft**.

## 3. Principios operativos (RFP)

- **Gestión unificada** de los distintos canales, integrada con el CRM y los procesos de Atención al Cliente.
- **Trazabilidad completa** de las interacciones, independientemente del canal de entrada (cada interacción se vincula a un Caso/Expediente).
- **Omnicanalidad:** continuidad de la conversación y **conservación del contexto** entre canales (ficha 360º accesible a todos los agentes).
- **Canal preferido del cliente** (coherente con RN-SAC-20) y asignación al canal más adecuado por tipología de cliente.
- **Escalado fluido** entre atención automatizada (IA) y atención por agente humano.
- **Uso progresivo de IA/automatización** sin comprometer la calidad del servicio.
- **Flexibilidad** para evolucionar y ampliar los canales soportados en el tiempo.

## 4. Cobertura desde la oferta (Wattyo)

Módulo **CRM** de Wattyo (atención omnicanal con conservación de contexto entre canales) + integración de los sistemas de canal (Amazon Connect para voz/CTI; WhatsApp Meta y Gateway SMS para mensajería) vía MuleSoft. Ver `../ns_wattyo_crm_foundation/` y `../ns_wattyo_based_architecture/`.

## 5. Entregables (US-02.2)

- Configuración/desarrollo de las capacidades de videoasistencia, telefonía/CTI y canales digitales.
- Integración de los canales con el CRM y el modelo de solicitudes (SR).
- Definición de flujos de atención y criterios de enrutamiento.
- Documentación funcional/técnica; evidencias de pruebas funcionales y de rendimiento; soporte a producción y estabilización.

## Referencias
- US-02: `../ns_project_context/us/us_02_atencion_cliente.md` · Matriz (GAP-T2): `../ns_project_context/06_matriz_trazabilidad_us_bbp.md`.
- Sistemas de canal: `../ns_systems_asis/sys_11_Amazon_Connect_CTI.md`, `../ns_systems_asis/sys_08_sistemas_externos.md`.
- Reglas transversales: `bbp_sac_general.md` (interacción multicanal, RN-SAC-20).
