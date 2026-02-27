---
status: "aceptado"
date: 2026-02-27
decision-makers: "Fabrizio (Tech Lead), Pablo (CEO)"
informed: "Jerónimo"
costo-mensual: "$0 (datos públicos)"
---

# AGEB/INEGI — Polígonos Geoespaciales para Análisis Territorial

## Contexto y Problema

Para análisis de mercado inmobiliario en México, se necesita una división territorial oficial que permita cruzar datos de propiedades con datos socioeconómicos del INEGI (DENUE, censos, etc.). ¿Qué sistema de división territorial usamos?

## Decisión

**AGEBs (Áreas Geoestadísticas Básicas) del INEGI** como sistema de polígonos territoriales. Los AGEBs son la unidad geográfica mínima del INEGI y permiten cruzar datos inmobiliarios con datos socioeconómicos oficiales.

### Consecuencias

* Bien, porque datos gratuitos y oficiales del gobierno mexicano
* Bien, porque permiten cruce con DENUE (comercios), censos, y otras fuentes INEGI
* Bien, porque complementan H3 (H3 = indexación hexagonal uniforme, AGEB = división territorial oficial)
* Mal, porque shapefiles de INEGI requieren carga y mantenimiento en PostGIS
* Neutral, porque los polígonos AGEB no son uniformes en tamaño (a diferencia de H3)

## Plan de Implementación

* **Sistemas afectados**: Supabase/PostGIS (tabla de AGEBs + spatial join), Trigger.dev (batch asignación)
* **Dependencias**: Shapefiles AGEB del INEGI (descarga pública), PostGIS para spatial operations
* **Patrones a seguir**: Cargar shapefiles en tabla PostGIS. Asignar AGEB a cada propiedad via spatial join (ST_Contains).
* **Patrones a evitar**: No calcular AGEB on-the-fly en queries (precalcular al ingestar, similar a H3)
* **Migración**: (1) Descargar shapefiles AGEB de INEGI, (2) Cargar en tabla PostGIS, (3) Crear columna `ageb_id` en tablas de listings, (4) Batch process de propiedades existentes

### Verificación

- [ ] Shapefiles AGEB descargados de INEGI
- [ ] Tabla de AGEBs cargada en PostGIS
- [ ] Columna `ageb_id` en tablas de listings
- [ ] Spatial join funcional (propiedad → AGEB)
- [ ] Batch processing de propiedades existentes

## Alternativas Consideradas

* **Solo H3**: H3 no tiene cruce directo con datos INEGI. Los dos se complementan.
* **Colonias/municipios**: Menos granulares y menos estandarizados que AGEBs.
* **Códigos postales**: Cobertura inconsistente en zonas industriales.

## Información Adicional

- Fuente: [INEGI Marco Geoestadístico](https://www.inegi.org.mx/temas/mg/)
- Investigación pendiente: [Data/Research/INEGI-APIs.md](../../01-Modules/Data/Research/INEGI-APIs.md)
- Relacionado con: [ADR-009](ADR-009-H3-Indexing.md) (H3 complementa AGEB), [ADR-001](ADR-001-Supabase-Plataforma.md) (PostGIS)
