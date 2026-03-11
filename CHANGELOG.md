# Changelog

> Registro cronológico de cambios significativos en la documentación y planeación de Beiqa Platform.
> Para el backlog de desarrollo: [GitHub Issues](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues)

---

## 2026-03-11 — Reestructuración del Roadmap a jerarquía Q/Sprint

- **Roadmap reestructurado** — De archivo monolítico (~16KB) a jerarquía Q > Sprint con carpetas `Q1-2026/`, `Q2-2026/`, `Q3-2026/` y templates reutilizables. `Roadmap.md` reescrito como índice (~80 líneas). Original archivado en `archive/Roadmap-Monolitico-v1.md`.
- **13 milestones outcome-based** — Reemplazan M1-M4 genéricos: Scrapers Consolidados, Enrichment Agent Operativo, Portal Autenticado, Design System Implementado, Golden Record Pipeline, Scoring Automatizado, Portal con Shortlists, Inteligencia Geoespacial, Búsqueda Inteligente, Inteligencia de Mercado, Portal en Producción, Dashboard Interno, Operación Estable.
- **OKRs definidos por Quarter** — Q1 (4 objetivos, 11 KRs) y Q2 (6 objetivos, 26 KRs) con capacidad del equipo y dependencias externas documentadas.
- **Sprint 1-3 con detalle completo** — OKRs, DoD, deliverables por track (AI/Mastra, Scraper/Infra, Frontend), riesgos y dependencias. Sprint 4-8 con alto nivel.
- **Backlog.md creado** — Issues agrupados por módulo (Scraper, Data, AI Brain, Geospatial, Frontend, Infra). GitHub Issues como source of truth.
- **GitHub sincronizado** — 13 milestones creados, issues reasignados de M1-M4 a nuevos milestones, M1-M4 cerrados. 8 issues nuevos creados (#125-#132): I24 migration tracking, data quality monitoring, multi-tenant RLS, API rate limiting, tenant onboarding UX, cost estimation AI, budget recalculation, design system core.
- **Propagación cross-file** — Executive-Summary.md, README.md, Modules/README.md actualizados con 13 milestones y nueva estructura.

## 2026-03-09 — Pincali scraper pipeline completo

- **Pincali scraper completado end-to-end** — Nuevos archivos: `download-files.ts` (descarga imágenes de EasyBroker CDN → Supabase Storage), `save-to-supabase.ts` (upsert a `pincali_listings`, `price_history`, `touchLastSeen`, `updateStorageUrls`). Pipeline completo: discover → scrape → save → download images → update storage URLs.
- **Optimización de créditos Firecrawl** — Skip re-scraping de propiedades existentes (solo actualiza `last_seen_at`). Batch processing con idempotency keys (`pincali-{id}-{date}`).
- **Pipeline de datos corregido en Scraper/README.md** — Flujo genérico ya no menciona HubSpot (era legacy de I24/Apify). Documentado flujo real por portal con tabla de destinos (Supabase vs HubSpot vs Storage).
- **discover-urls y scheduled-scrape actualizados** — `discover-urls` genera `discovered-urls.json`; `scheduled-scrape` lee el archivo, separa nuevas vs existentes, y orquesta el pipeline completo con batches de 10.

## 2026-03-08 — Actualización de scrapers, protocolo de cierre de sesión

- **Scrapers CBRE, Colliers y FinSA en producción** — Schema-Real.md actualizado con tablas `cbre_listings`, `colliers_listings`, `finsa_listings`, `price_history`, 5 Storage buckets, trigger `price_history_on_update` y RPC `increment_scrapes_sin_aparecer_finsa`.
- **Stack-Decidido actualizado** — CBRE/Colliers/FinSA marcados como producción. I24 en migración de Apify+Clay a Trigger.dev+Firecrawl (Sprint 1-2). Clay marcado como migrando.
- **Roadmap reorganizado** — Pincali persist e I24 migration pruebas movidos a Sprint 1. I24 migración completa movida a Sprint 2. Sprint 7-8 I24 tachado como adelantado. Sprint 1 Review actualizado.
- **Protocolo automático de cierre de sesión creado** — `tasks/session-protocol.md` con 9 pasos: resumen, commits, propagación, lecciones, issues, presupuesto, MEMORY.md, CHANGELOG/README, próximos pasos. Se ejecuta automáticamente vía MEMORY.md (auto-cargado por Claude Code).
- **MEMORY.md del proyecto creado** — Trigger automático + contexto de sesión persistente entre conversaciones. Incluye mapa de 5 repos Beiqa.

## 2026-03-05 — Integración Mastra y reestructuración Sprint-based

- **Hosting Strategy decidido (ADR-022)** — Vercel Pro para frontend ($20/mo), Mastra Cloud (beta, gratis) para agentes AI. DigitalOcean y Railway descartados. Stack-Decidido, Total-Budget, System-Architecture y ADR-015 actualizados.
- **Mastra adoptado como framework de AI agents** — ADR-020 y ADR-021 aprobados. Mastra maneja agentes, workflows, tools, memory y MCP. Reemplaza uso directo de Claude API para orquestación.
- **Agent-Architecture.md creado** — Diseño completo de 6 agentes AI: Address Enrichment, Data Normalization, Deduplication, Scoring/Matching, Market Intelligence, GIS Analysis.
- **System-Architecture.md reescrito** — Diagrama actualizado con Mastra como capa transversal de AI entre staging tables y golden record.
- **Trigger.dev scope reducido formalmente** — ADR-003 actualizado: Trigger.dev solo hace ejecución durable (scrapers, cron, sync). Toda lógica AI migra a Mastra.
- **Backboard.io deprecado** — ADR-014 actualizado a "supersedido". Mastra memory cubre persistent context, RAG y shared memory entre agentes.
- **Roadmap reescrito de fases a sprints** — Metodología Scrum con sprints de 2 semanas. 4 milestones (M1-M4, Mar 29 – Jun 21). Sprint 1 y 2 detallados con OKRs y acceptance criteria.
- **Archivos de roadmap viejos archivados** — `Fase-Real-1-Scrapers.md` y `Phase-1-MVP.md` movidos a `03-Roadmap/archive/`.
- **7 READMEs de módulos actualizados** — Scraper, Data, AI Brain, Geospatial, Tenant Portal, Internal App y Market Intelligence reflejan Mastra como capa transversal.
- **Total-Budget.md actualizado** — Sección 8 agregada con costos de Mastra LLM (~$67–88/mo estimado). Costo total con Mastra: ~$814–984/mo.
- **CLAUDE.md actualizado** — 21 ADRs (antes 19), Mastra en stack table, OpenRouter marcado como legacy, roles de equipo actualizados.
- **Archivos CONFLICT de sync tool limpiados** — 10 archivos `*-CONFLICT-1` eliminados (generados por herramienta de sincronización, no conflictos de git).

## 2026-03-02 — Presupuesto real y Tenant Portal

- **Presupuesto operativo reescrito con datos reales** — Total-Budget.md reemplazado completamente: de $30–50/mes estimados a $747–896/mes verificados. Incluye costos fijos vs variables, costo por portal, costo por propiedad (nueva $0.003–0.009 vs update $0.002–0.006), proyecciones 6–12 meses ($775–1,200/mes), puntos de inflexión y exclusiones.
- **Stack-Decidido.md actualizado con costos reales** — Apify $300, Trigger.dev $50, Atlas.co $178–267, OpenRouter $15–30, Rube+Claude $75–100.
- **Changelog y quick links actualizados en README** — Refleja presupuesto real.

## 2026-02-28 — Workflow Boris Cherny implementado

- **Nuevas secciones en CLAUDE.md** — Workflow de tareas (modo plan + sync GitHub Issues multi-repo), Principios de trabajo (simplicidad, estándar senior, solo lo necesario), Delegación y autonomía (tabla de autonomía + subagentes). Regla #6 de auto-mejora.
- **tasks/ creado** — `lessons.md` (loop de auto-mejora) y `todo.md` (scratchpad de sesión sincronizado con GitHub Issues).

## 2026-02-27 — Gran refactor de documentación y ADRs

- **19 ADRs documentados** — Documentación completa de todas las decisiones de arquitectura en formato MADR 4.0: 12 aceptadas (Supabase, scraping multi-portal, Trigger.dev, Firecrawl, Browserbase, Rube, HubSpot, monitoreo, H3, AGEB, Google Maps, multi-portal data), 6 propuestas (OpenRouter, Backboard, frontend Next.js, deduplicación, GIS, CI/CD) y 1 deprecada (n8n).
- **Templates MADR 4.0 y Simple** — Dos templates adaptados al proyecto en español con campo de costo mensual, plan de implementación y checklist de verificación.
- **Stack-Decidido.md convertido a dashboard** — Tabla de decisiones con costos y links directos a cada ADR. n8n eliminado como activo, Firecrawl ($99/mo), Browserbase ($20/mo), Rube como UI actual.
- **System-Architecture.md actualizado** — Diagrama mermaid con stack real: 4 portales (I24/Pincali/CBRE/Colliers), Trigger.dev como orquestador, Rube como interfaz.
- **CLAUDE.md actualizado** — Stack real: Trigger.dev reemplaza n8n, Firecrawl/Browserbase/Rube/H3/Google Maps agregados, Internal App movida a Fase 2-3.
- **Documentos legacy archivados** — Tech-Stack-Decision.md, Integraciones.md y Deduplication-Strategy.md movidos a `archive/`.
- **README.md actualizado** — Stack, módulos, quick links y estructura reflejan el estado real.
- **.gitignore agregado** — Excluye 30+ carpetas de herramientas AI y config local.
- **Fases de módulos actualizadas** — Internal App a Fase 3, Geospatial y Data a Diseño/investigación.

## 2026-02-26 — CLAUDE.md inicial

- **CLAUDE.md creado** — Instrucciones para Claude Code: rol, convenciones, reglas, archivos de referencia.

## 2026-02-24 — Reestructuración module-centric

- **Estado real documentado** — Stack decidido, schema real, fases actuales, equipo.
- **Modelo module-centric por Pablo** — 6 carpetas (00-05), 7 módulos auto-contenidos.

## 2026-02-06 — Expansión de cuestionarios

- **463 preguntas de producto** — Cuestionarios expandidos en 9 módulos.

## 2026-02-05 — Reestructuración README

- **README reestructurado** — Formato actualizado.

## 2026-02-04 — Inicio del proyecto

- **Estructura inicial creada** — Documentación base del proyecto.

---

*Última actualización: 2026-03-09*
