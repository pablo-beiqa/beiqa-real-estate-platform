# Stack Tecnológico — Dashboard de Decisiones

> **Fecha**: Febrero 2026 | **Actualizado**: 2026-03-02
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
                                                ↓           ↓
                                          Trigger.dev    HubSpot (CRM)
                                          (sync, AI)
                                                ↓
                                          Claude API (vía Rube)
                                                ↓
                                          Rube + Claude Desktop  ← UI actual (Fase 1)
                                          Next.js 15 (beiqa-frontend) ← Tenant Portal (Fase 2)
```

---

## Tabla de Decisiones — Stack Activo

| Componente | Decisión | ADR | Estado | Costo/mes |
|-----------|---------|-----|--------|-----------|
| **Plataforma DB** | Supabase (PostgreSQL 15 + PostGIS + Auth + Storage + REST API) | [ADR-001](ADRs/ADR-001-Supabase-Plataforma.md) | ✅ Producción | $25 |
| **Scraping (I24)** | Apify (actor contratado) | [ADR-002](ADRs/ADR-002-Estrategia-Scraping.md) | ✅ Activo | $300 ($150 × 2 corridas/mes) |
| **Scraping (motor)** | Firecrawl (HTTP engine, LLM extraction, stealth proxy) | [ADR-007](ADRs/ADR-007-Firecrawl.md) | ✅ Activo | ~$103 ($1,800 MXN) |
| **Scraping (browser)** | Browserbase (cloud browser sessions) | [ADR-008](ADRs/ADR-008-Browserbase.md) | ✅ Activo | $0–20 (TBD) |
| **Automatización** | Trigger.dev (scrapers, sync, limpieza, cron, AI batch) | [ADR-003](ADRs/ADR-003-Trigger-dev.md) | ✅ Activo | $50 |
| **AI Processing** | OpenRouter (GPT-4o-mini para extracción de campos) | [ADR-013](ADRs/ADR-013-OpenRouter.md) | ✅ Activo | $15–30 |
| **UI actual** | Rube + Claude Desktop (MCP bridge) | [ADR-004](ADRs/ADR-004-Rube-MCP-Bridge.md) | ✅ Activo (transitorio) | $75–100 (Rube $25 + Claude $25 × 2–3) |
| **CRM** | HubSpot (clientes, deals, pipeline comercial) | [ADR-005](ADRs/ADR-005-HubSpot-CRM.md) | ✅ Activo | No atribuido (costo general Beiqa) |
| **Monitoreo** | Slack + tabla `error_logs` | [ADR-006](ADRs/ADR-006-Monitoreo.md) | ✅ Activo | $0 |
| **Geoespacial (H3)** | h3-js — indexación hexagonal capas 5-11 | [ADR-009](ADRs/ADR-009-H3-Indexing.md) | 🟡 En pruebas | $0 |
| **Geoespacial (AGEB)** | Polígonos INEGI para análisis territorial | [ADR-010](ADRs/ADR-010-AGEB-INEGI.md) | 🔴 Decidido, por implementar | $0 |
| **Geocodificación** | Google Maps Platform (Geocoding + Places API) | [ADR-011](ADRs/ADR-011-Google-Maps-Platform.md) | ✅ Activo | ~$0 (crédito $200) |
| **Data architecture** | Staging tables por portal + golden record `properties` | [ADR-012](ADRs/ADR-012-Multi-Portal-Data.md) | ✅ Decidido | $0 |
| **Frontend** | Next.js 15 + App Router + TypeScript + Tailwind + shadcn/ui | [ADR-015](ADRs/ADR-015-Frontend-Next-js.md) | 🟡 Phase 0 completo | $0 (Vercel free) |

---

## Decisiones Propuestas (Fase 2-3)

| Componente | Opciones | ADR | Cuándo decidir |
|-----------|---------|-----|---------------|
| **AI Routing** | OpenRouter (multi-modelo) | [ADR-013](ADRs/ADR-013-OpenRouter.md) | ✅ Activo (scraping pipeline) |
| **AI Memory** | Backboard.io (persistent memory + RAG) | [ADR-014](ADRs/ADR-014-Backboard.md) | Con módulo AI Brain |
| **Deduplicación** | Scoring + LLM-assisted (híbrido) | [ADR-016](ADRs/ADR-016-Deduplicacion.md) | Con ≥2 portales en staging |
| **Mapas GIS** | Atlas.co (API + embed) — $89/usuario | [ADR-017](ADRs/ADR-017-Plataforma-GIS.md) | ✅ Activo (2–3 usuarios, $178–267/mes) |
| **CI/CD** | GitHub Actions + Vercel | [ADR-018](ADRs/ADR-018-CI-CD.md) | Con frontend activo |

---

## Tecnologías Deprecadas / Descartadas

| Tecnología | Status | ADR / Razón |
|-----------|--------|-------------|
| **n8n Cloud** | ❌ Deprecado | [ADR-019](ADRs/ADR-019-n8n-Deprecado.md) — todo migrado a Trigger.dev |
| **Clay** | ⚠️ Transitorio, saliendo | Lógica se replica en Trigger.dev tasks |
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
| **Supabase** | Propiedades, listings, brokers, analytics, geo data | Apify, Trigger.dev (scrapers), Google APIs |
| **HubSpot** | Clientes, deals, comunicación comercial | Supabase (sync one-way vía Trigger.dev) |

**Sincronizaciones:**
- Supabase → HubSpot: propiedades y brokers (one-way, vía Trigger.dev)
- HubSpot → Supabase: deal status (one-way, minimal)
- Clay → HubSpot: enriquecimiento (transitorio, no toca Supabase)

---

## Decisiones Diferidas (no implementar aún)

| Tema | Cuándo implementar | Razón para diferir |
|------|-------------------|-------------------|
| Separar DB operativa / analítica | Cuando jobs compitan con búsquedas operativas | 60K listings + 4 usuarios no lo requieren |
| Supabase read replica | Cuando haya carga analítica real | Prematuro hoy |
| Schema `analytics` con materialized views | Cuando haya reportes complejos recurrentes | Prematuro hoy |
| Redis cache | Cuando el volumen lo justifique | No necesario hoy |

---

*Documento actualizado: 2026-03-02 | Costos actualizados con datos reales verificados. Ver [Total-Budget.md](../04-Validation/Total-Budget.md) para el desglose completo.*
