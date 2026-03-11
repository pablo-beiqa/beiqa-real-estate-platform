# Estrategia de Análisis GIS — AI Brain

**Status**: 🟡 En diseño
**Updated**: 2026-03-11

---

## Visión

El GIS Analysis Agent realiza análisis geoespacial avanzado más allá del H3/AGEB básico (que se manejan con triggers de Supabase). Se conecta a 10+ fuentes de datos para generar un composite score de calidad de zona diferenciado por tipo de propiedad.

> **NOTA**: H3 (res 5-11) y la asignación de AGEB se calculan automáticamente por triggers de Supabase en INSERT/UPDATE. El GIS Agent maneja análisis AVANZADO, no indexación básica.

---

## Fuentes de Datos (10+)

| Fuente | Tipo | Datos | API/Acceso | Estado | Prioridad |
|--------|------|-------|------------|--------|-----------|
| INEGI DENUE | Gobierno | Establecimientos económicos, actividad comercial | API pública | 🟡 Disponible | Alta |
| INEGI AGEB | Gobierno | Datos censales por área geoestadística | Shapefiles / API | 🟡 Disponible | Alta |
| INEGI Socioeconómico | Gobierno | NSE, población, ingreso, educación | API / descarga | 🟡 Disponible | Alta |
| Google Places | Comercial | Negocios cercanos, POIs, ratings | API (pagada) | ✅ Activo | Alta |
| Google Distance Matrix | Comercial | Tiempos de traslado, rutas | API (pagada) | ✅ Activo | Alta |
| ArcGIS | Comercial | Análisis espacial avanzado, geoprocesamiento | MCP server | 🟡 Por integrar | Alta |
| Catastro | Gobierno | Uso de suelo, zonificación, valor catastral | API / manual | 🔴 Por investigar | Media |
| Foot traffic (Placer.ai/SafeGraph/etc.) | Comercial | Tráfico peatonal, afluencia por hora/día | TBD | 🔴 Por investigar | Media |
| D9 | Varios | Datos complementarios | TBD | 🔴 Por investigar | Baja |
| Heat maps | Derivado | Densidad de actividad | Calculado de fuentes anteriores | 🔴 Por diseñar | Baja |

> **NOTA**: Las fuentes se agregan incrementalmente. Prioridad: INEGI + Google + ArcGIS primero.

---

## Análisis por Tipo de Propiedad

### Industrial / Bodega / Logística

**Factores clave**: acceso logístico, calidad de infraestructura

Puntos de análisis:
- Distancia a autopistas principales (México-Querétaro, México-Puebla, etc.)
- Distancia a aeropuertos (AICM, Santa Lucía, Toluca)
- Distancia a puertos/aduanas
- Presencia en zona/corredor industrial reconocido
- Acceso ferroviario cercano
- Infraestructura eléctrica de la zona
- Actividad industrial vecina (DENUE)

### Comercial / Restaurante / Retail

**Factores clave**: demanda, competencia, accesibilidad

Puntos de análisis:
- Tráfico peatonal estimado (foot traffic API o proxy)
- NSE predominante en radio de 2km
- Densidad de competencia directa (DENUE por tipo)
- Densidad de negocios complementarios
- Flujo vehicular y visibilidad
- Estacionamiento público cercano
- Zonas de alta densidad residencial/oficinas

### Oficinas / Coworking

**Factores clave**: conectividad, transporte, contexto de clase de edificio

Puntos de análisis:
- Distancia a metro (minutos caminando + líneas cercanas)
- Rutas de transporte público
- Amenidades en radio de 500m (restaurantes, bancos, gym)
- Clasificación del área (zona financiera, corporativa, mixta)
- Acceso vehicular y estacionamiento público

### Terreno

**Factores clave**: potencial de desarrollo, regulación

Puntos de análisis:
- Uso de suelo actual y potencial (Catastro)
- CUS/COS permitido
- Servicios disponibles (agua, luz, drenaje, telecomunicaciones)
- Acceso carretero
- Valor catastral de referencia
- Desarrollos en construcción cercanos

---

## Zone Quality Composite Score

### Concepto de la fórmula

Componentes base (cada uno 0-100):
- **accessibility** — accesibilidad vial, transporte, conectividad
- **economic_activity** — actividad económica de la zona (DENUE, foot traffic)
- **infrastructure** — calidad de infraestructura (servicios, electricidad, telecomunicaciones)
- **competition_context** — contexto competitivo (densidad de competencia, complementarios)

### Pesos por tipo de propiedad

| Componente | Industrial | Comercial | Oficinas | Terreno |
|-----------|-----------|-----------|----------|---------|
| accessibility | 40% | 25% | 40% | 30% |
| economic_activity | 20% | 35% | 30% | 25% |
| infrastructure | 30% | 10% | 20% | 35% |
| competition_context | 10% | 30% | 10% | 10% |

> **NOTA**: Los pesos son propuestas iniciales — necesitan calibración con el equipo contra evaluaciones reales.

---

## Output Schema (propuesto)

```json
{
  "property_id": "uuid",
  "analysis_date": "2026-03-11T00:00:00Z",
  "property_type": "industrial",
  "location": {
    "lat": 19.4326,
    "lng": -99.1332,
    "h3_res7": "87309ea0fffffff",
    "ageb_id": "0901200010345"
  },
  "components": {
    "accessibility": {
      "score": 82,
      "details": {
        "nearest_highway_km": 1.2,
        "nearest_airport_km": 15.4,
        "nearest_rail_km": 3.1,
        "industrial_corridor": true,
        "corridor_name": "Corredor Cuautitlán-Texcoco"
      }
    },
    "economic_activity": {
      "score": 71,
      "details": {
        "denue_establishments_1km": 234,
        "industrial_neighbors": 45,
        "dominant_activity": "Manufactura"
      }
    },
    "infrastructure": {
      "score": 88,
      "details": {
        "electrical_capacity": "alta",
        "water_availability": true,
        "telecom_fiber": true
      }
    },
    "competition_context": {
      "score": 65,
      "details": {
        "similar_properties_5km": 12,
        "avg_vacancy_rate_zone": 0.08
      }
    }
  },
  "composite_score": 80.3,
  "weights_used": {
    "accessibility": 0.40,
    "economic_activity": 0.20,
    "infrastructure": 0.30,
    "competition_context": 0.10
  },
  "sources_used": ["DENUE", "Google Places", "Google Distance Matrix", "ArcGIS"],
  "confidence": 0.85,
  "notes": "Sin datos de foot traffic disponibles para zona industrial"
}
```

---

## Integración con ArcGIS vía MCP

- El ArcGIS MCP server provee: geocoding, routing, análisis espacial, buffer zones, heat maps
- El GIS Agent actúa como MCP client vía Mastra
- Casos de uso principales:
  - **Análisis de isócronas** (radio de 15 min en auto)
  - **Overlay analysis** (cruzar capas: uso de suelo + DENUE + demographics)
  - **Demographic profiling** (perfil socioeconómico por zona de influencia)

---

## Preguntas Abiertas

- Proveedor exacto de foot traffic? (Placer.ai, SafeGraph, alternativa local)
- Método de acceso a datos de Catastro? (API, web scraping, descarga manual, open data)
- D9 — qué es exactamente esta fuente? Necesita clarificación de Pablo
- ArcGIS MCP server — qué implementación comunitaria usar? (gis-mcp en GitHub?)
- Pesos del zone quality — necesitan dataset de calibración
