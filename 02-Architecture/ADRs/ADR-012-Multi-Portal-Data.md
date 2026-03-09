---
status: "aceptado"
date: 2026-02-27
decision-makers: "Fabrizio (Tech Lead), Pablo (CEO)"
consulted: "Alex (Advisor)"
informed: "Jerónimo"
costo-mensual: "$0"
---

# Estrategia Multi-Portal — Staging Tables + Golden Record

## Contexto y Problema

BEIQA scrapeará 4 portales inmobiliarios (Inmuebles24, Pincali, CBRE, Colliers), cada uno con estructura de datos diferente. La misma propiedad puede aparecer en múltiples portales. ¿Cómo estructuramos los datos en Supabase para manejar multi-portal, preservar datos originales, y permitir deduplicación?

## Factores de Decisión

* 4 portales con schemas diferentes (campos, tipos, calidad de datos distintos)
* Propiedades pueden aparecer en múltiples portales simultáneamente
* Necesidad de rastrear origen ("esta propiedad viene de 3 portales")
* El frontend y el equipo deben consultar datos limpios y deduplicados
* La deduplicación es un proceso iterativo que mejora con el tiempo

## Opciones Consideradas

* Schema unificado desde el inicio (una tabla para todos los portales)
* Staging tables por portal solamente (sin golden record)
* Staging tables por portal + golden record `properties`

## Decisión

Opción elegida: "**Staging tables por portal + golden record `properties`**", porque preserva fidelidad de datos originales, permite deduplicación cross-portal, y ofrece una tabla limpia como source of truth para consultas.

**Arquitectura de datos:**

```
Portal Sources (staging)          Golden Record
┌─────────────────────┐          ┌──────────────┐
│ inmuebles24_listings │ ───┐    │              │
│ pincali_listings     │ ───┼──→ │  properties  │
│ cbre_listings        │ ───┤    │  (deduplicated│
│ colliers_listings    │ ───┘    │   clean data) │
└─────────────────────┘          └──────────────┘
         ↕                              ↑
  possible_duplicates ──────────────────┘
```

**Staging tables**: Una tabla por portal con schema propio que refleja los datos crudos.
**Golden record** (`properties`): Tabla unificada, normalizada, deduplicada. Source of truth para el frontend y analytics.
**`possible_duplicates`**: Tabla de enlaces entre staging y golden record (ya existe en schema actual).

### Consecuencias

* Bien, porque datos originales se preservan intactos en staging tables
* Bien, porque se puede rastrear "esta propiedad viene de portales X, Y, Z"
* Bien, porque la deduplicación opera entre staging tables para alimentar el golden record
* Bien, porque el frontend solo consulta `properties`, nunca las tablas raw
* Mal, porque más tablas y más complejidad en el schema
* Mal, porque requiere pipeline de normalización y deduplicación activo
* Neutral, porque la tabla actual `inmuebles24_listings` se convierte en la primera staging table

## Plan de Implementación

* **Sistemas afectados**: Supabase (nuevas tablas), Trigger.dev (pipeline de normalización), `beiqa-scraper` (escritura a staging tables)
* **Dependencias**: Migrations de Supabase para crear tablas nuevas
* **Patrones a seguir**: Cada scraper escribe a SU staging table. Un pipeline separado de normalización alimenta el golden record. Mapeo de campos de cada portal documentado en Data-Model.md.
* **Patrones a evitar**: No escribir directamente al golden record desde scrapers. No forzar normalización prematura en staging tables.
* **Migración**: (1) `inmuebles24_listings` ya existe como staging, (2) Crear `pincali_listings`, `cbre_listings`, `colliers_listings`, (3) Crear tabla `properties` (golden record), (4) Pipeline de normalización en Trigger.dev, (5) Mapear `possible_duplicates` al golden record

### Verificación

- [x] `inmuebles24_listings` existente (~24,700 rows) — primera staging table
- [x] `possible_duplicates` existente (mecanismo de dedup)
- [x] `pincali_listings` creada (1,761 rows, 58 columnas, H3 res5-11, 4 triggers, bucket pincali-images)
- [x] `cbre_listings` creada (203 rows, schema completo, 4 triggers, buckets cbre-images + cbre-pdfs)
- [x] `colliers_listings` creada (240 rows, schema completo, 4 triggers, buckets colliers-images + colliers-pdfs)
- [x] `finsa_listings` creada (713 rows, schema completo, 5 triggers, bucket finsa-flyers)
- [ ] Tabla `properties` (golden record) creada
- [ ] Pipeline de normalización staging → golden record (Mastra agent)

## Información Adicional

- Schema actual: [Database/Schema-Real.md](../Database/Schema-Real.md)
- Modelo target: [Database/Data-Model.md](../Database/Data-Model.md) (patrón PROPERTY + PROPERTY_SOURCE)
- Estrategia de dedup: [ADR-016](ADR-016-Deduplicacion.md) (propuesto, por definir)
- Relacionado con: [ADR-001](ADR-001-Supabase-Plataforma.md) (Supabase como DB), [ADR-002](ADR-002-Estrategia-Scraping.md) (4 portales)
