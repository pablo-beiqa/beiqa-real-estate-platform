---
status: "propuesto"
date: 2026-02-27
decision-makers: "Pablo (CEO), Pamela (Frontend), Fabrizio (Tech Lead)"
costo-mensual: "por definir"
---

# Plataforma GIS — Mapas Interactivos para Frontend

## Contexto y Problema

En Fase 2, la Internal App y el Tenant Portal necesitarán mapas interactivos para visualizar propiedades, zonas H3, polígonos AGEB, y análisis geoespacial. ¿Qué librería/plataforma de mapas usar en el frontend (Next.js)?

Esta decisión es independiente de Google Maps Platform ([ADR-011](ADR-011-Google-Maps-Platform.md)) que cubre APIs backend (Geocoding, Places). Aquí se trata del componente visual de mapas en el frontend.

## Factores de Decisión

* Necesidad de renderizar hexágonos H3 ([ADR-009](ADR-009-H3-Indexing.md)) y polígonos AGEB ([ADR-010](ADR-010-AGEB-INEGI.md))
* Volumen de markers: potencialmente miles de propiedades en un mapa
* Next.js como framework frontend ([ADR-015](ADR-015-Frontend-Next-js.md))
* Pamela es la desarrolladora principal del frontend
* Performance en mobile importa (brokers en campo)
* Presupuesto limitado — preferir opciones de bajo costo

## Opciones a Evaluar

### Leaflet + OpenStreetMap
* Open source, gratis, amplio ecosistema de plugins
* Pros: $0, maduro, gran comunidad, plugins para H3 y GeoJSON
* Contras: rendering de muchos markers puede ser lento sin optimización (clustering)

### Mapbox GL JS
* Rendering con WebGL, muy performante, mapas custom
* Pros: excelente performance, estilos personalizables, soporte nativo H3
* Contras: ~$50-500/mo según uso, vendor lock-in en tiles

### Google Maps JavaScript API
* Ya tenemos Google Maps Platform ([ADR-011](ADR-011-Google-Maps-Platform.md))
* Pros: integración natural con Geocoding/Places, $200 crédito mensual
* Contras: crédito puede no alcanzar si se agregan loads de mapas, menos flexible para custom layers

### Deck.gl (sobre Mapbox o Google)
* Capa de visualización WebGL de Uber (mismos creadores de H3)
* Pros: integración nativa con H3, excelente para grandes datasets, heatmaps
* Contras: curva de aprendizaje alta, requiere Mapbox o Google como base

### Atlas.co (API + Embed) — Preferido
* Plataforma de mapas con API y capacidad de embed
* Pros: API + embed simplifica integración en Next.js, diseñado para datos geoespaciales
* Contras: pricing pendiente de evaluación, menor ecosistema que Mapbox/Leaflet
* Nota: preferencia del equipo basada en capacidades de API + embed (decisión de entrevista 2026-03-02)

## Decisión

**Inclinación: Atlas.co.** Preferido por el equipo basado en API + embed capabilities. Pendiente evaluación técnica de pricing, rendimiento con miles de markers, y compatibilidad con capas H3/AGEB. Se validará cuando Pamela inicie el componente de mapas en Fase 2.

Estrategia progresiva:
1. **Fase básica**: Markers de propiedades del shortlist
2. **Fase contexto**: Capas de POIs, transporte, datos socioeconómicos
3. **Fase análisis**: Heatmaps de precio/m2, densidad de oferta

## Información Adicional

- Depende de: [ADR-015](ADR-015-Frontend-Next-js.md) (framework frontend)
- Datos a visualizar: [ADR-009](ADR-009-H3-Indexing.md) (H3), [ADR-010](ADR-010-AGEB-INEGI.md) (AGEB)
- APIs backend de geo: [ADR-011](ADR-011-Google-Maps-Platform.md)
- Condición para activar: cuando Pamela inicie componente de mapas en Fase 2
