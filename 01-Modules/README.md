# Módulos de la Plataforma BEIQA

> Hub central de todos los módulos funcionales. Cada módulo es auto-contenido con su descripción, preguntas de producto, requerimientos e investigación técnica.

---

## Mapa de Módulos

| Módulo | Descripción | Sprint | Estado |
|--------|-------------|--------|--------|
| [Scraper](./Scraper/) | Extracción automatizada de propiedades (Apify + TriggerDev + Firecrawl) | Sprint 1+ | 🟢 En desarrollo |
| [Internal App](./Internal-App/) | Aplicación web para el equipo Beiqa (Next.js — Pamela) | Sprint 2+ | 🟡 Diseño activo |
| [Data](./Data/) | Normalización (Mastra agents), integración de fuentes externas (INEGI, Google, catastro) | Sprint 1+ | 🟢 En desarrollo |
| [Market Intelligence](./Market-Intelligence/) | Análisis de mercado, tendencias, reportes automatizados | Sprint 3+ | 🔴 Por iniciar |
| [Geospatial](./Geospatial/) | Análisis geoespacial, H3, AGEB, mapas | Sprint 2+ | 🟡 En pruebas |
| [Tenant Portal](./Tenant-Portal/) | Portal web para clientes: scoring, shortlists, feedback | Sprint 3+ | 🟡 En diseño |
| [AI Brain](./AI-Brain/) | Matching inteligente, NLP, procesamiento de llamadas | Sprint 1+ | 🟢 En desarrollo |

> **Nota**: La Base de Datos (PostgreSQL + PostGIS) vive en [02-Architecture/Database/](../02-Architecture/Database/) como infraestructura compartida.

> **Mastra como capa transversal**: [Mastra](../02-Architecture/Agent-Architecture.md) es el framework de orquestación de agentes AI que opera como capa transversal a todos los módulos. Los agentes de Mastra (Data Normalization Agent, Address Enrichment Agent, GIS Analysis Agent, etc.) proporcionan capacidades de enriquecimiento, normalización e inteligencia que cruzan las fronteras de los módulos individuales.

---

## Mapeo a Sprints del Proyecto

### Sprint 1+ — Scrapers, Inventario & AI Brain (En curso)

| Módulo | Alcance |
|--------|---------|
| **Scraper** | Apify actor Inmuebles24 ✅, TriggerDev como plataforma primaria, Firecrawl + Browserbase para portales custom |
| **Internal App** | Next.js — lista propiedades, mapa, filtros, shortlists (Pamela) |
| **Data** | Normalización vía Mastra agents, golden record (properties) |
| **AI Brain** | Agentes Mastra en implementación (enrichment, normalization, matching) |
| **Database** *(Arquitectura)* | Supabase activo ✅, 14 migrations ✅, ~30K propiedades ✅ |

### Sprint 2+ — Portal Web + Geospatial

| Módulo | Alcance |
|--------|---------|
| **Tenant Portal** | Portal para clientes: shortlists, feedback, mapa de opciones |
| **Geospatial** | H3 + AGEB + Atlas.co visualización + GIS Analysis Agent (Mastra) |

### Sprint 3+ — Data Ingestion + Market Intelligence

| Módulo | Alcance |
|--------|---------|
| **Data (Ingestion)** | INEGI DENUE, Google Places, AGEB shapefiles, indicadores económicos |
| **Market Intelligence** | Tendencias de precio, precio promedio m2, heatmaps por zona |

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

    subgraph S1["Sprint 1+ — Core"]
        SCR[Scraper]
        DB[(Database<br/>Arquitectura)]
        APP[Internal App]
        AI[AI Brain]
    end

    subgraph S2["Sprint 2+ — Expansión"]
        DATA[Data]
        GEO[Geospatial]
        TP[Tenant Portal]
    end

    subgraph S3["Sprint 3+ — Inteligencia"]
        MI[Market Intelligence]
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

*Última actualización: 2026-03-05*
