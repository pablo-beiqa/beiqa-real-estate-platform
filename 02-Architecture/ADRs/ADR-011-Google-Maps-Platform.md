---
status: "aceptado"
date: 2026-02-25
decision-makers: "Pablo (CEO)"
consulted: "Fabrizio (Tech Lead)"
informed: "Pamela, Jerónimo"
costo-mensual: "~$0 (dentro del crédito de $200/mo)"
---

# Google Maps Platform — APIs de Geocodificación y Ubicación

## Contexto y Problema

BEIQA necesita geocodificar direcciones de propiedades (convertir texto a coordenadas), enriquecer datos de ubicación, y eventualmente mostrar mapas interactivos. ¿Qué proveedor de APIs de ubicación usamos?

## Factores de Decisión

* Google Maps Platform ofrece crédito de $200/mo (cubre uso actual)
* Geocoding API ya en uso via Clay (validada con datos mexicanos)
* Mejor cobertura de direcciones en México comparado con alternativas
* Múltiples APIs en una sola plataforma (Geocoding, Places, Maps JS)

## Decisión

**Google Maps Platform** como proveedor de APIs de ubicación. APIs confirmadas en uso/planificadas:

| API | Estado | Uso |
|-----|--------|-----|
| Geocoding API | En uso (via Clay → migrando a Trigger.dev) | Convertir direcciones a coordenadas |
| Places API | En uso | Enriquecer datos de ubicación |
| Maps JavaScript API | Planificada (Fase 2) | Mapas interactivos en frontend |

El catálogo completo de Google Maps Platform se está explorando — APIs adicionales pueden adoptarse según necesidad.

### Consecuencias

* Bien, porque crédito $200/mo cubre el volumen actual ($0 efectivo)
* Bien, porque mejor cobertura de geocodificación en México
* Bien, porque una sola plataforma para múltiples necesidades de ubicación
* Mal, porque vendor lock-in en APIs de Google
* Neutral, porque cuando Clay salga, la geocodificación se replica en Trigger.dev

## Plan de Implementación

* **Sistemas afectados**: Trigger.dev (llamadas a API post-scraping), Supabase (cache de respuestas), futuro frontend (Maps JS)
* **Dependencias**: Google Maps Platform API key, tabla `cache_api_responses` en Supabase (planificada)
* **Patrones a seguir**: Cachear respuestas de geocoding para no repetir llamadas. Geocodificar al momento de ingestar datos (no on-the-fly).
* **Patrones a evitar**: No llamar a Google API sin cache. No exceder crédito de $200/mo sin justificación.
* **Configuración**: `GOOGLE_MAPS_API_KEY` en Trigger.dev

### Verificación

- [x] Google Maps Platform activo con crédito $200/mo
- [x] Geocoding API validada con direcciones mexicanas
- [x] Places API en uso para enriquecimiento
- [ ] Cache de respuestas implementado en Supabase
- [ ] Geocodificación migrada de Clay a Trigger.dev

## Alternativas Consideradas

* **HERE Maps**: Buena alternativa pero menor cobertura en México.
* **Geocodio**: Solo USA/Canadá. No aplica para México.
* **OpenStreetMap/Nominatim**: Gratuito pero cobertura inconsistente en México, especialmente zonas industriales.

## Información Adicional

- Investigación detallada: [Geospatial/Research/Google-Maps-Platform.md](../../01-Modules/Geospatial/Research/Google-Maps-Platform.md)
- La decisión de **plataforma GIS** (frontend de mapas interactivos) es separada → [ADR-017](ADR-017-Plataforma-GIS.md)
- Relacionado con: [ADR-009](ADR-009-H3-Indexing.md), [ADR-010](ADR-010-AGEB-INEGI.md)
