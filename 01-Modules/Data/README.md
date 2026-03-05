# Data

**Sprint del proyecto**: Sprint 1+ (Normalization) + Sprint 3+ (Ingestion)
**Estado**: 🟢 En desarrollo
**Owner**: Pablo + Fabrizio

---

## Descripcion

El modulo de Data agrupa tres sub-modulos complementarios:

1. **Database (Supabase)**: Base de datos PostgreSQL + PostGIS + Storage que soporta toda la plataforma. Documenta la implementacion concreta en Supabase.

2. **Data Normalization** (Sprint 1+ — Core): Normalización profunda, deduplicacion cross-portal, y limpieza de datos que vienen del Scraper. Ejecutado por el **Data Normalization Agent** de Mastra, que procesa datos en staging tables, los enriquece y los promueve al golden record. Garantiza que los datos en Supabase sean consistentes, deduplicados y enriquecidos con H3/AGEB.

3. **Data Ingestion** (Sprint 3+ — Post-MVP): Integracion de fuentes de datos externos estructurados que enriquecen las propiedades con contexto: datos demograficos del INEGI, POIs de Google Maps, datos catastrales, e indicadores economicos.

---

## Pipeline de Datos: Staging → Golden Record

El flujo de datos sigue un patrón de staging → enriquecimiento → golden record:

```
┌─────────────┐     ┌──────────────────────┐     ┌─────────────────┐
│   Scraper   │────→│   Staging Tables     │────→│  Golden Record  │
│  (TriggerDev)│     │  (datos crudos)      │     │  (properties)   │
└─────────────┘     └──────────────────────┘     └─────────────────┘
                            │                           ▲
                            ▼                           │
                    ┌──────────────────────┐            │
                    │  Mastra Enrichment   │────────────┘
                    │  (Data Normalization │
                    │   Agent)             │
                    └──────────────────────┘
```

### Golden Record Tables

| Tabla | Descripcion |
|-------|-------------|
| `properties` | Tabla principal (golden record) — propiedades normalizadas, deduplicadas y enriquecidas |
| `property_sources` | Registro de cada fuente/portal que reporta una propiedad (trazabilidad cross-portal) |
| `enrichment_queue` | Cola de propiedades pendientes de enriquecimiento por agentes Mastra |

---

## Sub-modulos

### Database (Supabase)

Base de datos PostgreSQL + PostGIS alojada en Supabase. Incluye tablas core (propiedades, clientes, zonas), Supabase Storage para imagenes, e indices geoespaciales.

Ver documentacion detallada en [Database/](./Database/)

### Data Normalization (Sprint 1+)

Normalización profunda ejecutada por el **Data Normalization Agent** de Mastra, que opera sobre datos en staging tables.

**Responsabilidades del agente**:
- Deduplicacion cross-portal (misma propiedad en Inmuebles24, Pincali, CBRE, Colliers, FinSA)
- Geocodificacion batch (propiedades sin coordenadas o con baja confianza) via Address Enrichment Agent
- Validacion y correccion de coordenadas
- Normalizacion avanzada de direcciones
- Asignacion de H3 hexagonos y AGEB a propiedades
- Merge de registros duplicados preservando la mejor informacion
- Promocion de staging tables al golden record (`properties`)

> **Nota**: La normalizacion basica (precios→MXN, superficies→m², tipos→catalogo) se hace dentro del pipeline del Scraper (TriggerDev) antes de guardar en staging. El Data Normalization Agent de Mastra hace la limpieza PROFUNDA y la promocion al golden record.

Ver documentacion detallada en [Normalization/](./Normalization/)

### Data Ingestion (Sprint 3+)

Integracion de fuentes externas que enriquecen las propiedades con contexto de zona.

**Fuentes planificadas**:
- INEGI DENUE — Unidades economicas por ubicacion y actividad (SCIAN)
- INEGI Cartografia — Poligonos AGEB, limites municipales
- Google Maps/Places — POIs cercanos a cada propiedad
- Datos catastrales — Valor catastral y uso de suelo
- Indicadores economicos — Datos macro por region (INEGI, Banxico, IMSS)

Ver documentacion de investigacion en [Research/](./Research/)

---

## Objetivos

1. **Normalization**: Deduplicar cross-portal al menos el 90% de propiedades duplicadas detectables y asignar H3+AGEB al 100% de propiedades geocodificadas
2. **Ingestion**: Enriquecer al menos el 70% de propiedades con perfil de zona completo (demografico, economico, POIs)
3. Reducir el tiempo de investigacion contextual de zonas de 2+ horas a <5 minutos por propiedad

---

## Metricas de Exito / KPIs

| Metrica | Target | Como se mide | Sub-modulo |
|---------|--------|--------------|------------|
| Duplicados resueltos | >=90% de duplicados cross-portal detectados y mergeados | Pares detectados vs pares confirmados | Normalization |
| Cobertura H3+AGEB | 100% de propiedades geocodificadas con H3 y AGEB asignado | Propiedades con H3/AGEB / propiedades con coordenadas | Normalization |
| Fuentes de datos integradas | >=5 fuentes activas | Conectores con datos actualizados en ultimos 30 dias | Ingestion |
| Cobertura de enriquecimiento | >=70% de propiedades con perfil de zona completo | Propiedades con >=4 campos enriquecimiento / total | Ingestion |
| Tiempo de investigacion de zona | <5 minutos por propiedad | Medicion con equipo operaciones | Ingestion |
| Costo mensual de APIs externas | <$500 USD/mes | Monitoreo de consumo | Ingestion |

---

## Entregables Clave

| Entregable | Descripcion | Sub-modulo | Estado |
|-----------|-------------|------------|--------|
| Data Normalization Agent (Mastra) | Agente que ejecuta deduplicacion, normalizacion profunda y promocion al golden record | Normalization | 🟢 En desarrollo |
| Golden record tables | Tablas `properties`, `property_sources`, `enrichment_queue` | Normalization | 🟡 En progreso |
| Pipeline staging → golden record | Flujo automatizado desde staging tables hasta golden record via Mastra | Normalization | 🟡 En progreso |
| Asignacion H3 + AGEB | Asignar hexagono H3 y AGEB a cada propiedad para integracion GIS | Normalization | 🟡 En progreso |
| Geocodificacion batch | Geocodificar propiedades sin coordenadas o con baja confianza (via Address Enrichment Agent) | Normalization | 🟡 En progreso |
| Conector INEGI (DENUE + Cartografia) | Pipeline para datos economicos y poligonos AGEB | Ingestion | 🔴 |
| Conector Google Maps/Places | POIs cercanos a cada propiedad | Ingestion | 🔴 |
| Conector datos catastrales | Valor catastral y uso de suelo | Ingestion | 🔴 |
| Conector indicadores economicos | Datos macro por region | Ingestion | 🔴 |
| Workflows de enriquecimiento | Enriquecimiento automatico al ingresar propiedad nueva (via enrichment_queue) | Ingestion | 🔴 |

---

## Dependencias

### Necesita (upstream)
- **Base de datos (Arquitectura)** → Esquema para propiedades, zonas, POIs, demograficos
- **Scraper** → Proporciona registros base de propiedades con ubicacion (lat/lng) en staging tables
- **Supabase** → PostGIS habilitado para queries geoespaciales
- **Mastra** → Framework de agentes para Data Normalization Agent y Address Enrichment Agent

### Depende de este (downstream)
- **Market Intelligence** → Datos demograficos y economicos para reportes
- **Geospatial** → Zonas INEGI, POIs, limites catastrales para visualizacion en mapa
- **AI Brain** → Datos enriquecidos para matching inteligente

---

## Riesgos Clave

| # | Riesgo | Impacto | Probabilidad | Mitigacion |
|---|--------|---------|--------------|------------|
| 1 | Costos de APIs externas exceden presupuesto | Alto | Media | Cache agresivo, queries por zona (no por propiedad), monitoreo |
| 2 | Calidad irregular de datos gubernamentales | Medio | Alta | Validacion por fuente, indicadores de confianza, fechas de actualizacion |
| 3 | Rate limits en APIs externas | Medio | Media | Batch nocturno, colas con reintentos y backoff (enrichment_queue) |
| 4 | Deduplicacion cross-portal con falsos positivos/negativos | Medio | Media | Score de confianza, revision humana para casos ambiguos, multiples signals (coords, fotos, direccion) |
| 5 | H3 y AGEB con cobertura incompleta en zonas rurales | Bajo | Media | H3 siempre disponible (global), AGEB puede no cubrir zonas remotas — documentar gaps |

---

## Documentos del Modulo

- [Product Questions](./Product-Questions.md) — Cuestionario de discovery
- [Requirements](./Requirements.md) — Capacidades y criterios de aceptacion
- [Research/](./Research/) — Investigacion tecnica (INEGI, catastro, proveedores)
- [Database/](./Database/) — Base de datos Supabase (PostgreSQL + PostGIS + Storage)
- [Normalization/](./Normalization/) — Sub-modulo de normalizacion y deduplicacion
