---
status: "aceptado"
date: 2026-02-27
decision-makers: "Fabrizio (Tech Lead), Pablo (CEO)"
consulted: "Alex (Advisor)"
informed: "Jerónimo"
costo-mensual: "$0 (open source)"
---

# H3 Indexing — Sistema Hexagonal Geoespacial

## Contexto y Problema

BEIQA necesita indexar propiedades geoespacialmente para: búsqueda por zona, análisis de mercado por área, agregación de métricas (precio promedio m2, tendencias). ¿Qué sistema de indexación geoespacial usamos para particionar el territorio en zonas?

## Factores de Decisión

* Necesidad de agregación por zona (precio promedio, tendencias)
* Compatible con PostGIS (ya activo en Supabase)
* Resoluciones múltiples para diferentes casos de uso (ciudad → manzana)
* Open source y gratuito
* Amplia adopción en la industria (Uber, Foursquare, etc.)

## Opciones Consideradas

* H3 de Uber (hexágonos)
* S2 de Google (cuadriláteros esféricos)
* Geohash (cuadriláteros planos)
* Polígonos custom dibujados por el equipo

## Decisión

Opción elegida: "**H3 de Uber**", porque provee un sistema hexagonal con resoluciones jerárquicas (capas 5 a 11), amplia adopción, y la librería `h3-js` es el binding oficial de TypeScript.

**Resoluciones seleccionadas (capas 5 a 11):**

| Capa | Área aprox. | Uso |
|------|------------|-----|
| 5 | ~252 km² | Región metropolitana |
| 7 | ~5.16 km² | Zona/corredor industrial |
| 9 | ~0.105 km² | Colonia/vecindario |
| 11 | ~0.002 km² | Manzana/predio |

### Consecuencias

* Bien, porque hexágonos tienen distancia uniforme entre centros (mejor que cuadriláteros)
* Bien, porque resoluciones jerárquicas permiten zoom in/out
* Bien, porque `h3-js` es TypeScript nativo — compatible con Trigger.dev
* Bien, porque $0 (open source, MIT license)
* Neutral, porque requiere precalcular índices H3 para cada propiedad

## Plan de Implementación

* **Sistemas afectados**: Supabase (columnas H3 en tablas de listings), `beiqa-scraper` (cálculo de H3 post-scrape), Trigger.dev (batch processing)
* **Dependencias**: `h3-js` (binding oficial de JavaScript/TypeScript para H3)
* **Patrones a seguir**: Calcular H3 a múltiples resoluciones al momento de persistir coordenadas. Almacenar como TEXT en columnas `h3_res5` a `h3_res11`.
* **Patrones a evitar**: No calcular H3 on-the-fly en queries (precalcular al ingestar). No usar solo una resolución.
* **Configuración**: Columnas H3 en migrations de Supabase (pendiente)
* **Migración**: (1) Crear columnas H3 en Supabase, (2) Batch process de propiedades existentes (~60K), (3) Integrar cálculo en pipeline de scraping

### Verificación

- [x] Librería h3-js identificada y validada por Fabrizio
- [x] Pruebas de cálculo H3 en ambiente de test
- [ ] Columnas H3 creadas en schema de Supabase (migration)
- [ ] Batch processing de propiedades existentes
- [ ] Pipeline de scraping calcula H3 al persistir

## Información Adicional

- h3-js: `https://github.com/uber/h3-js`
- Recomendación de Alex (23 feb 2026): precalcular H3 al momento del scraping
- Resoluciones 7 y 9 son las más usadas para análisis de mercado inmobiliario
- Relacionado con: [ADR-001](ADR-001-Supabase-Plataforma.md) (PostGIS), [ADR-010](ADR-010-AGEB-INEGI.md) (AGEB complementa H3)
