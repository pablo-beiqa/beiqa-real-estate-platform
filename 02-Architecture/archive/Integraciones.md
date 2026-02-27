# Mapa de Integraciones

> **Estado**: ✅ Activo | **Fecha**: Febrero 2026
>
> Este documento describe cómo se conectan todos los sistemas del stack de BEIQA.

---

## Diagrama de sistemas

```
┌─────────────────────────────────────────────────────────────┐
│                    FUENTES DE DATOS                         │
│  Inmuebles24    EasyBroker (futuro)    Otros portales       │
└──────────┬───────────────────────────────────────────────────┘
           │ scraping
           ▼
┌──────────────────┐
│      Apify       │  Actor contratado por portal
│  (actor i24)     │  Maneja: proxies, rate limiting, anti-bot
└──────────┬───────┘
           │ webhook (listing data JSON)
           ▼
┌──────────────────┐         ┌─────────────────────┐
│      n8n         │────────▶│      Slack           │
│  (orquestador)   │ notif   │  (alertas, triggers) │
└──────┬───────────┘         └─────────────────────┘
       │                              │
       │ upsert listings              │ trigger manual
       ▼                              ▼
┌──────────────────────────────────────────────────┐
│                   Supabase                        │
│  PostgreSQL + PostGIS + Auth + Storage + REST     │
│                                                   │
│  Tablas: inmuebles24_listings, brokers,           │
│          empresas_brokers, tenants,               │
│          portales_config, error_logs,             │
│          possible_duplicates                      │
│                                                   │
│  Triggers: populate_geo                           │
│  RPCs: tendencia_precios, precio_promedio_m2      │
└──────┬──────────────────────┬────────────────────┘
       │                      │
       │ trigger job           │ sync one-way
       ▼                      ▼
┌──────────────┐    ┌─────────────────────┐
│ Trigger.dev  │    │      HubSpot         │
│  (jobs TS)   │    │  (CRM — deals,       │
│              │    │   clientes, comms)   │
│  batch AI    │    └──────────┬──────────┘
│  extraction  │               │
└──────┬───────┘               │ enriquecimiento
       │                       ▼
       │             ┌─────────────────────┐
       │             │        Clay          │
       │             │  (data enrichment    │
       │             │   brokers/empresas)  │
       │             └─────────────────────┘
       │
       │ Claude API calls
       ▼
┌──────────────────┐
│   Claude API     │  vía Rube (~3x más barato que OpenAI)
│  (AI / NLP)      │  Extracción de features de descripciones
└──────────────────┘

                    ┌─────────────────────┐
                    │      Next.js         │
                    │  Internal App +      │◀─── Supabase REST/Auth
                    │  Tenant Portal       │
                    │  (Pamela — Fase 1)   │
                    └─────────────────────┘
```

---

## Flujos detallados

### Flujo 1: Scraping automático (cron semanal)

```
n8n cron job (semanal)
  → llama Apify Run API
  → Apify ejecuta actor de Inmuebles24
  → Apify finaliza → webhook a n8n
  → n8n hace upsert en Supabase (inmuebles24_listings)
  → trigger populate_geo calcula geo_point
  → n8n envía notificación a Slack
```

### Flujo 2: Scraping manual (trigger Slack)

```
Jerónimo/Fabrizio escribe comando en Slack
  → n8n webhook escucha el mensaje
  → n8n llama Apify Run API
  → mismos pasos del Flujo 1
```

### Flujo 3: Batch AI extraction (Trigger.dev)

```
Trigger.dev job se activa (cron o manual)
  → consulta Supabase: listings sin property_features
  → para cada listing, llama Claude API (vía Rube)
  → Claude extrae features estructuradas de la descripción
  → Trigger.dev escribe property_features en Supabase
```

### Flujo 4: Sync HubSpot (one-way)

```
Supabase → n8n → HubSpot
  → Cuando un broker nuevo entra (vía scraping)
  → n8n crea/actualiza contacto en HubSpot
  → Cuando una propiedad se asigna a un deal
  → n8n actualiza el deal en HubSpot

HubSpot → Supabase (minimal)
  → Deal status changes se sincronizan de vuelta
```

### Flujo 5: Data enrichment (Clay)

```
HubSpot (broker/empresa sin datos)
  → Clay enriquece: LinkedIn, web, teléfono, etc.
  → Clay actualiza el contacto/empresa en HubSpot
  → Clay NO escribe directamente a Supabase
```

---

## División de responsabilidades: n8n vs Trigger.dev

Esta distinción es importante para saber dónde implementar nueva lógica.

| n8n | Trigger.dev |
|-----|-------------|
| Webhooks de entrada | Jobs que requieren TypeScript |
| Cron scheduling | Batch processing (miles de registros) |
| Notificaciones Slack | Lógica compleja con Claude API |
| Sync HubSpot ↔ Supabase | Procesamiento de property_features |
| Integraciones visuales | Futuro: parsing de transcripts |
| Disparar jobs de Trigger.dev | — |

**Regla simple**: Si puedes hacerlo arrastrando nodos en n8n → usa n8n. Si necesitas TypeScript real + loops + AI en batch → usa Trigger.dev.

---

## Source of Truth Rules

Principio de Alex (llamada 23 feb): "Mientras menos fuentes del mismo dato tengan, mucho mejor."

| Dato | Source of Truth | No duplicar en |
|------|----------------|---------------|
| Propiedades / listings | Supabase | — |
| Brokers y empresas | Supabase (datos base) + HubSpot (datos enriquecidos) | Mantener sync |
| Geoespacial / analytics | Supabase | — |
| Clientes / tenants | HubSpot | Supabase solo guarda hubspot_deal_id |
| Deals y pipeline comercial | HubSpot | — |
| Comunicaciones con clientes | HubSpot | — |

**Sincronizaciones activas:**
- Supabase → HubSpot: propiedades y brokers (one-way)
- HubSpot → Supabase: deal status (one-way, minimal)
- Clay → HubSpot: enriquecimiento (no toca Supabase)

---

## Integraciones planificadas (Fase 2+)

| Integración | Para qué | Cuándo |
|-----------|---------|--------|
| Google Places API | POIs cercanos a propiedades | Fase 2 — Data Ingestion |
| INEGI DENUE API | Datos socioeconómicos por zona | Fase 2 — Data Ingestion |
| H3 (PostgreSQL extension) | Index geoespacial hexagonal para analytics | Fase 2 — Geospatial |
| AGEB shapefiles (INEGI) | Cruzar propiedades con unidades censales | Fase 2 — Geospatial |
| CircleBack | Transcripción de llamadas con clientes | Fase 3 — AI Brain |
| EasyBroker (Apify actor) | Segundo portal de scraping | Fase 1 — Scrapers (pendiente) |

---

*Documento creado: 2026-02-24*
