# Roadmap — Beiqa Platform

> **Actualizado**: 2026-03-05 | **Metodología**: Scrum (sprints de 2 semanas)
>
> Sprints **cross-cutting**: Scraper, AI/Mastra y Frontend avanzan en paralelo cada sprint.
> Solo Sprint 1-2 se planean en detalle. Sprint 3+ = backlog priorizado.
>
> **Backlog centralizado**: [GitHub Issues](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues)
> Archivos anteriores: [archive/](archive/)

---

## Estado Actual (Marzo 2026)

| Componente | Estado | Detalle |
|-----------|--------|---------|
| Supabase | ✅ Producción | PostgreSQL 17, 32 migrations, PostGIS, RLS, ~25K propiedades I24 + ~3K otros portales |
| Scraper I24 (Apify) | ⚠️ Migrando | Apify+Clay → Trigger.dev+Firecrawl (Sprint 1: pruebas, Sprint 2: migración completa) |
| Scraper FinSA | ✅ Producción | beiqa-scraper, Supabase + PDFs + H3 + validación + Slack |
| Scraper CBRE | ✅ Producción | Persistencia a Supabase + Storage, cron martes 6am UTC |
| Scraper Colliers | ✅ Producción | Persistencia a Supabase + Storage, cron lunes 6am UTC |
| Scraper Pincali | ✅ Producción | Persistencia a `pincali_listings` (1,761 props), Storage, H3, cron lunes 7am |
| Frontend (Next.js 15) | 🟡 Phase 0 | Scoring dashboard funcional, scorings en filesystem |
| Mastra (AI Agents) | 🔴 Por implementar | ADRs aprobados (020, 021), arquitectura diseñada, repo NO existe |
| Golden record | 🔴 Por implementar | Schema diseñado, sin tablas creadas |
| GIS (H3) | 🟡 En pruebas | h3-js validado, sin deployment |
| GIS (Atlas.co) | ✅ Activo | 2-3 usuarios |
| HubSpot sync | 🟡 En migración | De n8n a Trigger.dev |

### Landscape de portales

| Portal | Estado | PDFs | Prioridad |
|--------|--------|------|-----------|
| Inmuebles24 | ⚠️ Migrando (Apify → Trigger.dev) | No | Sprint 1-2 |
| FinSA | ✅ Producción (Trigger.dev) | Sí (flyers) | — |
| CBRE | ✅ Producción (Trigger.dev) | Sí (imágenes + PDFs) | — |
| Colliers | ✅ Producción (Trigger.dev) | Sí (imágenes + PDFs) | — |
| Pincali | ✅ Producción (Trigger.dev) | Sí (imágenes) | — |
| Cushman | Planeado | TBD | Sprint 3+ |
| Proximity Parks | Planeado | TBD | Sprint 3+ |
| PDFs developers | Manual → GDrive → Agent → Supabase | Sí | Sprint 3+ |

### Capacidad del equipo

| Persona | Rol | Split Sprint 1 | Split Sprint 2 |
|---------|-----|-----------------|-----------------|
| **Pablo** | CEO / PO + Dev | 60% AI/Mastra, 20% Frontend, 20% Planning | 55% AI/Mastra, 25% Frontend, 20% Planning |
| **Fabrizio** | Tech Lead | 70% Scraper/Infra, 30% DB/Migrations | 60% Scraper/Infra, 40% DB/Migrations |
| **Pamela** | Design | Figma designs (shortlist, feedback, dashboard) | Figma designs |

---

## Milestones

| # | Nombre | Sprints | Due Date | Valor de negocio |
|---|--------|---------|----------|------------------|
| M1 | **Datos Confiables** | 1-2 | Mar 29 | Scrapers en Supabase. Golden record structure. Primer agente AI. Auth en portal. |
| M2 | **Búsqueda Inteligente** | 3-4 | Abr 26 | Scoring AI. Dedup cross-portal. Shortlists para clientes. |
| M3 | **Experiencia del Cliente** | 5-6 | May 24 | Portal live. Feedback de clientes. Pipeline E2E automatizado. |
| M4 | **Operación Estable** | 7-8 | Jun 21 | Sistema estable. CI/CD. Evals. Documentación operativa. |

---

## Sprint 1 (Mar 2-15) — ACTIVO

### OKRs

| Objetivo | Key Result |
|----------|------------|
| O1: Infraestructura AI agents | KR1: `beiqa-agents` repo con `mastra dev` funcionando |
| O1 | KR2: Address Enrichment Agent procesa 1 propiedad I24 E2E |
| O2: Scrapers persisten en Supabase | KR3: Staging table CBRE + scraper escribe datos |
| O2 | KR4: Golden record tables creadas (structure) |
| O3: Auth funcional en portal | KR5: Login magic link funcional |

### Definition of Done
- Todos los KRs verificados con evidencia (screenshot, query, log)
- Issues cerrados o con justificación
- Sin bugs bloqueantes
- Código commiteado y pusheado

### Deliverables por track

#### Track: AI / Mastra (Pablo)

| Deliverable | Issue | AC |
|------------|-------|----|
| Inicializar repo beiqa-agents | [#86](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/86) | `mastra dev` arranca. Estructura agents/tools/workflows. README + .env.example. |
| Address Enrichment Agent v1 | [#87](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/87) | 1 prop I24 → dirección corregida + confidence (0-100). Google Geocoding como tool. |
| Evaluación modelos LLM | [#88](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/88) | ≥2 modelos en 10 props. Tabla costo/calidad/velocidad. Decisión documentada. |
| Coordinate Validator tool | [#89](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/89) | Valida coords en CDMX/EdoMex/Morelos/Puebla. PostGIS. |
| Cost tracking | [#90](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/90) | Cada run registra modelo, tokens, costo, duración en agent_runs. |

#### Track: Scraper / Infra (Fabrizio)

| Deliverable | Issue | AC |
|------------|-------|----|
| Golden record migrations (5 tablas) | [#104](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/104) | properties, property_sources, agent_runs, enrichment_queue, cache_api_responses. RLS. |
| Campos comunes golden record | [#105](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/105) | Schema unificado para 8+ portales. Confidence + needs_review. |
| Staging table CBRE | [#108](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/108) | cbre_listings creada. Schema alineado a CbreProperty. |
| Módulo Supabase write | [#13](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/13) | Upsert a staging tables. Testeada con CBRE. ON CONFLICT. |
| CBRE: persist + test | [#21](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/21) + [#24](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/24) | Datos en cbre_listings. ≥80% campos. Cron Tue 6am. Slack. |
| Módulo Slack | [#18](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/18) | Notificación success/error. Portal, #props, duración, errores. |
| Módulo imágenes → Storage | [#19](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/19) | CBRE images en Supabase Storage. URL en staging. |
| Detección anomalías | [#20](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/20) | Logs: campos vacíos, coords fuera de rango, precios inválidos. |
| ~~Pincali: persist a Supabase~~ | — | ✅ Completado — 1,761 props en `pincali_listings`. |
| I24: pruebas de migración a Trigger.dev+Firecrawl | — | Viabilidad técnica y económica validada. Reemplazar Apify+Clay. |

#### Track: Frontend / TP (Pablo)

| Deliverable | Issue | AC |
|------------|-------|----|
| Supabase Auth | [#47](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/47) | @supabase/ssr configurado. Redirect URLs. Smoke test. |
| Login (magic link + password) | [#50](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/50) | Card + logo. Magic link + password fallback. Responsive. |
| Middleware protección rutas | [#51](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/51) | middleware.ts valida getUser(). Session refresh. Redirect /login. |

### Sprint 1 Review (Mar 15)
**Demo**: Address Enrichment procesando 1 propiedad E2E. CBRE datos en Supabase con cron. ~~Pincali persist~~ ✅ completado. I24 viabilidad de migración. Login magic link. Golden record tables.

---

## Sprint 2 (Mar 16-29)

### OKRs

| Objetivo | Key Result |
|----------|------------|
| O1: Backfill masivo | KR1: ≥10K propiedades I24 enriquecidas |
| O2: Pipeline staging→golden record | KR2: Data Normalization mapea I24→properties |
| O3: ≥2 scrapers en Supabase | KR3: Colliers y/o Pincali datos en staging |
| O4: Scoring de Supabase | KR4: Scoring pages refactorizadas, DB |

### Definition of Done
- Backfill ≥10K con progreso trackeable
- ≥2 scrapers con datos en Supabase
- Scoring lee de Supabase (no filesystem)
- Vercel deploy funcional

### Deliverables por track

#### Track: AI / Mastra (Pablo)

| Deliverable | Issue | AC |
|------------|-------|----|
| Backfill workflow (batches 500) | [#91](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/91) | ≥10K I24 procesadas. Rate limits. Progreso en enrichment_queue. |
| Description address extractor | [#92](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/92) | LLM extrae dirección de descripción. Accuracy en 50 props. |
| Data Normalization Agent v1 | [#93](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/93) | I24→golden record. ≥95% campos mapeados. |
| Enrichment queue processing | [#95](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/95) | pending→processing→done/error. Errores no bloquean. |
| Trigger.dev→Mastra HTTP | [#94](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/94) | POST funcional con payload real. |

#### Track: Scraper / Infra (Fabrizio)

| Deliverable | Issue | AC |
|------------|-------|----|
| Staging table Colliers | [#109](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/109) | colliers_listings. Schema alineado. RLS. |
| Staging table Pincali | [#110](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/110) | pincali_listings. Schema alineado. RLS. |
| Colliers: persist + test | [#22](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/22) + [#25](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/25) | Datos en staging. Cron Mon 6am. PDFs en Storage. |
| Check de existencia | [#15](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/15) | Update si existe. Marcar unavailable si desaparece. Validación CBRE/Colliers. |
| Columnas enrichment en I24 | [#106](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/106) | enrichment_status, confidence, address_corrected, golden_record_id. |
| I24: migración completa a Trigger.dev | [#27](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/27), [#29](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/29), [#30](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/30) | Config + push a Supabase + gestión de lógica. Eliminar Apify+Clay. |

#### Track: Frontend / TP (Pablo)

| Deliverable | Issue | AC |
|------------|-------|----|
| Tabla scorings + RLS | [#48](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/48) | scorings: tenant_id, scoring_data JSONB, status. RLS. |
| Vincular auth con tenant | [#52](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/52) | auth_user_id en tenants. RLS via auth.uid(). |
| Refactorizar scoring pages | [#56](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/56) | Dashboard/lista/detalle leen de Supabase. |
| Deploy Vercel | [#55](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/55) | beiqa-frontend en Vercel. Env vars. Auto-deploy main. |
| shadcn/ui components | [#49](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/49) | ~13 componentes instalados. |

### Sprint 2 Review (Mar 29)
**Demo**: Backfill ≥10K. Normalization I24→golden record. I24 migrado a Trigger.dev (sin Apify/Clay). Validación CBRE/Colliers. Scoring de DB. Vercel deploy.

---

## Backlog Sprint 3+ (priorizado, no detallado)

### Sprint 3 (Mar 30 - Abr 12): Backfill Completo + Dedup + Shortlists
- Completar backfill I24 (~25K) — backlog MA
- Dedup Agent v1 ([#96](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/96))
- H3 indexer + AGEB ([#113](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/113), [#114](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/114))
- Normalization schemas custom portales ([#101](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/101))
- Shortlist tables + UI ([#57](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/57)-[#60](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/60))
- Human-in-the-loop review ([#107](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/107))
- Cushman scraper ([#36](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/36))
- Proximity Parks ([#111](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/111))

### Sprint 4 (Abr 13-26): Scoring AI + Market Intel
- Scoring/Matching Agent ([#97](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/97))
- Market Intelligence Agent ([#98](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/98))
- GIS Agent + ArcGIS MCP ([#115](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/115), [#116](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/116))
- Frontend consume Mastra scoring ([#117](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/117))
- Shortlist dashboard + mapa ([#62](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/62), [#64](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/64), [#67](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/67), [#69](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/69))

### Sprint 5-6: Pipeline E2E + Portal Live
- Orchestrator workflow ([#99](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/99))
- Slack alerts agentes ([#100](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/100))
- Error handling + DLQ ([#102](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/102))
- Internal App lee golden record ([#118](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/118))
- Notifications + approval + comparación ([#61](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/61), [#63](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/63), [#65](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/65), [#66](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/66))
- Market intel en portal ([#68](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/68), [#70](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/70), [#71](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/71))
- PDF developers pipeline ([#112](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/112))

### Sprint 7-8: Evals + Operaciones + Launch
- Evals todos agentes ([#103](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/103))
- CI/CD ([#119](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/119), [#75](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/75))
- Performance monitoring ([#120](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/120))
- ~~I24 migración Clay→TriggerDev~~ → **Adelantado a Sprint 1-2**
- Testing mobile + QA ([#72](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/72)-[#74](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/74))
- HubSpot sync, PDF export, docs ([#76](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/76)-[#78](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/78))
- Runbook + onboarding ([#121](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/121), [#122](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/122))

---

*Documento actualizado: 2026-03-09 | Pincali persist completado. PG 17, 32 migrations. I24 migración adelantada Sprint 1-2. 5 scrapers en producción. Reemplaza: [archive/Fase-Real-1-Scrapers.md](archive/Fase-Real-1-Scrapers.md), [archive/Phase-1-MVP.md](archive/Phase-1-MVP.md)*
