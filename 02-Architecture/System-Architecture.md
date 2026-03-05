# Arquitectura del Sistema — BEIQA Platform

> **Estado**: ✅ Stack decidido e implementado | **Actualizado**: 2026-03-05
>
> Para el detalle de cada decisión técnica: [Stack-Decidido.md](./Stack-Decidido.md)
> Para cada ADR individual: [ADRs/README.md](ADRs/README.md)
> Para la arquitectura de agentes AI: [Agent-Architecture.md](./Agent-Architecture.md)

---

## Diagrama de arquitectura (stack real — marzo 2026)

```mermaid
flowchart TB
    subgraph Portales[Portales Inmobiliarios]
        I24[Inmuebles24]
        Pincali[Pincali]
        CBRE[CBRE]
        Colliers[Colliers]
    end

    subgraph ScrapingI24[Scraping — I24]
        Apify[Apify\nactor contratado]
    end

    subgraph ScrapingCustom[Scraping — Portales Custom]
        Firecrawl[Firecrawl\nHTTP engine + LLM extraction]
        Browserbase[Browserbase\ncloud browser sessions]
    end

    subgraph Automation[Ejecución Durable — Trigger.dev]
        ScraperTasks[Scraper tasks\nPincali, CBRE, Colliers]
        Persistence[Persistencia\nupsert a staging tables]
        HubspotSync[Sync HubSpot\none-way]
        CronJobs[Cron jobs\nscheduled runs]
        HTTPTrigger[HTTP trigger\na Mastra post-scrape]
    end

    subgraph AIBrain[AI Brain — Mastra ⚡ Capa Transversal]
        AddressAgent[Address Enrichment\nAgent]
        NormAgent[Data Normalization\nAgent]
        DedupAgent[Deduplication\nAgent]
        ScoringAgent[Scoring / Matching\nAgent]
        MarketAgent[Market Intelligence\nAgent]
        GISAgent[GIS Analysis\nAgent]
    end

    subgraph DB[Base de Datos — Supabase]
        PG[(PostgreSQL 15\n+ PostGIS)]
        GoldenRecord[(Golden Record\nproperties)]
        StagingTables[(Staging Tables\ninmuebles24, pincali...)]
        Auth[Supabase Auth]
        Storage[Supabase Storage]
        REST[REST API\nautomática]
    end

    subgraph CRM[CRM]
        HubSpot[HubSpot]
    end

    subgraph Frontend[Frontend — Next.js 15]
        InternalApp[Internal App\napp.beiqa.com]
        TenantPortal[Tenant Portal\nportal.beiqa.com]
    end

    subgraph UI[Interfaz Actual — Rube]
        Rube[Rube + Claude Desktop\nMCP bridge]
    end

    subgraph Geo[Geoespacial]
        Google[Google Maps\nGeocoding + Places]
        H3[H3 Index]
        ArcGIS[ArcGIS\nvía MCP]
    end

    subgraph LLMs[Modelos LLM]
        LLM[Claude / GPT / etc\nmodelo TBD por agente]
    end

    subgraph Monitoring[Monitoreo]
        Slack[Slack\nalertas]
    end

    %% Scraping flow
    I24 --> Apify
    Pincali --> Firecrawl
    Pincali --> Browserbase
    CBRE --> Firecrawl
    Colliers --> Firecrawl
    Colliers --> Browserbase
    Apify -->|webhook| Persistence
    Firecrawl --> ScraperTasks
    Browserbase --> ScraperTasks
    ScraperTasks --> Persistence
    Persistence -->|upsert| StagingTables
    CronJobs --> ScraperTasks
    CronJobs --> Apify

    %% Trigger.dev → Mastra
    Persistence -->|HTTP POST| HTTPTrigger
    HTTPTrigger -->|trigger enrichment| AddressAgent

    %% AI Agent flows
    StagingTables -->|lee datos crudos| AddressAgent
    AddressAgent --> NormAgent
    NormAgent --> GoldenRecord
    GoldenRecord --> DedupAgent
    AddressAgent -->|geocoding| Google
    GISAgent -->|H3, AGEB| H3
    GISAgent -->|MCP| ArcGIS
    AddressAgent --> LLM
    NormAgent --> LLM
    DedupAgent --> LLM
    ScoringAgent --> LLM
    MarketAgent --> LLM

    %% Data consumption
    GoldenRecord --> REST
    REST --> InternalApp
    REST --> TenantPortal
    InternalApp -->|scoring request| ScoringAgent
    TenantPortal -->|scoring request| ScoringAgent

    %% HubSpot (determinístico, queda en Trigger.dev)
    PG --> HubspotSync --> HubSpot

    %% Rube (transitorio)
    REST --> Rube
    Auth --> Rube
    Rube -->|MCP| PG
    Rube -->|MCP| HubSpot
    Rube -->|MCP| Slack

    %% Monitoring
    Persistence --> Slack
    HTTPTrigger --> Slack
```

---

## Componentes del sistema

### Capa de scraping

#### Inmuebles24 — Apify
- **ADR**: [ADR-002](ADRs/ADR-002-Estrategia-Scraping.md)
- **Responsabilidad**: Extracción de propiedades de Inmuebles24
- **Implementación**: Actor contratado, maneja rate limiting, proxies, anti-bot internamente
- **Output**: JSON con datos del listing → webhook a Trigger.dev

#### Pincali, CBRE, Colliers — Firecrawl + Browserbase
- **ADRs**: [ADR-007](ADRs/ADR-007-Firecrawl.md), [ADR-008](ADRs/ADR-008-Browserbase.md)
- **Firecrawl** ($99/mo): Motor HTTP con JS rendering, LLM extraction, stealth proxy
- **Browserbase** ($20/mo): Cloud browser para Colliers (downloads) y Pincali (broker contact)
- **Código**: Trigger.dev tasks en repo `beiqa-scraper`

---

### Ejecución Durable (Trigger.dev)

- **ADR**: [ADR-003](ADRs/ADR-003-Trigger-dev.md)
- **Repo**: `github.com/pablo-beiqa/beiqa-scraper`
- **Scope**: Scrapers custom, persistencia a Supabase staging tables, sync HubSpot, cron jobs, HTTP trigger a Mastra post-scrape
- **NO es**: el backend, ni el motor de AI/NLP (→ Mastra), ni el "cerebro" del sistema

**Tasks activos**:
| Task | Función |
|------|---------|
| `pincali-scraper` | Scraping de Pincali via Firecrawl |
| `cbre-scraper` | Scraping de CBRE via Firecrawl |
| `colliers-scraper` | Scraping de Colliers via Firecrawl + Browserbase |
| `persist-to-supabase` | Upsert de datos scrapeados a staging tables |
| `sync-hubspot` | Sync one-way Supabase → HubSpot (en migración) |
| `trigger-mastra` | HTTP POST a Mastra post-scrape (por implementar) |

> **Nota**: `batch-ai-extraction` migra a Mastra como Address Enrichment + Data Normalization Agents. Ver [ADR-020](ADRs/ADR-020-Mastra.md).

---

### AI Brain — Mastra (Capa Transversal)

- **ADR**: [ADR-020](ADRs/ADR-020-Mastra.md)
- **Separación de responsabilidades**: [ADR-021](ADRs/ADR-021-Separacion-Trigger-Mastra.md)
- **Repo**: `github.com/pablo-beiqa/beiqa-agents`
- **Framework**: [Mastra](https://mastra.ai) (TypeScript, open source, Apache 2.0)
- **Scope**: Orquestación de agentes AI — enrichment, normalization, deduplication, scoring, market intelligence, GIS analysis
- **Comunicación**: HTTP API (Hono server) + Supabase shared DB + MCP clients (ArcGIS, etc.)

**Agentes**:
| Agente | Prioridad | Módulo que sirve |
|--------|-----------|-----------------|
| Address Enrichment | P0 | Data, Geospatial |
| Data Normalization | P0 | Data |
| Deduplication | P1 | Data |
| Scoring / Matching | P1 | Internal App, Tenant Portal |
| Market Intelligence | P2 | Market Intelligence |
| GIS Analysis | P2 | Geospatial |

Ver arquitectura completa de agentes: [Agent-Architecture.md](./Agent-Architecture.md)

---

### Base de datos (Supabase)

- **ADR**: [ADR-001](ADRs/ADR-001-Supabase-Plataforma.md)
- **Motor**: PostgreSQL 15 + PostGIS
- **Auth**: Supabase Auth (JWT, sin Auth0)
- **Storage**: Supabase Storage (imágenes, documentos)
- **API**: REST automática generada desde el schema
- **RLS**: Row Level Security configurado
- **14 migrations** activas
- **~60,000 propiedades** en `inmuebles24_listings`

**Estructura de datos** ([ADR-012](ADRs/ADR-012-Multi-Portal-Data.md)):
- **Staging tables**: `inmuebles24_listings`, `pincali_listings`, `cbre_listings`, `colliers_listings` — datos crudos por portal
- **Golden record**: `properties` — datos normalizados, deduplicados, enriquecidos por Mastra
- **Source of truth**: Supabase es source of truth para propiedades, listings, brokers, analytics, geo data

Ver schema: [Database/Schema-Real.md](./Database/Schema-Real.md)

---

### Frontend (Next.js 15)

- **ADR**: [ADR-015](ADRs/ADR-015-Frontend-Next-js.md)
- **Repo**: `github.com/pablo-beiqa/beiqa-frontend`
- **Internal App** (`app.beiqa.com`): Dashboard del equipo — gestión de propiedades, shortlists, analytics
- **Tenant Portal** (`portal.beiqa.com`): Portal de clientes — scoring, shortlists compartidas, feedback
- **Consume**: Supabase REST API (golden record) + Mastra API (scoring on-demand)
- **Estado**: Phase 0 completo, en desarrollo

---

### Interfaz actual (Rube + Claude Desktop)

- **ADR**: [ADR-004](ADRs/ADR-004-Rube-MCP-Bridge.md)
- **Función**: Bridge MCP que conecta Claude Desktop con Supabase, HubSpot, Slack
- **Scope**: SOLO interacción humana. Los pipelines automáticos van por Trigger.dev y Mastra.
- **Transitorio**: Se reemplazará progresivamente por Internal App + Tenant Portal

---

### CRM y Data Enrichment

- **ADR**: [ADR-005](ADRs/ADR-005-HubSpot-CRM.md)
- **HubSpot**: CRM para clientes (tenants), deals y pipeline comercial
- **Sincronización**: Supabase → HubSpot (one-way vía Trigger.dev, determinístico). HubSpot → Supabase (deal status, minimal).

---

### Geoespacial

- **H3 Indexing** ([ADR-009](ADRs/ADR-009-H3-Indexing.md)): Sistema hexagonal, capas 5-11 — calculado por GIS Agent post-enrichment
- **AGEB/INEGI** ([ADR-010](ADRs/ADR-010-AGEB-INEGI.md)): Polígonos censales — asignados por GIS Agent via PostGIS spatial join
- **Google Maps** ([ADR-011](ADRs/ADR-011-Google-Maps-Platform.md)): Geocoding + Places API — usado como tool del Address Enrichment Agent
- **ArcGIS**: Análisis geoespacial avanzado — consumido vía MCP server por GIS Agent
- **PostGIS**: Trigger `populate_geo` operando, índices GIST activos

---

## Flujos de datos principales

### Flujo 1: Scraping Inmuebles24 (Apify)

```mermaid
sequenceDiagram
    participant Cron as Trigger.dev Cron
    participant Apify
    participant Portal as Inmuebles24
    participant TDev as Trigger.dev
    participant Supabase
    participant Mastra
    participant Slack

    Cron->>Apify: Trigger run
    Apify->>Portal: Scraping (proxies + rate limiting)
    Portal-->>Apify: HTML → JSON listings
    Apify-->>TDev: Webhook (run completado)
    TDev->>Supabase: Upsert listings (inmuebles24_listings)
    Note over Supabase: trigger populate_geo
    TDev->>Mastra: HTTP POST (new listings available)
    TDev->>Slack: Notificación completado
```

### Flujo 2: Scraping portales custom (Firecrawl + Trigger.dev)

```mermaid
sequenceDiagram
    participant Cron as Trigger.dev Cron
    participant FC as Firecrawl
    participant BB as Browserbase
    participant TDev as Trigger.dev Task
    participant Supabase
    participant Mastra
    participant Slack

    Cron->>TDev: Trigger scraper task
    TDev->>FC: Fetch páginas (HTTP + JS render)
    FC-->>TDev: HTML/JSON extraído
    opt Scraping complejo
        TDev->>BB: Abrir sesión browser
        BB-->>TDev: Datos descargados
    end
    TDev->>TDev: Limpiar + normalizar datos
    TDev->>Supabase: Upsert a staging table
    TDev->>Mastra: HTTP POST (new listings available)
    TDev->>Slack: Notificación completado
```

### Flujo 3: Enrichment Pipeline (Mastra)

```mermaid
sequenceDiagram
    participant Mastra as Mastra Orchestrator
    participant AE as Address Enrichment Agent
    participant DN as Data Normalization Agent
    participant Google as Google Maps API
    participant LLM as LLM (TBD)
    participant Supabase

    Mastra->>Supabase: Query: staging WHERE enrichment_status = 'pending'
    Supabase-->>Mastra: Batch de listings nuevos
    loop Por cada listing
        Mastra->>AE: Enriquecer dirección
        AE->>Google: Geocode dirección
        Google-->>AE: Coordenadas + formatted_address
        AE->>Google: Reverse geocode coordenadas originales
        Google-->>AE: Dirección verificación
        AE->>LLM: Extraer dirección de descripción
        LLM-->>AE: Dirección + landmarks
        AE->>AE: Calcular confidence score
        AE->>Supabase: UPDATE staging (corrected address + confidence)
        Mastra->>DN: Normalizar al golden record
        DN->>LLM: Extraer features de descripción
        LLM-->>DN: Features estructuradas
        DN->>Supabase: UPSERT properties (golden record)
    end
    Mastra->>Supabase: Log en agent_runs
```

### Flujo 4: Scoring on-demand (Mastra)

```mermaid
sequenceDiagram
    participant FE as Frontend
    participant Mastra as Mastra API
    participant SA as Scoring Agent
    participant LLM as LLM (TBD)
    participant Supabase

    FE->>Mastra: POST /api/agents/scoring/generate
    Mastra->>SA: Generar scoring
    SA->>Supabase: Query propiedades (filtros del requerimiento)
    Supabase-->>SA: Propiedades candidatas
    SA->>LLM: Evaluar fit propiedad-requerimiento
    LLM-->>SA: Score + justificación por propiedad
    SA->>Supabase: INSERT scoring_report + scoring_results
    SA-->>Mastra: Shortlist generada
    Mastra-->>FE: Respuesta con shortlist
```

### Flujo 5: Sync HubSpot (one-way, Trigger.dev)

```mermaid
sequenceDiagram
    participant TDev as Trigger.dev
    participant Supabase
    participant HubSpot

    TDev->>Supabase: Detectar brokers/propiedades nuevos
    Supabase-->>TDev: Datos nuevos
    TDev->>HubSpot: Crear/actualizar contacto o deal
    HubSpot-->>TDev: hubspot_contact_id
    TDev->>Supabase: UPDATE SET hubspot_id
```

---

## Lo que NO existe (y no se necesita hoy)

| Componente eliminado | Reemplazado por |
|--------------------|----------------|
| n8n Cloud | Trigger.dev ([ADR-019](ADRs/ADR-019-n8n-Deprecado.md)) |
| Backboard.io | Mastra memory ([ADR-020](ADRs/ADR-020-Mastra.md), supersede [ADR-014](ADRs/ADR-014-Backboard.md)) |
| Python / Scrapy | Apify + Firecrawl |
| FastAPI / Express | Supabase REST automático |
| GraphQL API | REST auto-generado |
| Redis cache | No necesario con volumen actual |
| Auth0 / Clerk | Supabase Auth |
| AWS S3 / Cloudflare R2 | Supabase Storage |
| Sentry / Datadog | Slack + `error_logs` table |
| EasyBroker | Descartado ([ADR-002](ADRs/ADR-002-Estrategia-Scraping.md)) |

---

*Documento actualizado: 2026-03-05 | Versión anterior archivada en [archive/](archive/)*
