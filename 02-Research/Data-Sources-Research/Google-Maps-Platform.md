# Estrategia GIS y Geoespacial

**Fase**: R-2 (Tecnología y Plataforma)
**Estado**: 🔴 Por investigar
**Depende de**: Cuestionario 05-Geoespacial-y-GIS (R-0)

> **Nota**: Este documento se expandió de una evaluación de Google Maps a una **estrategia GIS completa**, cubriendo todas las opciones de plataforma geoespacial para BEIQA.

---

## Objetivo

Definir la estrategia completa de GIS para BEIQA: qué plataforma de mapas usar, cómo almacenar y consultar datos geoespaciales, cómo renderizar mapas, y cómo integrar datos de gobierno y fuentes externas.

---

## Componentes de la Estrategia GIS

### 1. Almacenamiento Geoespacial (Backend)

**Decisión tomada: PostGIS** (extensión de PostgreSQL)

| Capacidad | PostGIS |
|-----------|---------|
| Almacenar puntos (propiedades) | ✅ POINT geometry |
| Almacenar polígonos (zonas) | ✅ POLYGON, MULTIPOLYGON |
| Búsqueda por radio | ✅ ST_DWithin |
| Búsqueda dentro de polígono | ✅ ST_Within, ST_Intersects |
| Cálculo de distancias | ✅ ST_Distance |
| Overlay de capas | ✅ ST_Intersection, ST_Union |
| Importar shapefiles | ✅ shp2pgsql o ogr2ogr |
| Importar GeoJSON | ✅ Nativo |
| Importar KML | ✅ Via ogr2ogr |
| Índices espaciales | ✅ GiST index |

**Preguntas por investigar:**
- [ ] ¿Cuáles funciones PostGIS necesitamos para el MVP?
- [ ] ¿Qué SRID usar? (4326 WGS84 estándar, o EPSG:6372 para México)

---

### 2. Plataforma de Mapas (Frontend Rendering)

| Opción | Tipo | Costo | Calidad México | Customización | Offline | Estado |
|--------|------|-------|----------------|---------------|---------|--------|
| **Google Maps Platform** | Comercial | Pay per use ($200 crédito/mes) | ✅ Excelente | ⚠️ Limitada | ❌ | 🔴 |
| **Mapbox** | Comercial | Free tier generoso, luego pay per use | ✅ Buena | ✅ Alta (Studio) | ✅ | 🔴 |
| **Leaflet + OSM** | Open source | Gratis | ✅ Buena (OSM) | ✅ Alta | ⚠️ | 🔴 |
| **OpenLayers** | Open source | Gratis | ✅ Buena | ✅ Alta | ⚠️ | 🔴 |
| **deck.gl** | Open source | Gratis | ✅ (usa tiles externos) | ✅ Muy alta | ❌ | 🔴 |

#### Google Maps Platform -- Detalle

| API | Uso en BEIQA | Precio | Free tier |
|-----|-------------|--------|-----------|
| Maps JavaScript API | Mapa interactivo | $7/1000 cargas | $200/mes crédito |
| Places API | Buscar POIs | $17/1000 requests | Incluido en crédito |
| Geocoding API | Dirección → coordenadas | $5/1000 requests | Incluido |
| Routes API | Cálculo de rutas | $5-10/1000 requests | Incluido |
| Distance Matrix API | Matrices de distancia | $5/1000 elementos | Incluido |

**Estimación para MVP (6-15 usuarios, ~500 cargas/mes):**
- Maps: ~$3.50/mes
- Geocoding: ~$2.50/mes (500 geocodes)
- Places: ~$8.50/mes (500 POI lookups)
- **Total: ~$15/mes → cubierto por crédito gratuito de $200/mes**

#### Mapbox -- Detalle

| Feature | Disponibilidad | Precio |
|---------|---------------|--------|
| Map rendering | ✅ | 50K cargas gratis/mes, luego $5/1000 |
| Geocoding | ✅ | 100K requests gratis/mes |
| Isochrones | ✅ | Incluido |
| Navigation | ✅ | Incluido |
| Studio (custom styles) | ✅ | Gratis |
| Customización visual | ✅ Mucho mejor que Google | |

#### Leaflet + OpenStreetMap -- Detalle

| Feature | Disponibilidad | Notas |
|---------|---------------|-------|
| Map rendering | ✅ | Tiles gratis (OSM, Stamen, CartoDB) |
| Geocoding | Via Nominatim (gratis) | Menor precisión que Google |
| Clustering | Via plugins | Leaflet.markercluster |
| Dibujar polígonos | Via plugins | Leaflet.draw |
| Heatmaps | Via plugins | Leaflet.heat |
| Costo | $0 | Completamente gratis |

---

### 3. Overture Maps Foundation

| Campo | Evaluación |
|-------|------------|
| Descripción | Datos de mapa abiertos (Linux Foundation + Meta, Microsoft, AWS, TomTom) |
| Datos disponibles | Edificios, POIs, transporte, límites administrativos, direcciones |
| Cobertura México | 🔴 Por evaluar |
| Formato | GeoParquet, GeoJSON |
| Costo | Gratis |
| Actualización | Releases periódicos |

**Preguntas por investigar:**
- [ ] ¿Qué calidad tienen los datos de Overture para México/CDMX?
- [ ] ¿Los building footprints son suficientes para nuestro uso?
- [ ] ¿Los POIs incluyen infraestructura industrial relevante?
- [ ] ¿Cómo se integra con PostGIS?

---

### 4. Formatos de Datos Geoespaciales

| Formato | Origen | Cómo ingestar | Herramienta |
|---------|--------|---------------|-------------|
| Shapefile (.shp) | INEGI, gobierno | ogr2ogr o shp2pgsql → PostGIS | GDAL/OGR |
| GeoJSON | APIs, exports | INSERT directo en PostGIS | Nativo |
| KML | Google Earth, exports | ogr2ogr → PostGIS | GDAL/OGR |
| CSV con lat/lon | Portales, Excel | ST_MakePoint en INSERT | SQL |
| GeoParquet | Overture Maps | Carga via Python (geopandas) | geopandas + SQLAlchemy |

**Pipeline propuesto:**
```
Archivo externo → ogr2ogr/geopandas → PostGIS → API → Frontend (mapa)
```

---

### 5. Gestión de Zonas y Polígonos

| Tipo de zona | Fuente | Formato | Cómo almacenar |
|-------------|--------|---------|----------------|
| Entidades (estados) | INEGI Marco Geoestadístico | SHP | PostGIS MULTIPOLYGON |
| Municipios | INEGI | SHP | PostGIS MULTIPOLYGON |
| AGEB | INEGI | SHP | PostGIS MULTIPOLYGON |
| Corredores industriales | Definidos por Beiqa | Dibujados en mapa | PostGIS POLYGON |
| Colonias/Códigos Postales | INEGI/Correos | SHP/CSV | PostGIS POLYGON |
| Zonas custom del cliente | Dibujadas por el equipo | Mapa interactivo | PostGIS POLYGON |

**Preguntas por investigar:**
- [ ] ¿Cuántos polígonos de zona necesitamos para CDMX?
- [ ] ¿El equipo necesita poder dibujar zonas custom?
- [ ] ¿Los polígonos del INEGI son suficientes o se necesitan zonas más granulares?

---

### 6. Análisis de Proximidad

| Análisis | Descripción | Implementación |
|----------|-------------|----------------|
| Radio de búsqueda | "Propiedades a < 5km del aeropuerto" | PostGIS ST_DWithin |
| Isócrona | "Qué alcanzas en 30 min en auto" | Mapbox Isochrone API o OpenRouteService |
| Distancia a POIs | "Distancia a carretera más cercana" | PostGIS ST_Distance + tabla de POIs |
| Nearest N | "5 propiedades más cercanas al cliente" | PostGIS ORDER BY ST_Distance LIMIT N |

---

### 7. Renderización y Performance

| Estrategia | Mejor para | Complejidad |
|------------|-----------|-------------|
| Tiles raster (pre-generados) | Mapas base | Alta (necesita tile server) |
| Tiles vector (Mapbox/protobuf) | Mapas interactivos, zoom smooth | Media (Mapbox ofrece esto) |
| GeoJSON directo | < 1000 features | Baja (carga todo en browser) |
| Clustering | Muchos puntos | Baja (plugin de Leaflet/Mapbox) |

**Para BEIQA MVP:** GeoJSON directo + clustering es suficiente para cientos de propiedades.

---

## Recomendación Preliminar por Componente

| Componente | Recomendación MVP | Alternativa | Justificación |
|------------|-------------------|-------------|---------------|
| Backend GIS | PostGIS | N/A | Ya decidido |
| Mapa frontend | Leaflet + OSM (gratis) | Mapbox (si se necesita más) | $0, suficiente para MVP |
| Geocoding | Google Geocoding API | Nominatim | Precisión en direcciones mexicanas |
| POIs | Google Places | Overture Maps | Calidad en México |
| Isócronas | OpenRouteService | Mapbox Isochrone | Gratis |
| Polígonos CDMX | INEGI shapefiles | N/A | Gratis, oficial |
| Pipeline de datos | ogr2ogr + geopandas | N/A | Estándar de la industria |

---

## Estimación de Costos GIS

### Escenario MVP (costo mensual)

| Componente | Costo |
|------------|-------|
| PostGIS | Incluido en hosting DB |
| Leaflet + OSM | $0 |
| Google Geocoding (500 requests/mes) | ~$2.50 (cubierto por crédito) |
| Google Places (500 requests/mes) | ~$8.50 (cubierto por crédito) |
| INEGI Shapefiles | $0 |
| **Total GIS mensual MVP** | **~$0-11/mes** |

---

## Acciones de Investigación

1. [ ] Evaluar calidad de datos Overture Maps para CDMX
2. [ ] Descargar y probar shapefiles de INEGI para CDMX
3. [ ] Hacer PoC de mapa con Leaflet + datos de propiedades
4. [ ] Probar geocoding de direcciones mexicanas (Google vs Nominatim)
5. [ ] Evaluar isochrone APIs (OpenRouteService vs Mapbox)
6. [ ] Medir performance con volumen estimado de propiedades
7. [ ] Documentar decisión final por componente
