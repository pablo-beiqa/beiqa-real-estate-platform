# Data Normalization

**Fase del proyecto**: Fase 1 — MVP
**Estado**: 🟡 Parcial — triggers automatizan normalización básica (geo, price_per_m2, price_history, last_seen) en las 5 tablas de staging. Pipeline Mastra de normalización profunda (golden record, dedup) por iniciar.
**Owner**: Pablo (Mastra agents), Fabrizio (DB/triggers)

---

## Descripcion

Sub-modulo de limpieza profunda y normalizacion avanzada de datos que opera sobre propiedades ya almacenadas en Supabase. Mientras el Scraper hace normalizacion basica antes de guardar (precios→MXN, superficies→m², tipos→catalogo), este modulo se encarga de:

1. **Deduplicacion cross-portal**: Detectar la misma propiedad publicada en multiples portales (Inmuebles24, Pincali, CBRE, Colliers) y consolidar en un solo registro maestro
2. **Geocodificacion batch**: Re-geocodificar propiedades con baja confianza o sin coordenadas
3. **Asignacion geoespacial**: Calcular y asignar H3 hexagonos y AGEB a cada propiedad
4. **Validacion de datos**: Correccion de coordenadas, normalizacion avanzada de direcciones

---

## Pipeline de Normalizacion

```
1. Deteccion de duplicados  → Comparar propiedades entre portales por:
                               - Proximidad geografica (PostGIS ST_DWithin)
                               - Similitud de direccion (text similarity)
                               - Superficie similar (±10%)
                               - Similitud de fotos (pHash, si implementado)
                               - Score de confianza del match

2. Merge de registros       → Para pares con score > umbral:
                               - Crear registro maestro (golden record)
                               - Preservar la mejor informacion de cada fuente
                               - Mantener referencia a todas las fuentes originales
                               - Score 0.70-0.95: revision humana o LLM
                               - Score > 0.95: merge automatico

3. Geocodificacion batch    → Re-procesar propiedades con:
                               - Sin coordenadas
                               - Confianza de geocoding baja
                               - Coordenadas flaggeadas como anomalia

4. Asignacion H3 + AGEB    → Para cada propiedad con coordenadas validas:
                               - Calcular H3 hexagono (resolucion por definir)
                               - Buscar AGEB correspondiente via PostGIS (ST_Contains)
                               - Almacenar ambos IDs en la propiedad

5. Normalizacion avanzada   → Limpieza adicional de:
                               - Direcciones (formato estandar)
                               - Nombres de colonias/zonas
                               - Clasificacion de tipo de propiedad
```

---

## Modelo de Deduplicacion

> Basado en la estrategia documentada en [Deduplication-Strategy.md](../../../02-Architecture/Deduplication-Strategy.md)

```
PROPERTY (registro maestro / golden record)
├── id, canonical_address, canonical_coordinates
├── canonical_price, canonical_surface, confidence_score
└── last_merged_at

PROPERTY_SOURCE (tracking de fuentes - 1:N con PROPERTY)
├── id, property_id (FK), portal_name, portal_listing_id
├── original_address, original_price, original_surface
├── source_url, agent_name, captured_at, last_seen_at
└── raw_data (JSON de campos originales)
```

---

## Dependencias

### Necesita (upstream)
- **Scraper** → Proporciona propiedades con normalizacion basica en Supabase
- **Supabase (PostGIS)** → Indices espaciales para queries de proximidad
- **INEGI Cartografia** → Poligonos AGEB cargados en PostGIS

### Depende de este (downstream)
- **Internal App** → Muestra propiedades deduplicadas y normalizadas
- **AI Brain** → Usa datos limpios para matching
- **Market Intelligence** → Datos consistentes para analisis de mercado
- **Geospatial** → H3 y AGEB para visualizacion en mapa

---

## Documentos Relacionados

- [Deduplication-Strategy.md](../../../02-Architecture/Deduplication-Strategy.md) — Algoritmos y reglas de deduplicacion
- [Data-Model.md](../../../02-Architecture/Database/Data-Model.md) — Esquema de base de datos
- [Scraper README](../../Scraper/README.md) — Pipeline de normalizacion basica del scraper
