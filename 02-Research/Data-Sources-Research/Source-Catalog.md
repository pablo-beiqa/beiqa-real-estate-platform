# Catálogo de Fuentes de Datos

## Objetivo

Este documento cataloga TODAS las fuentes de datos potenciales para la plataforma BEIQA. Cada fuente debe ser investigada y evaluada antes de decidir cuáles integrar.

**Estado**: 🔴 Por investigar | 🟡 En investigación | 🟢 Evaluado | ✅ Aprobado | ❌ Descartado

---

## 1. Portales Inmobiliarios (Scraping)

| # | Portal | Tipo Inmuebles | Cobertura | API | Estado | Responsable |
|---|--------|----------------|-----------|-----|--------|-------------|
| 1 | Inmuebles24 | Todos | Nacional | ¿? | 🔴 | |
| 2 | Vivanuncios | Todos | Nacional | ¿? | 🔴 | |
| 3 | Segundamano | Todos | Nacional | ¿? | 🔴 | |
| 4 | Metros Cúbicos | Todos | Nacional | ¿? | 🔴 | |
| 5 | Lamudi | Todos | Nacional | ¿? | 🔴 | |
| 6 | Propiedades.com | Todos | Nacional | ¿? | 🔴 | |
| 7 | EasyBroker | Comercial | Nacional | ¿? | 🔴 | |
| 8 | Solili | Industrial | Nacional | ¿? | 🔴 | |
| 9 | LoopNet México | Comercial | Nacional | ¿? | 🔴 | |
| 10 | (Otros por identificar) | | | | 🔴 | |

**Documentos detallados**: 
- [[Portal-Comparison|Comparativo de Portales]]

---

## 2. Fuentes Gubernamentales (Públicas)

| # | Fuente | Tipo de Datos | Formato | Acceso | Estado | Responsable |
|---|--------|---------------|---------|--------|--------|-------------|
| 1 | INEGI - DENUE | Unidades económicas | API, CSV, SHP | Público | 🔴 | |
| 2 | INEGI - Cartografía | Polígonos territoriales | SHP, GeoJSON | Público | 🔴 | |
| 3 | INEGI - Censos | Demográficos | CSV, API | Público | 🔴 | |
| 4 | datos.gob.mx | Múltiples datasets | CSV, JSON | Público | 🔴 | |
| 5 | DataMéxico | Económicos integrados | Web, ¿API? | Público | 🔴 | |
| 6 | SEDATU | Desarrollo urbano | Varía | Varía | 🔴 | |
| 7 | Catastro (por estado) | Valores, propietarios | Varía | Varía | 🔴 | |
| 8 | Registro Público | Titularidad | Varía | Solicitud | 🔴 | |
| 9 | SIGCDMX | Info geográfica CDMX | Web | Público | 🔴 | |
| 10 | Planes Desarrollo Urbano | Zonificación | PDF, mapas | Varía | 🔴 | |
| 11 | SCT/SICT | Infraestructura transporte | Varía | Público | 🔴 | |
| 12 | Banco de México | Indicadores económicos | API | Público | 🔴 | |
| 13 | CONAPO | Proyecciones población | CSV | Público | 🔴 | |
| 14 | IMSS | Datos de empleo | ¿? | ¿? | 🔴 | |

**Documentos detallados**:
- [[INEGI-APIs|Investigación INEGI]]
- [[02-Research/Data-Sources-Research/datos-gob-mx|Investigación datos.gob.mx]]
- [[02-Research/Data-Sources-Research/Catastro-Access|Acceso a Catastro]]

---

## 3. Proveedores Comerciales

| # | Proveedor | Tipo de Datos | Cobertura | Costo Est. | Estado | Responsable |
|---|-----------|---------------|-----------|------------|--------|-------------|
| 1 | CBRE Research | Market reports, vacancy | Nacional | ¿? | 🔴 | |
| 2 | JLL Research | Market intelligence | Nacional | ¿? | 🔴 | |
| 3 | Cushman & Wakefield | MarketBeat, analytics | Nacional | ¿? | 🔴 | |
| 4 | Newmark | Research | Nacional | ¿? | 🔴 | |
| 5 | Colliers | Research | Nacional | ¿? | 🔴 | |
| 6 | SoftPro | Software inmobiliario | México | ¿? | 🔴 | |
| 7 | Real Capital Analytics | Transacciones | Global | ¿? | 🔴 | |
| 8 | CoStar | Datos RE | ¿México? | ¿? | 🔴 | |
| 9 | (Proveedores locales) | ¿? | México | ¿? | 🔴 | |

**Documentos detallados**:
- [[02-Research/Data-Sources-Research/Commercial-Data-Providers|Proveedores Comerciales]]

---

## 4. APIs de Mapas y Geolocalización

| # | Servicio | Funcionalidades | Modelo Precio | Costo Est. | Estado | Responsable |
|---|----------|-----------------|---------------|------------|--------|-------------|
| 1 | Google Maps Platform | Maps, Places, Routes | Pay per use | ¿? | 🔴 | |
| 2 | Mapbox | Maps, Geocoding | Pay per use | ¿? | 🔴 | |
| 3 | HERE | Maps, Routing | Pay per use | ¿? | 🔴 | |
| 4 | OpenStreetMap | Base map data | Gratis | $0 | 🔴 | |
| 5 | Leaflet | Visualización | Gratis | $0 | 🔴 | |
| 6 | OpenRouteService | Routing | Gratis | $0 | 🔴 | |
| 7 | Nominatim | Geocoding | Gratis | $0 | 🔴 | |
| 8 | Foursquare Places | POIs | Pay per use | ¿? | 🔴 | |

**Documentos detallados**:
- [[Google-Maps-Platform|Google Maps Platform]]
- [[02-Research/Technology-Research/Geospatial-Stack|Stack Geoespacial]]

---

## 5. Otras Fuentes Potenciales

| # | Fuente | Tipo de Datos | Relevancia | Estado |
|---|--------|---------------|------------|--------|
| 1 | Noticias sector | Anuncios inversión | Media | 🔴 |
| 2 | LinkedIn | Señales expansión empresas | Baja | 🔴 |
| 3 | Bases de datos internas Beiqa | Histórico, conocimiento | Alta | 🔴 |
| 4 | (Por identificar) | | | 🔴 |

---

## Matriz de Priorización

| Fuente | Valor para Negocio | Complejidad Técnica | Costo | Prioridad |
|--------|-------------------|---------------------|-------|-----------|
| Portales (scraping) | Alto | Media | Bajo | 🔴 ALTA |
| INEGI DENUE | Alto | Baja | Gratis | 🔴 ALTA |
| Google Maps | Alto | Baja | Medio | 🟡 MEDIA |
| Proveedores comerciales | Medio | Baja | Alto | 🟡 MEDIA |
| Catastro | Medio | Alta | Bajo | 🟢 BAJA |

---

## Próximos Pasos

- [ ] Asignar responsable a cada fuente
- [ ] Investigar las de prioridad ALTA primero
- [ ] Documentar hallazgos en archivos individuales
- [ ] Estimar costos donde aplique
- [ ] Decidir cuáles incluir en MVP
