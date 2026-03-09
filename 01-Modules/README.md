# Módulos de la Plataforma BEIQA

> Hub central de todos los módulos funcionales. Cada módulo es auto-contenido con su descripción, preguntas de producto, requerimientos e investigación técnica.

---

## Mapa de Módulos

| Módulo | Descripción | Sprint | Estado |
|--------|-------------|--------|--------|
| [Scraper](./Scraper/) | Extracción automatizada de propiedades (Trigger.dev + Firecrawl). CBRE/Colliers/FINSA en producción. I24 migrando de Apify | Sprint 1+ | 🟢 En desarrollo |
| [Data](./Data/) | Normalización (Mastra agents), integración de fuentes externas | Sprint 1-2 | 🟢 En desarrollo |
| [AI Brain](./AI-Brain/) | Agentes AI con Mastra (enrichment, normalization, matching) | Sprint 1+ | 🟢 En desarrollo |
| [Geospatial](./Geospatial/) | Análisis geoespacial, H3, AGEB, mapas | Sprint 3+ | 🟡 En pruebas |
| [Tenant Portal](./Tenant-Portal/) | Portal web para clientes: scoring, shortlists, feedback | Sprint 1+ | 🟡 En desarrollo |
| [Internal App](./Internal-App/) | Aplicación web para el equipo Beiqa (Next.js — Pamela) | Sprint 5+ | 🟡 En diseño |
| [Market Intelligence](./Market-Intelligence/) | Análisis de mercado, tendencias, reportes automatizados | Sprint 4+ | 🔴 Por iniciar |

> **Nota**: La Base de Datos (PostgreSQL + PostGIS) vive en [02-Architecture/Database/](../02-Architecture/Database/) como infraestructura compartida.

> **Mastra como capa transversal**: [Mastra](../02-Architecture/Agent-Architecture.md) es el framework de orquestación de agentes AI que opera como capa transversal a todos los módulos. Los agentes de Mastra (Data Normalization Agent, Address Enrichment Agent, GIS Analysis Agent, etc.) proporcionan capacidades de enriquecimiento, normalización e inteligencia que cruzan las fronteras de los módulos individuales.

---

## Mapeo a Sprints del Proyecto

### Sprint 1-2 — Core: Scrapers, Data, AI Brain, Auth (En curso)

| Módulo | Alcance |
|--------|---------|
| **Scraper** | CBRE/Colliers/FINSA ✅ producción. Pincali persist Sprint 1. I24 migración Apify→Trigger.dev Sprint 1-2. Golden record staging |
| **Data** | Golden record schema, normalización vía Mastra agents |
| **AI Brain** | Address Enrichment Agent, Data Normalization Agent, LLM eval |
| **Tenant Portal** | Supabase Auth (magic link + password), scoring desde DB |
| **Database** *(Arquitectura)* | Supabase activo ✅, 14 migrations ✅, ~30K propiedades I24 ✅ |

### Sprint 3-4 — Inteligencia: Dedup, Scoring AI, Geospatial

| Módulo | Alcance |
|--------|---------|
| **Data** | Deduplication Agent, backfill completo |
| **AI Brain** | Scoring/Matching Agent (migra de frontend) |
| **Geospatial** | H3 indexer, AGEB lookup, GIS Analysis Agent |
| **Market Intelligence** | Market Intelligence Agent, reportes por zona |
| **Tenant Portal** | Shortlists UI, feedback, mapa de opciones |

### Sprint 5+ — Experiencia y Operaciones

| Módulo | Alcance |
|--------|---------|
| **Internal App** | Next.js — lee golden record, dashboard interno (Pamela diseño) |
| **Tenant Portal** | Portal live, pipeline E2E |

---

## Diagrama de Dependencias

```mermaid
flowchart TD
    subgraph MASTRA["Mastra — Capa Transversal de AI"]
        direction LR
        MA_NORM[Data Normalization Agent]
        MA_ADDR[Address Enrichment Agent]
        MA_GIS[GIS Analysis Agent]
        MA_MATCH[Matching Agent]
    end

    subgraph S1["Sprint 1-2 — Core"]
        SCR[Scraper]
        DB[(Database<br/>Arquitectura)]
        DATA[Data]
        AI[AI Brain]
        TP[Tenant Portal]
    end

    subgraph S2["Sprint 3-4 — Inteligencia"]
        GEO[Geospatial]
        MI[Market Intelligence]
    end

    subgraph S3["Sprint 5+ — Operaciones"]
        APP[Internal App]
    end

    MASTRA -.->|enriquecimiento| SCR
    MASTRA -.->|normalización| DATA
    MASTRA -.->|análisis geo| GEO
    MASTRA -.->|matching| AI
    MASTRA -.->|inteligencia| MI

    SCR --> DB
    DATA --> DB
    DB --> APP
    DB --> GEO
    DB --> TP
    SCR --> MI
    SCR --> TP
    DATA --> MI
    DATA --> GEO
    GEO --> MI
    MI --> AI
    GEO --> AI
    SCR --> AI
    DB --> AI
    AI --> APP
    MI --> APP
    GEO --> APP
```

---

## Estructura Estándar de Cada Módulo

Cada módulo contiene:

```
Módulo/
├── README.md              # Overview: descripción, objetivos, métricas, entregables, dependencias, riesgos
├── Product-Questions.md   # Cuestionario de discovery (preguntas de producto)
├── Requirements.md        # Capacidades con priorización Must/Should/Could
└── Research/              # Investigación técnica (docs específicos del módulo)
```

---

## Cómo Navegar

1. **Elige un módulo** de la tabla de arriba
2. **Lee el README.md** para entender qué hace, sus objetivos y métricas
3. **Responde el Product-Questions.md** para informar el diseño
4. **Consulta Requirements.md** para ver las capacidades definidas
5. **Explora Research/** para la investigación técnica de soporte

---

*Última actualización: 2026-03-08*
