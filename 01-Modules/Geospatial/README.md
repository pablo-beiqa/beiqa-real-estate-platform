# Geospatial

**Sprint del proyecto**: Sprint 2+ — Post-MVP
**Estado**: 🟡 En pruebas
**Owner**: Por definir

---

## Descripcion

El modulo Geospatial proporciona todas las capacidades de analisis y visualizacion geografica de la plataforma BEIQA. Incluye el servicio de geocodificacion que convierte direcciones en coordenadas, el componente de mapa interactivo para visualizar propiedades, el motor de analisis de zonas que calcula metricas por poligono geografico, y las herramientas de busqueda espacial que permiten encontrar propiedades dentro de un radio, poligono dibujado por el usuario o zona predefinida.

En el mercado inmobiliario comercial e industrial, la ubicacion es el factor mas determinante en la decision de un cliente. No basta con listar propiedades: el equipo de BEIQA necesita entender y comunicar el contexto geografico de cada opcion — que tan cerca esta de vias principales, zonas industriales consolidadas, centros de distribucion, nodos de transporte y servicios. Este modulo convierte datos de ubicacion en inteligencia espacial, permitiendo al equipo identificar patrones geograficos, comparar zonas y presentar opciones a clientes con mapas profesionales que respalden la recomendacion.

---

## Estado Actual

### Validado / Activo
- **H3 indexing**: Validado con h3-js (funcionando). Capas 5-11 disponibles para diferentes niveles de agregacion.
- **Atlas.co**: Activo para visualizacion de mapas. 2-3 usuarios del equipo utilizandolo actualmente.
- **Google Maps Geocoding**: Activo como Mastra tool, utilizado por el Address Enrichment Agent para geocodificacion de propiedades.

### Planificado
- **AGEB/INEGI shapefiles import**: Importacion de poligonos AGEB y limites municipales del INEGI (Sprint 3).
- **GIS Analysis Agent** (Mastra): Agente dedicado al analisis geoespacial avanzado — calculo de metricas por zona, deteccion de clusters, analisis de proximidad (Sprint 4+).

---

## Objetivos

1. Habilitar busqueda de propiedades basada en ubicacion (por radio, poligono o zona) con resultados en menos de 2 segundos.
2. Proveer analisis de zona automatizado que consolide demografia, POIs, conectividad y oferta inmobiliaria por cada area geografica definida.
3. Ofrecer visualizaciones de mapa interactivas con capas de informacion (propiedades, zonas, POIs, heatmaps de precios) para uso interno y presentaciones a clientes.

---

## Metricas de Exito / KPIs

| Metrica | Target | Como se mide |
|---------|--------|--------------|
| Precision de geocodificacion | >=95% de direcciones geocodificadas correctamente (a nivel de calle) | Muestreo aleatorio de 100 propiedades verificadas manualmente cada mes |
| Tiempo de carga de mapa | <3 segundos para renderizar 1,000 propiedades con clusters | Medicion de performance en navegador (Lighthouse, metricas de usuario real) |
| Cobertura de zonas definidas | >=20 zonas metropolitanas y corredores industriales con poligonos | Conteo de zonas con poligono definido y datos asociados |
| Uso del componente de mapa | >=80% de sesiones de usuario incluyen interaccion con mapa | Analytics de uso del componente de mapa en la aplicacion |
| Busquedas espaciales por dia | >=50 busquedas espaciales diarias por el equipo | Logs de queries espaciales en la API |

---

## Entregables Clave

| Entregable | Descripcion | Estado |
|-----------|-------------|--------|
| H3 indexing | Asignacion de hexagonos H3 a propiedades (h3-js validado) | 🟡 Validado |
| Atlas.co visualizacion | Mapas interactivos para el equipo (2-3 usuarios activos) | ✅ Activo |
| Google Maps Geocoding (Mastra tool) | Geocodificacion de direcciones como tool del Address Enrichment Agent | ✅ Activo |
| AGEB/INEGI shapefiles import | Importacion de poligonos AGEB y limites municipales del INEGI | 🔴 Sprint 3 |
| GIS Analysis Agent (Mastra) | Agente de analisis geoespacial: metricas por zona, clusters, proximidad | 🔴 Sprint 4+ |
| Componente de mapa interactivo | Mapa basado en Mapbox/Google Maps con clusters, filtros, capas y controles de visualizacion | 🔴 |
| Motor de analisis de zona | Servicio que calcula metricas agregadas (oferta, precio promedio, POIs) dentro de un poligono geografico | 🔴 |
| Busqueda por poligono | Funcionalidad para dibujar un area en el mapa y obtener propiedades dentro de ese poligono (PostGIS) | 🔴 |
| Capas de visualizacion | Heatmaps de precio, capas de zonas industriales, rutas de transporte, y POIs como overlays en el mapa | 🔴 |

---

## Stack Tecnologico

| Herramienta | Uso | Estado |
|-------------|-----|--------|
| **H3** (h3-js) | Indexacion hexagonal para agregacion geoespacial (capas 5-11) | 🟡 Validado |
| **Atlas.co** | Visualizacion de mapas para el equipo | ✅ Activo (2-3 usuarios) |
| **Google Maps Geocoding API** | Geocodificacion de direcciones (Mastra tool del Address Enrichment Agent) | ✅ Activo |
| **PostGIS** | Queries geoespaciales, indices espaciales (GiST) | ✅ Habilitado en Supabase |
| **INEGI Cartografia** | Shapefiles de AGEB, limites municipales | 🔴 Sprint 3 |
| **Mastra** | GIS Analysis Agent para analisis geoespacial avanzado | 🔴 Sprint 4+ |

---

## Dependencias

### Necesita (upstream)
- **Base de datos con PostGIS** → Extension espacial de PostgreSQL para almacenar geometrias, ejecutar queries espaciales y manejar indices geograficos
- **Data (zonas INEGI)** → Poligonos de AGEBs, municipios y zonas metropolitanas del INEGI para definicion de areas de analisis
- **Mastra** → Framework de agentes para GIS Analysis Agent y Address Enrichment Agent (geocodificacion)

### Depende de este (downstream)
- **Internal App** → Integra el componente de mapa y las busquedas espaciales en la interfaz del equipo
- **Market Intelligence** → Usa zonas y poligonos como unidad de agregacion para reportes de mercado
- **AI Brain** → Utiliza proximidad y contexto geografico como features para el modelo de matching

---

## Riesgos Clave

| # | Riesgo | Impacto | Probabilidad | Mitigacion |
|---|--------|---------|--------------|------------|
| 1 | Costos de API de Google Maps/Mapbox que escalen con el uso y superen presupuesto | Alto | Media | Evaluar Mapbox vs Google Maps por costo, implementar cache de tiles y geocodificacion, limitar llamadas innecesarias |
| 2 | Complejidad tecnica de PostGIS para el equipo de desarrollo | Medio | Media | Invertir en capacitacion, crear abstracciones (ORM/funciones) que simplifiquen queries espaciales comunes |
| 3 | Calidad de datos de zonas gubernamentales (poligonos desactualizados, limites imprecisos) | Medio | Media | Permitir edicion manual de poligonos, usar multiples fuentes para validacion cruzada |
| 4 | Performance de queries espaciales con volumen alto de datos | Medio | Baja | Indices espaciales (GiST) correctamente configurados, particionamiento por zona metropolitana |
| 5 | Rendering lento del mapa con miles de propiedades simultaneas | Medio | Media | Implementar clustering del lado del servidor, lazy loading de marcadores, y simplificacion de geometrias por nivel de zoom |

---

## Documentos del Modulo

- [Product Questions](./Product-Questions.md) — Cuestionario de discovery
- [Requirements](./Requirements.md) — Capacidades y criterios de aceptacion
- [Research/](./Research/) — Investigacion tecnica
