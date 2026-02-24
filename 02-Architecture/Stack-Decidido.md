# Stack Tecnológico — Decisiones Tomadas

> **Fecha**: Febrero 2026 | **Estado**: ✅ Todo decidido e implementado
>
> Este documento reemplaza [Tech-Stack-Decision.md](Tech-Stack-Decision.md), que contenía 11 decisiones "por tomar". Todas están resueltas.

---

## Resumen del Stack

```
Portales inmobiliarios
        ↓
     Apify          ← scraping actor contratado
        ↓
      n8n            ← webhooks, cron, integraciones visuales
        ↓
    Supabase         ← PostgreSQL + PostGIS + Auth + Storage + REST API
        ↓         ↘
  Trigger.dev    HubSpot   ← jobs pesados de código + CRM
        ↓
   Claude API       ← NLP, extracción de features, procesamiento
        ↓
    Next.js          ← Internal App + Tenant Portal (Pamela)
```

---

## Tabla de Decisiones

| Componente | Decisión | Justificación | Estado | Costo |
|-----------|---------|--------------|--------|-------|
| **Scraping** | Apify (actor contratado) | Maneja rate limiting, proxies, anti-bot internamente. Sin código Python de scraping. | ✅ Activo | ~$20/mes |
| **Orquestación visual** | n8n Cloud | Webhooks, cron jobs, integraciones sin código, notificaciones Slack. Ideal para flujos de integración. | ✅ Activo | Incluido en plan |
| **Jobs pesados** | Trigger.dev | TypeScript nativo, batch AI extraction con Claude API, procesamiento de miles de descripciones, futuro parsing de transcripts. | ✅ Activo | Free tier |
| **Base de datos** | Supabase (PostgreSQL 15 + PostGIS) | All-in-one: DB + Auth + Storage + REST API automático + RLS. PostGIS para geoespacial. 14 migrations implementadas. | ✅ En producción | $0-25/mes |
| **Backend API** | Supabase REST automático + RPC functions | No hay backend custom (sin FastAPI, sin Express). La API la genera Supabase automáticamente desde el schema. | ✅ Activo | Incluido en Supabase |
| **AI / NLP** | Claude API (vía Rube) | ~3x más barato que OpenAI. Mejor calidad en español. Procesamiento de descripciones, extracción de features. | ✅ Activo | Por tokens |
| **Frontend** | Next.js | Decisión tomada, Pamela asignada. Internal App + Tenant Portal en el mismo stack. | 🟡 En diseño | — |
| **Auth** | Supabase Auth | Incluido en Supabase. Sin Auth0, sin Clerk, sin JWT custom. | ✅ Decidido | Incluido |
| **CRM** | HubSpot | Operando. Sync de propiedades/brokers vía n8n. Source of truth para clientes y deals. | ✅ Activo | Plan existente |
| **Data enrichment** | Clay | Enriquece datos de brokers y empresas antes de ingresar a HubSpot. | ✅ Activo | Plan existente |
| **Object storage** | Supabase Storage | Incluido en Supabase. Sin AWS S3, sin Cloudflare R2. | ✅ Decidido | Incluido |
| **Cache** | No necesario (por ahora) | Con ~60K listings y 4 usuarios concurrentes, Supabase aguanta sin cache. Se revisará cuando los jobs de Trigger.dev generen carga analítica real. | ✅ Decidido | $0 |
| **Monitoreo** | Slack + n8n logs + tabla `error_logs` | Suficiente para el equipo y volumen actual. Sin Sentry, sin Datadog, sin Grafana. | ✅ Activo | $0 |
| **Geoespacial** | PostGIS (incluido en Supabase) | Geography points, índices GIST, spatial joins. Trigger `populate_geo` activo. H3 index planificado para Fase 2. | ✅ Activo (básico) | Incluido |
| **CI/CD** | Por implementar | GitHub Actions cuando el frontend esté activo. | 🔴 Por implementar | — |
| **Call processing** | CircleBack (futuro) | Transcripción de llamadas con clientes. Fase 3. | 🔴 Fase 3 | — |

---

## División de responsabilidades: n8n vs Trigger.dev

Esta es una decisión arquitectónica importante que no estaba documentada.

### n8n maneja:
- Webhooks de entrada (Slack trigger para scraping manual)
- Cron jobs (ejecución semanal del scraper)
- Notificaciones a Slack
- Sync HubSpot ← Supabase (propiedades, brokers)
- Integraciones visuales sin código complejo

### Trigger.dev maneja:
- Batch extraction con Claude API (procesar miles de descripciones)
- Procesamiento de `property_features` con AI
- Jobs que requieren TypeScript / lógica compleja
- Futuro: parsing de transcripts de llamadas (CircleBack)

**Regla simple**: Si el flujo es visual o de integración → n8n. Si requiere código TypeScript + lógica compleja o AI en batch → Trigger.dev.

---

## Source of Truth Rules

Recomendación de Alex (llamada 23 feb 2026): "Mientras menos fuentes del mismo dato tengan, mucho mejor."

| Sistema | Es source of truth para... | Recibe datos de... |
|---------|--------------------------|-------------------|
| **Supabase** | Propiedades, listings, brokers, analytics, geo data | Apify (vía n8n), Trigger.dev |
| **HubSpot** | Clientes, deals, comunicación comercial | Supabase (sync one-way vía n8n), Clay |

**Sincronización Supabase → HubSpot**: One-way para propiedades y brokers.
**Sincronización HubSpot → Supabase**: One-way para deal status.
**Clay**: Enriquece datos que van a HubSpot, no a Supabase directamente.

---

## Decisiones diferidas (no implementar aún)

| Tema | Cuándo implementar | Razón para diferir |
|------|-------------------|-------------------|
| Separar DB operativa / analítica | Cuando Trigger.dev jobs compitan con búsquedas operativas | 60K listings + 4 usuarios no lo requieren |
| Supabase read replica | Cuando haya carga analítica real | Prematuro hoy |
| Schema `analytics` con materialized views | Cuando haya reportes complejos recurrentes | Prematuro hoy |
| Redis cache | Cuando el volumen lo justifique | No necesario hoy |

---

## Costos actuales (febrero 2026)

| Herramienta | Costo mensual |
|-----------|--------------|
| Apify | ~$20/mes |
| Supabase | $0-25/mes |
| n8n | Incluido en plan |
| Trigger.dev | Free tier |
| Claude API (Rube) | Por tokens (~mínimo) |
| HubSpot | Plan existente |
| **Total herramientas** | **~$30-50/mes** |

> Comparado con el presupuesto original estimado de $50K-$100K para desarrollo outsource. El costo real es ~$30-50/mes en herramientas con desarrollo interno.

---

*Documento creado: 2026-02-24 | Reemplaza: Tech-Stack-Decision.md*
