# Roadmap — Beiqa Platform

> **Actualizado**: 2026-03-05 | **Metodología**: Scrum (sprints de 2 semanas)
>
> Cada milestone tiene OKRs y KPIs. Cada sprint tiene deliverables y acceptance criteria.
> Los sprints son **cross-track**: scraper, agentes AI, y frontend avanzan juntos.
>
> Archivos anteriores: [archive/](archive/) (Fase-Real-1-Scrapers.md, Phase-1-MVP.md)

---

## Estado Actual (Marzo 2026)

| Componente | Estado | Detalle |
|-----------|--------|---------|
| Supabase | ✅ Producción | 14 migrations, PostGIS, RLS, ~60K propiedades |
| Scraper I24 (Apify) | ✅ Activo | Actor contratado, corridas bimensuales |
| Scrapers custom (Trigger.dev) | 🟡 En desarrollo | Pincali, CBRE, Colliers en desarrollo (Fabrizio) |
| Frontend (Next.js 15) | 🟡 Phase 0 | Estructura base, sin funcionalidad completa |
| AI (batch extraction) | 🟡 Parcial | OpenRouter + Trigger.dev, migrando a Mastra |
| GIS (H3) | 🟡 En pruebas | h3-js validado, sin deployment en producción |
| GIS (Atlas.co) | ✅ Activo | 2-3 usuarios |
| HubSpot sync | 🟡 En migración | De n8n a Trigger.dev |
| Mastra (AI Agents) | 🔴 Por implementar | ADRs aprobados, arquitectura diseñada |
| Golden record (`properties`) | 🔴 Por implementar | Schema diseñado, sin tablas creadas |

---

## Milestone 1: Datos Limpios

> **Timeline estimado**: Sprints 1-2 (4 semanas)
> **OKR**: Propiedades con direcciones correctas y datos normalizados en un golden record unificado

| KPI | Target |
|-----|--------|
| Address accuracy (muestra 100 propiedades) | >80% |
| Propiedades en golden record | >50% del inventario (30K+) |
| Enrichment pipeline funcional | End-to-end (staging → golden record) |

### Sprint 1: Infraestructura Mastra + Address Enrichment v1

**Objetivo**: Establecer la infraestructura de Mastra y validar el Address Enrichment Agent con un lote de prueba.

| Deliverable | Owner | Acceptance Criteria |
|------------|-------|--------------------|
| Repo `beiqa-agents` inicializado con Mastra | Pablo | `mastra dev` arranca sin errores. Estructura de agentes definida. |
| Supabase migrations (golden record tables) | Fabrizio | Tablas `properties`, `property_sources`, `agent_runs`, `enrichment_queue` creadas. RLS configurado. |
| Address Enrichment Agent v1 | Pablo | Procesa una propiedad: dirección corregida + confidence score. Google Geocoding integrado. |
| Coordinate Validator tool | Fabrizio | Valida coordenadas dentro de zona geográfica esperada (PostGIS). |
| Cache de API responses | Pablo | Tabla `cache_api_responses` con TTL 30 días para Google APIs. |
| Cost tracking | Pablo | Cada ejecución de agente registra costo estimado en `agent_runs`. |

**Exit criteria**: Agent procesa 1 propiedad → produce dirección corregida + confidence score. Migrations clean. Runs logged.

---

### Sprint 2: Backfill inicial + Data Normalization v1

**Objetivo**: Comenzar backfill de propiedades y establecer el pipeline de normalización staging → golden record.

| Deliverable | Owner | Acceptance Criteria |
|------------|-------|--------------------|
| Backfill workflow (batches de 500) | Pablo | Procesa 10,000 propiedades de inmuebles24_listings con enrichment. |
| Description address extractor (LLM tool) | Pablo | LLM extrae dirección/landmarks de descripción. Accuracy medida. |
| Data Normalization Agent v1 | Pablo | Mapea inmuebles24 → golden record. Campos: dirección, precio, superficie, tipo, operación. |
| Trigger.dev → Mastra HTTP integration | Fabrizio | POST de Trigger.dev a Mastra API funcional después de cada scrape. |
| Enrichment queue processing | Pablo | `enrichment_queue` se vacía progresivamente. Status tracking funcional. |

**Exit criteria**: 10K propiedades enriquecidas con confidence score. Normalization mapea staging → golden record. Trigger.dev triggers Mastra post-scrape.

---

## Milestone 2: Inteligencia Core

> **Timeline estimado**: Sprints 3-4 (4 semanas, después de Milestone 1)
> **OKR**: Deduplicación cross-portal, scoring migrado a Mastra, market intelligence básico

| KPI | Target |
|-----|--------|
| Backfill completado | 100% de propiedades enriquecidas (60K) |
| Deduplicación precision | >85% (verificación manual de 50 pares) |
| Scoring equivalente al frontend | Validación por Pablo |
| H3 + AGEB coverage | >90% de propiedades geocodificadas |

### Sprint 3: Backfill completo + Dedup v1

**Objetivo**: Completar el backfill de 60K propiedades y lanzar deduplicación cross-portal.

| Deliverable | Owner | Acceptance Criteria |
|------------|-------|--------------------|
| Backfill completo (60K propiedades) | Pablo | 100% de inmuebles24_listings procesadas. Confidence scores asignados. |
| H3 + AGEB tools en GIS Agent | Fabrizio | H3 res 5/7/9/11 calculados para propiedades geocodificadas. AGEB asignado via PostGIS. |
| AGEB shapefiles cargados en PostGIS | Fabrizio | Tabla de AGEBs funcional. Spatial join operativo. |
| Deduplication Agent v1 | Pablo | Detecta duplicados cross-portal con scoring + LLM para ambiguos. |

**Exit criteria**: 60K enriquecidas. >90% H3 coverage. >85% dedup precision (50 pares verificados).

---

### Sprint 4: Scoring migration + Market Intelligence v1

**Objetivo**: Migrar scoring del frontend a Mastra y generar los primeros reportes de market intelligence.

| Deliverable | Owner | Acceptance Criteria |
|------------|-------|--------------------|
| Scoring/Matching Agent | Pablo | Genera shortlists equivalentes al frontend actual. API endpoint funcional. |
| Market Intelligence Agent v1 | Pablo | Genera reportes de precio/m² por zona (H3 res 7). |
| GIS Agent v1 | Fabrizio | Zone quality scores para zonas principales. |
| Frontend calls Mastra API para scoring | Fabrizio | `beiqa-frontend` consume scoring API de Mastra en lugar de lógica local. |
| scoring_reports + scoring_results tables | Fabrizio | Tablas creadas, RLS configurado, datos de scoring persistidos. |

**Exit criteria**: Scoring quality validado por Pablo. 5+ zone reports generados. Frontend integrado con Mastra scoring API.

---

## Milestone 3: Integración y Producción

> **Timeline estimado**: Sprints 5-6 (4 semanas, después de Milestone 2)
> **OKR**: Pipeline end-to-end funcional, frontend consume golden record, sistema monitoreado

| KPI | Target |
|-----|--------|
| Latencia enrichment (nuevas propiedades) | <60 minutos de scrape a golden record |
| Scoring response time | <30 segundos |
| Costo mensual total | Dentro de presupuesto |
| Uptime del pipeline | >95% |

### Sprint 5: Pipeline end-to-end + Frontend integration

| Deliverable | Owner | Acceptance Criteria |
|------------|-------|--------------------|
| Pipeline completo (scrape → enrich → normalize → dedup → golden record) | Pablo + Fabrizio | Propiedad nueva aparece en golden record <60 min después de scrape. |
| Orchestrator workflow | Pablo | Coordina Address Enrichment → Normalization → Dedup → GIS en secuencia. |
| Frontend lee golden record | Fabrizio | Internal App muestra propiedades del golden record (no staging). |
| Slack alerts para agentes | Pablo | Errores de agentes → Slack. Resumen diario de procesamiento. |
| Cost monitoring dashboard | Pablo | Costos por agente visibles. Alertas al 80% del budget. |

---

### Sprint 6: Polish, evals, documentación

| Deliverable | Owner | Acceptance Criteria |
|------------|-------|--------------------|
| Evaluation scorers para todos los agentes | Pablo | Cada agente tiene eval automatizado (ver [Agent-Architecture.md](../02-Architecture/Agent-Architecture.md)). |
| Error handling robusto | Pablo + Fabrizio | Agentes manejan edge cases sin crashes. Dead letter queue para errores persistentes. |
| Performance optimization | Fabrizio | Batch processing optimizado. Queries eficientes en golden record. |
| Documentación actualizada | Pablo | ADRs, módulos, y roadmap reflejan el estado real. |
| Operations runbook para Jerónimo | Pablo | Guía de operación: cómo monitorear, qué hacer si falla, cómo re-ejecutar. |

**Exit criteria**: Quality metrics met (address >80%, dedup >85%, scoring >75%). Sistema procesa 10K nuevas/semana. Docs actualizados. Runbook aprobado por Jerónimo.

---

## Más allá de Milestone 3

| Área | Descripción | Cuándo |
|------|------------|--------|
| Tenant Portal con scoring | Clientes ven y dan feedback a shortlists | Post-Milestone 2 |
| Multi-modelo optimization | Evaluar y optimizar modelo por agente/tarea | Continuo |
| ArcGIS MCP integration | GIS Agent consume ArcGIS para análisis avanzado | Post-Milestone 3 |
| Nuevos portales | Cushman & Wakefield, JLL | Cuando scrapers estén estables |
| AI Memory (RAG) | Mastra memory para contexto acumulado de clientes | Post-Milestone 3 |

---

## Relación con GitHub Issues

| Issue | Milestone | Notas |
|-------|-----------|-------|
| #40 Mejorar roadmap de versiones | — | Resuelto por este documento |
| #44 Actualizar Arquitectura/ADRs/Stack-Decidido.md | — | Resuelto (actualizado 2026-03-05) |
| #35 Desarrollar módulo AI para extracción | M1 | Redefinido bajo Mastra (Address Enrichment + Normalization) |
| #12-#30 Issues de scraper/Trigger.dev | M1-M2 | Siguen activos, ejecutados por Fabrizio |
| #47-#78 Issues de Tenant Portal (TP-*) | M2-M3 | Avanzan en paralelo con agentes |

---

*Documento creado: 2026-03-05 | Reemplaza: [archive/Fase-Real-1-Scrapers.md](archive/Fase-Real-1-Scrapers.md), [archive/Phase-1-MVP.md](archive/Phase-1-MVP.md)*
