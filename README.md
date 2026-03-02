# BEIQA Real Estate Platform

> Plataforma tecnológica interna que centraliza la búsqueda de inmuebles comerciales/industriales, inteligencia de mercado y gestión de clientes para el equipo de representación de tenants corporativos de Beiqa.

**Fase actual**: 🟢 Desarrollo Activo — Fase 1: Scrapers & Inventario
**Última actualización**: 2026-03-02

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
- [ADRs](./02-Architecture/ADRs/README.md) — 19 Architecture Decision Records documentados
- [Schema Real de la DB](./02-Architecture/Database/Schema-Real.md) — Tablas, triggers y RPCs implementados

### Para planificación
- [Mapa de Módulos](./01-Modules/) — Todos los módulos con dependencias y fases
- [Fase 1: Scrapers & Inventario](./03-Roadmap/Fase-Real-1-Scrapers.md) — En curso ahora
- [Roadmap completo](./03-Roadmap/Phase-1-MVP.md) — Las 3 fases del proyecto
- [Presupuesto Operativo](./04-Validation/Total-Budget.md) — Costos reales verificados ($747–896/mes)

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
- ✅ 19 ADRs documentados cubriendo todas las decisiones de arquitectura

#### Próximas prioridades
- Normalización multi-portal (staging tables por portal + golden record `properties`)
- H3 indexing geoespacial (en pruebas, Fabrizio)
- Deduplicación cross-portal
- Internal App (Next.js, Pamela) — Fase 2-3

> **[Ver detalles de Fase 1 →](./03-Roadmap/Fase-Real-1-Scrapers.md)**

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
| Automatización | Trigger.dev (scrapers, sync, limpieza, cron, AI batch) | ✅ Activo | [ADR-003](./02-Architecture/ADRs/ADR-003-Trigger-dev.md) |
| AI/NLP | Claude API | ✅ Activo | — |
| UI actual | Rube + Claude Desktop (MCP bridge) | ✅ Transitorio | [ADR-004](./02-Architecture/ADRs/ADR-004-Rube-MCP-Bridge.md) |
| CRM | HubSpot | ✅ Activo | [ADR-005](./02-Architecture/ADRs/ADR-005-HubSpot-CRM.md) |
| Geocodificación | Google Maps Platform (Geocoding + Places) | ✅ Activo | [ADR-011](./02-Architecture/ADRs/ADR-011-Google-Maps-Platform.md) |
| Geoespacial | H3 indexing (h3-js, capas 5-11) | 🟡 En pruebas | [ADR-009](./02-Architecture/ADRs/ADR-009-H3-Indexing.md) |
| Frontend | Next.js | 🟡 Fase 2-3 | [ADR-015](./02-Architecture/ADRs/ADR-015-Frontend-Next-js.md) |

> **[Ver todas las decisiones técnicas →](./02-Architecture/Stack-Decidido.md)** | **[Ver los 19 ADRs →](./02-Architecture/ADRs/README.md)**

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
│   ├── ADRs/            # 19 Architecture Decision Records
│   ├── Database/        # Schema implementado
│   └── archive/         # Documentos legacy (referencia histórica)
├── 03-Roadmap/          # Fases del proyecto
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
| **2026-03-02** | **Presupuesto operativo reescrito con datos reales** — Total-Budget.md reemplazado completamente: de $30–50/mes estimados a $747–896/mes verificados. Incluye costos fijos vs variables, costo por portal, costo por propiedad (nueva $0.003–0.009 vs update $0.002–0.006), proyecciones 6–12 meses ($775–1,200/mes), puntos de inflexión y exclusiones. Stack-Decidido.md actualizado con costos reales: Apify $300, Trigger.dev $50, Atlas.co $178–267, OpenRouter $15–30, Rube+Claude $75–100 |
| **2026-02-28** | **Workflow Boris Cherny implementado** — Nuevas secciones en CLAUDE.md: Workflow de tareas (modo plan + sync GitHub Issues multi-repo), Principios de trabajo (simplicidad, estándar senior, solo lo necesario), Delegación y autonomía (tabla de autonomía + subagentes). Regla #6 de auto-mejora. Creados `tasks/lessons.md` (loop de auto-mejora) y `tasks/todo.md` (scratchpad de sesión sincronizado con GitHub Issues) |
| **2026-02-27** | **Actualizar fases de módulos** — Internal App movida a Fase 3 (🔴 Por iniciar), Geospatial y Data cambiados a 🟡 Diseño/investigación |
| 2026-02-27 | **19 ADRs documentados** — Documentación completa de todas las decisiones de arquitectura del proyecto en formato MADR 4.0: 12 aceptadas (Supabase, scraping multi-portal, Trigger.dev, Firecrawl, Browserbase, Rube, HubSpot, monitoreo, H3, AGEB, Google Maps, multi-portal data), 6 propuestas (OpenRouter, Backboard, frontend Next.js, deduplicación, GIS, CI/CD) y 1 deprecada (n8n) |
| 2026-02-27 | **Templates MADR 4.0 y Simple** — Dos templates adaptados al proyecto en español con campo de costo mensual, plan de implementación y checklist de verificación. Índice maestro de ADRs con estado, costo y decisores |
| 2026-02-27 | **Stack-Decidido.md convertido a dashboard** — Tabla de decisiones con costos y links directos a cada ADR. Eliminado n8n como activo, agregados Firecrawl ($99/mo), Browserbase ($20/mo), Rube como UI actual |
| 2026-02-27 | **System-Architecture.md actualizado** — Diagrama mermaid con stack real: 4 portales de scraping (I24/Pincali/CBRE/Colliers), Trigger.dev como orquestador central, Rube como interfaz, flujos actualizados. Eliminados n8n y EasyBroker |
| 2026-02-27 | **CLAUDE.md actualizado** — Stack real para AI assistants: Trigger.dev reemplaza n8n, Firecrawl/Browserbase/Rube/H3/Google Maps agregados, Internal App movida a Fase 2-3, n8n a descartadas |
| 2026-02-27 | **Documentos legacy archivados** — Tech-Stack-Decision.md, Integraciones.md y Deduplication-Strategy.md movidos a archive/ con README explicando qué reemplazó cada uno |
| 2026-02-27 | **README.md actualizado** — Stack, módulos, quick links y estructura reflejan el estado real del proyecto (sin n8n, con Firecrawl/Browserbase/Rube, Internal App en Fase 2-3) |
| 2026-02-27 | **.gitignore agregado** — Excluye 30+ carpetas de herramientas AI y config local que no son parte del proyecto |
| 2026-02-26 | Agregado CLAUDE.md con instrucciones para Claude Code (rol, convenciones, reglas, archivos de referencia) |
| 2026-02-24 | Actualización completa para reflejar estado real: stack decidido, schema real, fases actuales, equipo |
| 2026-02-24 | Reestructuración a modelo module-centric por Pablo: 6 carpetas, 7 módulos auto-contenidos |
| 2026-02-06 | Expansión de cuestionarios de producto: 463 preguntas en 9 módulos |
| 2026-02-05 | Reestructuración del README |
| 2026-02-04 | Creación de estructura inicial de documentación |

---

*Última actualización: 2026-03-02*
