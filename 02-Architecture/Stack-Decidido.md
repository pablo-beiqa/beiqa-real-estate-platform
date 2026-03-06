# Stack Tecnológico — Dashboard de Decisiones

> **Fecha**: Febrero 2026 | **Actualizado**: 2026-03-05
>
> Cada decisión tiene un ADR (Architecture Decision Record) con justificación completa.
> Ver [ADRs/README.md](ADRs/README.md) para el índice completo.
>
> Este documento reemplaza [archive/Tech-Stack-Decision.md](archive/Tech-Stack-Decision.md), que contenía 11 decisiones "por tomar". Todas están resueltas.

---

## Resumen del Stack

```
Portales inmobiliarios
  Inmuebles24 ──→ Apify (actor contratado)
  Pincali     ──→ Firecrawl + Browserbase ──→ Trigger.dev tasks
  CBRE        ──→ Firecrawl               ──→ Trigger.dev tasks
  Colliers    ──→ Firecrawl + Browserbase ──→ Trigger.dev tasks
                                                    ↓
                                              Supabase (PostgreSQL + PostGIS)
                                              staging tables ──→ Mastra agents ──→ golden record
                                                ↓                    ↓                 ↓
                                          Trigger.dev           LLMs (TBD)        Next.js 15
                                          (sync HubSpot)     Google Maps        (Internal App +
                                                             ArcGIS MCP         Tenant Portal)
                                                                ↓
                                                                ↓
                                              ┌─ Vercel Pro ─────────────┐
                                              │  Next.js 15              │
                                              │  (Internal App +         │
                                              │   Tenant Portal)         │
                                              └──────────────────────────┘

                                              ┌─ Mastra Cloud ───────────┐
                                              │  beiqa-agents            │
                                              │  Hono server + agents    │
                                              │  (beta, gratis)          │
                                              └──────────────────────────┘

                                          Rube + Claude Desktop  ← UI actual (transitorio)
```

---

## Tabla de Decisiones — Stack Activo

| Componente | Decisión | ADR | Estado | Costo/mes |
|-----------|---------|-----|--------|-----------|
| **Plataforma DB** | Supabase (PostgreSQL 15 + PostGIS + Auth + Storage + REST API) | [ADR-001](ADRs/ADR-001-Supabase-Plataforma.md) | ✅ Producción | $25 |
| **Scraping (I24)** | Apify (actor contratado) | [ADR-002](ADRs/ADR-002-Estrategia-Scraping.md) | ✅ Activo | $300 ($150 × 2 corridas/mes) |
| **Scraping (motor)** | Firecrawl (HTTP engine, LLM extraction, stealth proxy) | [ADR-007](ADRs/ADR-007-Firecrawl.md) | ✅ Activo | ~$103 ($1,800 MXN) |
| **Scraping (browser)** | Browserbase (cloud browser sessions) | [ADR-008](ADRs/ADR-008-Browserbase.md) | ✅ Activo | $0–20 (TBD) |
| **Ejecución Durable** | Trigger.dev (scrapers, persistencia, cron, sync HubSpot — NO AI) | [ADR-003](ADRs/ADR-003-Trigger-dev.md) | ✅ Activo | $50 |
| **AI Agent Orchestration** | Mastra (agents, workflows, tools, memory, MCP) | [ADR-020](ADRs/ADR-020-Mastra.md) | 🟢 En implementación | $0 (framework) + LLM TBD |
| **AI Processing** | LLMs vía Mastra (modelo TBD por agente — requiere testing) | [ADR-020](ADRs/ADR-020-Mastra.md) | 🟡 TBD | ~$67–88 (estimado) |
| **UI actual** | Rube + Claude Desktop (MCP bridge) | [ADR-004](ADRs/ADR-004-Rube-MCP-Bridge.md) | ✅ Activo (transitorio) | $75–100 (Rube $25 + Claude $25 × 2–3) |
| **CRM** | HubSpot (clientes, deals, pipeline comercial) | [ADR-005](ADRs/ADR-005-HubSpot-CRM.md) | ✅ Activo | No atribuido (costo general Beiqa) |
| **Monitoreo** | Slack + tabla `error_logs` | [ADR-006](ADRs/ADR-006-Monitoreo.md) | ✅ Activo | $0 |
| **Geoespacial (H3)** | h3-js — indexación hexagonal capas 5-11 | [ADR-009](ADRs/ADR-009-H3-Indexing.md) | 🟡 En pruebas | $0 |
| **Geoespacial (AGEB)** | Polígonos INEGI para análisis territorial | [ADR-010](ADRs/ADR-010-AGEB-INEGI.md) | 🔴 Decidido, por implementar | $0 |
| **Geocodificación** | Google Maps Platform (Geocoding + Places API) | [ADR-011](ADRs/ADR-011-Google-Maps-Platform.md) | ✅ Activo | ~$0 (crédito $200) |
| **Data architecture** | Staging tables por portal + golden record `properties` | [ADR-012](ADRs/ADR-012-Multi-Portal-Data.md) | ✅ Decidido | $0 |
| **Frontend** | Next.js 15 + App Router + TypeScript + Tailwind + shadcn/ui | [ADR-015](ADRs/ADR-015-Frontend-Next-js.md) | 🟡 Phase 0 completo | — |
| **Frontend Hosting** | Vercel Pro (portal.beiqa.com + app.beiqa.com) | [ADR-022](ADRs/ADR-022-Hosting-Vercel-Mastra-Cloud.md) | 🟡 Por deployar | $20 |
| **AI Agent Hosting** | Mastra Cloud (beta — observabilidad, Studio, deploy) | [ADR-022](ADRs/ADR-022-Hosting-Vercel-Mastra-Cloud.md) | 🔴 Por implementar | $0 (beta, pricing TBD) |

---

## Decisiones Propuestas (Fase 2-3)

| Componente | Opciones | ADR | Cuándo decidir |
|-----------|---------|-----|---------------|
| **AI Routing** | OpenRouter (multi-modelo) | [ADR-013](ADRs/ADR-013-OpenRouter.md) | Con Mastra activo — evaluar si OpenRouter o API directa |
| **Deduplicación** | Scoring + LLM-assisted (híbrido) vía Mastra agent | [ADR-016](ADRs/ADR-016-Deduplicacion.md) | Con ≥2 portales en staging |
| **Mapas GIS** | Atlas.co (API + embed) — $89/usuario | [ADR-017](ADRs/ADR-017-Plataforma-GIS.md) | ✅ Activo (2–3 usuarios, $178–267/mes) |
| **CI/CD** | GitHub Actions (CI: lint, tests) — Deploy: resuelto por [ADR-022](ADRs/ADR-022-Hosting-Vercel-Mastra-Cloud.md) | [ADR-018](ADRs/ADR-018-CI-CD.md) | Con frontend activo |

---

## Tecnologías Deprecadas / Descartadas

| Tecnología | Status | ADR / Razón |
|-----------|--------|-------------|
| **n8n Cloud** | ❌ Deprecado | [ADR-019](ADRs/ADR-019-n8n-Deprecado.md) — todo migrado a Trigger.dev |
| **Clay** | ⚠️ Transitorio, saliendo | Lógica se replica en Trigger.dev tasks |
| **Backboard.io** | ❌ Supersedido | [ADR-014](ADRs/ADR-014-Backboard.md) — Mastra memory reemplaza ([ADR-020](ADRs/ADR-020-Mastra.md)) |
| **EasyBroker** | ❌ Descartado como portal | [ADR-002](ADRs/ADR-002-Estrategia-Scraping.md) — portal no viable |
| **FastAPI / Express** | ❌ No necesario | Supabase genera REST API automática |
| **Auth0 / Clerk** | ❌ No necesario | Supabase Auth incluido |
| **Redis / cache** | ❌ No necesario (hoy) | 60K listings + 4 usuarios → PostgreSQL aguanta |
| **GraphQL** | ❌ No necesario | REST auto-generado es suficiente |
| **Scrapy / Python** | ❌ No necesario | Apify + Firecrawl cubren todo |
| **Sentry / Datadog** | ❌ No necesario (hoy) | Slack + error_logs suficiente |
| **AWS S3 / Cloudflare R2** | ❌ No necesario | Supabase Storage incluido |

---

## Source of Truth Rules

> Principio de Alex (llamada 23 feb): "Mientras menos fuentes del mismo dato tengan, mucho mejor."

| Sistema | Source of truth para... | Recibe datos de... |
|---------|--------------------------|-------------------|
| **Supabase** | Propiedades, listings, brokers, analytics, geo data, golden record | Apify, Trigger.dev (scrapers), Mastra (enrichment), Google APIs |
| **HubSpot** | Clientes, deals, comunicación comercial | Supabase (sync one-way vía Trigger.dev) |
| **Mastra** | NO es source of truth — lee y escribe a Supabase | Supabase (lee staging), Google APIs, ArcGIS MCP, LLMs |

**Sincronizaciones:**
- Trigger.dev → Supabase: datos crudos de scraping a staging tables
- Mastra → Supabase: datos enriquecidos al golden record (`properties`)
- Supabase → HubSpot: propiedades y brokers (one-way, vía Trigger.dev)
- HubSpot → Supabase: deal status (one-way, minimal)
- Trigger.dev → Mastra: HTTP POST post-scrape (trigger enrichment)

---

## Decisiones Diferidas (no implementar aún)

| Tema | Cuándo implementar | Razón para diferir |
|------|-------------------|-------------------|
| Separar DB operativa / analítica | Cuando jobs compitan con búsquedas operativas | 60K listings + 4 usuarios no lo requieren |
| Supabase read replica | Cuando haya carga analítica real | Prematuro hoy |
| Schema `analytics` con materialized views | Cuando haya reportes complejos recurrentes | Prematuro hoy |
| Redis cache | Cuando el volumen lo justifique | No necesario hoy |

---

*Documento actualizado: 2026-03-05 | Vercel y Mastra Cloud agregados como hosting ([ADR-022](ADRs/ADR-022-Hosting-Vercel-Mastra-Cloud.md)). Ver [Total-Budget.md](../04-Validation/Total-Budget.md) para el desglose completo.*
