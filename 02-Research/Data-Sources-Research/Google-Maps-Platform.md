# Investigación: Google Maps Platform

## Objetivo

Evaluar las APIs de Google Maps Platform para las necesidades de BEIQA y estimar costos.

**Estado**: 🔴 Por investigar

---

## APIs Potencialmente Necesarias

### 1. Maps JavaScript API

**Uso**: Visualización de mapas interactivos en la aplicación.

| Campo | Valor |
|-------|-------|
| Precio | $7.00 USD / 1,000 cargas de mapa |
| Free tier | $200/mes de crédito gratuito |
| Documentación | https://developers.google.com/maps/documentation/javascript |

**Preguntas**:
- [ ] ¿Cuántas cargas de mapa estimamos por mes?
- [ ] ¿El crédito gratuito es suficiente para MVP?

---

### 2. Places API

**Uso**: Buscar puntos de interés (POIs) - carreteras, puertos, aeropuertos, etc.

| Campo | Valor |
|-------|-------|
| Precio - Basic | $17.00 / 1,000 requests |
| Precio - Details | $17.00 / 1,000 requests |
| Precio - Autocomplete | $2.83 / 1,000 requests |

**Preguntas**:
- [ ] ¿Cuántos POIs necesitamos buscar?
- [ ] ¿Podemos cachear los POIs para reducir requests?
- [ ] ¿Hay alternativas gratuitas (OSM, Foursquare)?

---

### 3. Geocoding API

**Uso**: Convertir direcciones a coordenadas (lat/long).

| Campo | Valor |
|-------|-------|
| Precio | $5.00 / 1,000 requests |

**Preguntas**:
- [ ] ¿Cuántas direcciones necesitamos geocodificar?
- [ ] ¿Los portales ya dan coordenadas o solo direcciones?
- [ ] ¿Podemos usar Nominatim (gratis) como alternativa?

---

### 4. Routes API / Directions API

**Uso**: Calcular rutas y tiempos de viaje.

| Campo | Valor |
|-------|-------|
| Precio | $5.00 - $10.00 / 1,000 requests |

**Preguntas**:
- [ ] ¿Necesitamos cálculo de rutas en tiempo real?
- [ ] ¿O solo distancias punto a punto?
- [ ] ¿Podemos usar OpenRouteService (gratis)?

---

### 5. Distance Matrix API

**Uso**: Matriz de distancias entre múltiples puntos.

| Campo | Valor |
|-------|-------|
| Precio | $5.00 / 1,000 elementos |
| Elemento | Cada combinación origen-destino |

**Preguntas**:
- [ ] ¿Necesitamos matrices de distancia?
- [ ] ¿O solo distancias individuales?

---

## Estimación de Costos (Por Calcular)

### Escenario MVP

| API | Uso Estimado/Mes | Precio Unitario | Costo Mensual |
|-----|------------------|-----------------|---------------|
| Maps JS | ??? cargas | $7/1000 | $??? |
| Geocoding | ??? requests | $5/1000 | $??? |
| Places | ??? requests | $17/1000 | $??? |
| Routes | ??? requests | $5/1000 | $??? |
| **TOTAL** | | | **$???** |
| Crédito gratuito | | | -$200 |
| **TOTAL REAL** | | | **$???** |

### Escenario Producción

*(Calcular después de validar MVP)*

---

## Alternativas a Evaluar

### OpenStreetMap + Herramientas Open Source

| Funcionalidad | Google | Alternativa Open Source | Trade-off |
|---------------|--------|------------------------|-----------|
| Mapas base | Maps JS | Leaflet + OSM tiles | Menos features, gratis |
| Geocoding | Geocoding API | Nominatim | Menor precisión, gratis |
| Routing | Routes API | OpenRouteService | Límites de uso, gratis |
| Places/POIs | Places API | Overpass API (OSM) | Datos diferentes, gratis |

### Mapbox

| Campo | Google Maps | Mapbox |
|-------|-------------|--------|
| Pricing | Pay per use | Free tier más generoso |
| Customization | Limitado | Alta personalización |
| Calidad en México | Excelente | Buena |

### HERE

| Campo | Valor |
|-------|-------|
| Cobertura México | Por verificar |
| Pricing | Pay per use |

---

## Preguntas de Decisión

1. [ ] ¿Cuál es el presupuesto mensual máximo para APIs de mapas?
2. [ ] ¿Qué nivel de calidad/precisión necesitamos?
3. [ ] ¿Podemos empezar con alternativas gratuitas y migrar después?
4. [ ] ¿Qué tan importante es la UX de los mapas para clientes?

---

## Acciones de Investigación

1. [ ] Crear cuenta en Google Cloud Platform
2. [ ] Habilitar APIs en modo de prueba
3. [ ] Hacer pruebas de cada API con datos de México
4. [ ] Evaluar calidad de geocoding en direcciones mexicanas
5. [ ] Comparar con alternativas gratuitas
6. [ ] Calcular estimación de costos realista

---

## Hallazgos

*(Documentar aquí los resultados de la investigación)*
