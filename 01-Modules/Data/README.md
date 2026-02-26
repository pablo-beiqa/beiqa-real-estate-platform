# Data

**Fase del proyecto**: Fase 1 (Normalization) + Fase 2 (Ingestion)
**Estado**: 🟡 Parcialmente en diseno
**Owner**: Por definir

---

## Descripcion

El modulo de Data agrupa tres sub-modulos complementarios:

1. **Database (Supabase)**: Base de datos PostgreSQL + PostGIS + Storage que soporta toda la plataforma. Documenta la implementacion concreta en Supabase.

2. **Data Normalization** (Fase 1 — MVP): Pipeline de normalizacion profunda, deduplicacion cross-portal, y limpieza de datos que vienen del Scraper. Garantiza que los datos en Supabase sean consistentes, deduplicados y enriquecidos con H3/AGEB.

3. **Data Ingestion** (Fase 2 — Post-MVP): Integracion de fuentes de datos externos estructurados que enriquecen las propiedades con contexto: datos demograficos del INEGI, POIs de Google Maps, datos catastrales, e indicadores economicos.

---

## Sub-modulos

### Database (Supabase)

Base de datos PostgreSQL + PostGIS alojada en Supabase. Incluye tablas core (propiedades, clientes, zonas), Supabase Storage para imagenes, e indices geoespaciales.

Ver documentacion detallada en [Database/](./Database/)

### Data Normalization (Fase 1)

Pipeline de limpieza profunda que opera sobre datos ya almacenados en Supabase.

**Responsabilidades** (migradas desde el modulo Scraper):
- Deduplicacion cross-portal (misma propiedad en Inmuebles24, Pincali, CBRE, Colliers)
- Geocodificacion batch (propiedades sin coordenadas o con baja confianza)
- Validacion y correccion de coordenadas
- Normalizacion avanzada de direcciones
- Asignacion de H3 hexagonos y AGEB a propiedades
- Merge de registros duplicados preservando la mejor informacion

> **Nota**: La normalizacion basica (precios→MXN, superficies→m², tipos→catalogo) se hace dentro del pipeline del Scraper antes de guardar. Este sub-modulo hace la limpieza PROFUNDA post-ingesta.

Ver documentacion detallada en [Normalization/](./Normalization/)

### Data Ingestion (Fase 2)

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
| Pipeline de deduplicacion | Detectar y mergear misma propiedad publicada en multiples portales | Normalization | 🔴 |
| Asignacion H3 + AGEB | Asignar hexagono H3 y AGEB a cada propiedad para integracion GIS | Normalization | 🔴 |
| Geocodificacion batch | Geocodificar propiedades sin coordenadas o con baja confianza | Normalization | 🔴 |
| Conector INEGI (DENUE + Cartografia) | Pipeline para datos economicos y poligonos AGEB | Ingestion | 🔴 |
| Conector Google Maps/Places | POIs cercanos a cada propiedad | Ingestion | 🔴 |
| Conector datos catastrales | Valor catastral y uso de suelo | Ingestion | 🔴 |
| Conector indicadores economicos | Datos macro por region | Ingestion | 🔴 |
| Workflows de enriquecimiento | Enriquecimiento automatico al ingresar propiedad nueva | Ingestion | 🔴 |

---

## Dependencias

### Necesita (upstream)
- **Base de datos (Arquitectura)** → Esquema para propiedades, zonas, POIs, demograficos
- **Scraper** → Proporciona registros base de propiedades con ubicacion (lat/lng)
- **Supabase** → PostGIS habilitado para queries geoespaciales

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
| 3 | Rate limits en APIs externas | Medio | Media | Batch nocturno, colas con reintentos y backoff |
| 4 | Deduplicacion cross-portal con falsos positivos/negativos | Medio | Media | Score de confianza, revision humana para casos ambiguos, multiples signals (coords, fotos, direccion) |
| 5 | H3 y AGEB con cobertura incompleta en zonas rurales | Bajo | Media | H3 siempre disponible (global), AGEB puede no cubrir zonas remotas — documentar gaps |

---

## Documentos del Modulo

- [Product Questions](./Product-Questions.md) — Cuestionario de discovery
- [Requirements](./Requirements.md) — Capacidades y criterios de aceptacion
- [Research/](./Research/) — Investigacion tecnica (INEGI, catastro, proveedores)
- [Database/](./Database/) — Base de datos Supabase (PostgreSQL + PostGIS + Storage)
- [Normalization/](./Normalization/) — Sub-modulo de normalizacion y deduplicacion
