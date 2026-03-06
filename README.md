# BEIQA Real Estate Platform

> Plataforma tecnológica interna que centraliza la búsqueda de inmuebles comerciales/industriales, inteligencia de mercado y gestión de clientes para el equipo de representación de tenants corporativos de Beiqa.

**Fase actual**: 🟢 Desarrollo Activo — Sprint 1: Scrapers, Data & Mastra Agents
**Última actualización**: 2026-03-05

---

## ¿Qué es BEIQA Platform?

BEIQA Platform es una herramienta interna diseñada para **reducir el tiempo de búsqueda de propiedades en 50%** y **aumentar la calidad de las recomendaciones** mediante:

- **Scraping automatizado** de 4 portales inmobiliarios (Apify + Firecrawl + Trigger.dev)
- **Inteligencia de mercado** consolidada de múltiples fuentes
- **Análisis geoespacial** avanzado (PostGIS + H3 + AGEB)
- **Portal para clientes** con seguimiento de opciones

**Objetivo principal**: Diferenciar a Beiqa de brokers tradicionales mediante tecnología y datos superiores.

> **[Leer más sobre la visión y objetivos →](./00-Project/Vision-and-Goals.md)**

---

## Quick Links

### Para entender el proyecto
- [Visión y Objetivos](./00-Project/Vision-and-Goals.md) — ¿Qué queremos lograr?
- [Contexto de Negocio](./00-Project/Business-Context.md) — ¿Por qué existe este proyecto?
- [Resumen Ejecutivo](./05-Communication/Executive-Summary.md) — Overview para stakeholders

### Para el equipo técnico
- [Stack Decidido](./02-Architecture/Stack-Decidido.md) — Dashboard de decisiones con costos y links a ADRs
- [Arquitectura del Sistema](./02-Architecture/System-Architecture.md) — Diagrama del stack real (febrero 2026)
- [ADRs](./02-Architecture/ADRs/README.md) — 21 Architecture Decision Records documentados
- [Schema Real de la DB](./02-Architecture/Database/Schema-Real.md) — Tablas, triggers y RPCs implementados

### Para planificación
- [Mapa de Módulos](./01-Modules/) — Todos los módulos con dependencias y fases
- [Roadmap (Sprints)](./03-Roadmap/Roadmap.md) — Sprint 1-2 detallados, 4 milestones, backlog
- [Presupuesto Operativo](./04-Validation/Total-Budget.md) — Costos reales verificados ($747–896/mes)
- [Arquitectura de Agentes](./02-Architecture/Agent-Architecture.md) — 6 agentes AI diseñados con Mastra

---

## Estado del Proyecto

### Fase Actual: Desarrollo Activo — Fase 1

**Estado**: 🟢 En construcción | **Timeline**: ~4-5 semanas desde inicio

#### Lo que ya está funcionando
- ✅ Base de datos Supabase en producción (14 migrations, PostGIS, RLS)
- ✅ Apify actor para Inmuebles24 contratado y operando
- ✅ ~60,000 propiedades en el sistema
- ✅ Trigger.dev integrado para scrapers, automatizaciones, sync HubSpot y batch AI extraction
- ✅ Firecrawl ($99/mo) como motor de scraping para Pincali, CBRE, Colliers
- ✅ Browserbase ($20/mo) como cloud browser para scraping complejo
- ✅ Claude API para procesamiento de descripciones
- ✅ Rube + Claude Desktop como interfaz actual (MCP bridge a Supabase, HubSpot, Slack)
- ✅ Google Maps Platform (Geocoding + Places API)
- ✅ 21 ADRs documentados cubriendo todas las decisiones de arquitectura

#### Próximas prioridades
- Normalización multi-portal (staging tables por portal + golden record `properties`)
- H3 indexing geoespacial (en pruebas, Fabrizio)
- Deduplicación cross-portal
- Internal App (Next.js, Pamela) — Fase 2-3

> **[Ver Roadmap con sprints →](./03-Roadmap/Roadmap.md)**

---

## Módulos de la Plataforma

| Módulo | Descripción | Fase | Estado |
|--------|-------------|------|--------|
| [Scraper](./01-Modules/Scraper/) | Extracción automatizada de 4 portales (Apify, Firecrawl, Browserbase) | Fase 1 | 🟢 En desarrollo |
| [Internal App](./01-Modules/Internal-App/) | Aplicación web para el equipo Beiqa (Next.js) | Fase 3 | 🔴 Por iniciar |
| [Data](./01-Modules/Data/) | Normalización, deduplicación, integración de fuentes externas | Fase 1-2 | 🟡 Diseño/investigación |
| [Market Intelligence](./01-Modules/Market-Intelligence/) | Análisis y reportes de mercado automatizados | Fase 2 | 🔴 Por iniciar |
| [Geospatial](./01-Modules/Geospatial/) | Análisis geoespacial, H3 index, AGEB | Fase 2 | 🟡 Diseño/investigación |
| [Tenant Portal](./01-Modules/Tenant-Portal/) | Portal web para clientes (shortlists, feedback) | Fase 2 | 🔴 Por iniciar |
| [AI Brain](./01-Modules/AI-Brain/) | Matching inteligente, NLP, procesamiento de llamadas | Fase 3 | 🔴 Por iniciar |

> **[Ver mapa completo de módulos con dependencias →](./01-Modules/)**

---

## Stack Tecnológico

| Componente | Tecnología | Estado | ADR |
|-----------|-----------|--------|-----|
| Base de datos | Supabase (PostgreSQL + PostGIS + Auth + Storage + REST API) | ✅ Producción | [ADR-001](./02-Architecture/ADRs/ADR-001-Supabase-Plataforma.md) |
| Scraping (I24) | Apify (actor contratado) | ✅ Activo | [ADR-002](./02-Architecture/ADRs/ADR-002-Estrategia-Scraping.md) |
| Scraping (motor) | Firecrawl (HTTP engine, LLM extraction) | ✅ Activo | [ADR-007](./02-Architecture/ADRs/ADR-007-Firecrawl.md) |
| Scraping (browser) | Browserbase (cloud browser sessions) | ✅ Activo | [ADR-008](./02-Architecture/ADRs/ADR-008-Browserbase.md) |
| Automatización | Trigger.dev (scrapers, sync, cron — ejecución durable) | ✅ Activo | [ADR-003](./02-Architecture/ADRs/ADR-003-Trigger-dev.md) |
| AI Agent Orchestration | Mastra (agents, workflows, tools, memory, MCP) | 🟢 En implementación | [ADR-020](./02-Architecture/ADRs/ADR-020-Mastra.md) |
| AI/NLP | LLMs vía Mastra (modelo TBD por agente) | 🟡 TBD | [ADR-020](./02-Architecture/ADRs/ADR-020-Mastra.md) |
| UI actual | Rube + Claude Desktop (MCP bridge) | ✅ Transitorio | [ADR-004](./02-Architecture/ADRs/ADR-004-Rube-MCP-Bridge.md) |
| CRM | HubSpot | ✅ Activo | [ADR-005](./02-Architecture/ADRs/ADR-005-HubSpot-CRM.md) |
| Geocodificación | Google Maps Platform (Geocoding + Places) | ✅ Activo | [ADR-011](./02-Architecture/ADRs/ADR-011-Google-Maps-Platform.md) |
| Geoespacial | H3 indexing (h3-js, capas 5-11) | 🟡 En pruebas | [ADR-009](./02-Architecture/ADRs/ADR-009-H3-Indexing.md) |
| Frontend | Next.js | 🟡 Fase 2-3 | [ADR-015](./02-Architecture/ADRs/ADR-015-Frontend-Next-js.md) |

> **[Ver todas las decisiones técnicas →](./02-Architecture/Stack-Decidido.md)** | **[Ver los 21 ADRs →](./02-Architecture/ADRs/README.md)**

---

## Equipo

| Rol | Nombre |
|-----|--------|
| CEO / Product Owner | Pablo |
| Tech Lead | Fabrizio |
| Ops / Llamadas | Jerónimo |
| Frontend | Pamela |
| Advisor externo | Alex |

> **[Ver roles y responsabilidades completos →](./00-Project/Stakeholders.md)**

---

## Estructura de la Documentación

```
beiqa-real-estate-platform/
├── 00-Project/          # Visión, contexto, stakeholders, personas
├── 01-Modules/          # 7 módulos funcionales auto-contenidos
│   ├── Scraper/         # Extracción de datos (Apify + Firecrawl + Trigger.dev)
│   ├── Internal-App/    # App web del equipo (Next.js) — Fase 2-3
│   ├── Data/            # Normalización, dedup, fuentes externas
│   ├── Market-Intelligence/  # Análisis y tendencias
│   ├── Geospatial/      # Mapas, H3, AGEB
│   ├── AI-Brain/        # IA, matching, procesamiento de llamadas
│   └── Tenant-Portal/   # Portal para clientes
├── 02-Architecture/     # ADRs, stack decidido, arquitectura, DB schema
│   ├── ADRs/            # 21 Architecture Decision Records
│   ├── Database/        # Schema implementado
│   └── archive/         # Documentos legacy (referencia histórica)
├── 03-Roadmap/          # Sprints y milestones del proyecto
├── 04-Validation/       # Presupuesto y costos reales
└── 05-Communication/    # Materiales para stakeholders externos
```

---

## Convenciones de la Documentación

| Emoji | Significado |
|-------|-------------|
| 🟢 | Activo / En desarrollo |
| 🟡 | En diseño / Draft |
| 🔴 | Por iniciar |
| ✅ | Completado |

| Tag | Significado |
|-----|-------------|
| **MUST** | Imprescindible para que funcione |
| **SHOULD** | Importante pero no bloqueante |
| **COULD** | Deseable si hay tiempo/recursos |

---

## Changelog del Proyecto

| Fecha | Cambio |
|-------|--------|
| **2026-03-05** | **Integración Mastra y reestructuración Sprint-based** — Mastra adoptado como framework de AI agents (ADR-020, ADR-021). Agent-Architecture.md creado. Roadmap reescrito de fases a sprints de 2 semanas. 7 módulos actualizados. Archivos CONFLICT limpiados. |
| **2026-03-02** | **Presupuesto operativo reescrito con datos reales** — De $30–50/mes estimados a $747–896/mes verificados. Stack-Decidido.md con costos reales. |
| **2026-02-28** | **Workflow Boris Cherny implementado** — tasks/, auto-mejora, principios de trabajo, delegación y autonomía en CLAUDE.md. |
| **2026-02-27** | **Gran refactor de documentación** — 19 ADRs, Stack-Decidido dashboard, System-Architecture actualizado, documentos legacy archivados. |
| **2026-02-24** | **Reestructuración module-centric** — 6 carpetas, 7 módulos, estado real documentado. |

> **[Ver historial completo →](./CHANGELOG.md)**

---

*Última actualización: 2026-03-05*
