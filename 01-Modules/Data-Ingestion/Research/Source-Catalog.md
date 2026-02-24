# Catálogo de Fuentes de Datos

**Fase**: R-2 (Tecnología y Plataforma)
**Estado**: 🔴 Por investigar
**Depende de**: Cuestionario 02-Ingestion-de-Datos (R-0)

---

## Objetivo

Catalogar TODAS las fuentes de datos potenciales para BEIQA, con preguntas de investigación específicas por fuente y estrategia de integración.

> **Nota**: Este documento fue expandido para incluir preguntas de investigación por fuente y consolidar la estrategia de fuentes de datos que antes era un documento separado.

**Leyenda de estado**: 🔴 Por investigar | 🟡 En investigación | 🟢 Evaluado | ✅ Aprobado para MVP | ❌ Descartado | 🔵 Post-MVP

---

## 1. Portales Inmobiliarios

> Detalle completo en [Portal-Comparison.md](../../Scraper/Research/Portal-Comparison.md) y [Data-Acquisition-Strategy.md](../../Scraper/Research/Data-Acquisition-Strategy.md)

| # | Portal | Tipo | Método | Estado |
|---|--------|------|--------|--------|
| 1 | EasyBroker | Comercial/Brokers | API pública | 🔴 MVP |
| 2 | Inmuebles24 | Todos | Por determinar | 🔴 Post-MVP |
| 3 | Vivanuncios | Todos | Por determinar | 🔴 Post-MVP |
| 4 | Lamudi | Todos | Por determinar | 🔴 Post-MVP |
| 5 | Metros Cúbicos | Todos | Por determinar | 🔴 Post-MVP |
| 6 | Propiedades.com | Todos | Por determinar | 🔴 Post-MVP |
| 7 | Solili | Industrial | Posible suscripción | 🔴 Evaluar |
| 8 | LoopNet México | Comercial | Por determinar | 🔴 Post-MVP |

---

## 2. Fuentes Gubernamentales (Públicas)

### INEGI - DENUE (Directorio Estadístico Nacional de Unidades Económicas)

| Campo | Valor |
|-------|-------|
| URL | inegi.org.mx/servicios/api_denue.html |
| Tipo de datos | Unidades económicas: ubicación, actividad, tamaño |
| Formato | REST API, CSV, SHP |
| Acceso | Público, token gratuito |
| Costo | $0 |
| Estado | 🔴 Por investigar |

**Preguntas de investigación:**
- [ ] ¿Cuáles son los códigos SCIAN relevantes para inmuebles C/I?
- [ ] ¿Cuál es el rate limit de la API?
- [ ] ¿Cuántas unidades económicas hay en CDMX por código SCIAN relevante?
- [ ] ¿Qué calidad tienen las coordenadas?
- [ ] ¿Se pueden hacer queries masivas o hay límite por consulta?

**Valor para BEIQA:** Mapear empresas por zona para análisis de demanda y proximidad.

> Detalle completo en [INEGI-APIs.md](./INEGI-APIs.md)

---

### INEGI - Cartografía (Marco Geoestadístico)

| Campo | Valor |
|-------|-------|
| Datos | Polígonos: estados, municipios, AGEB, manzanas |
| Formato | SHP, GeoJSON |
| Acceso | Descarga pública |
| Costo | $0 |
| Estado | 🔴 Por investigar |

**Preguntas de investigación:**
- [ ] ¿Dónde descargar shapefiles actualizados para CDMX?
- [ ] ¿Qué sistema de coordenadas usan? (¿EPSG:4326 o EPSG:6372?)
- [ ] ¿Cuál es el peso de los archivos para CDMX?
- [ ] ¿Con qué frecuencia se actualizan?

**Valor para BEIQA:** Definir zonas/submercados usando polígonos oficiales.

---

### INEGI - Censos

| Campo | Valor |
|-------|-------|
| Datos | Demografía, economía, vivienda por AGEB |
| Formato | CSV, API |
| Acceso | Público |
| Costo | $0 |
| Estado | 🔴 Por investigar |

**Preguntas de investigación:**
- [ ] ¿Hay API para datos censales o solo descarga?
- [ ] ¿Qué granularidad geográfica tienen? (¿AGEB? ¿Municipio?)
- [ ] ¿Qué datos censales son relevantes para decisiones inmobiliarias?

---

### datos.gob.mx

| Campo | Valor |
|-------|-------|
| Datos | Múltiples datasets federales |
| Formato | CSV, JSON |
| Acceso | Público |
| Costo | $0 |
| Estado | 🔴 Por investigar |

**Preguntas de investigación:**
- [ ] ¿Qué datasets son relevantes? (desarrollo urbano, infraestructura, indicadores económicos)
- [ ] ¿Hay APIs o solo descarga manual?
- [ ] ¿Con qué frecuencia se actualizan?

> **Nota**: Anteriormente se planeó un archivo `datos-gob-mx.md` separado. Se consolida aquí.

---

### Catastro (CDMX)

| Campo | Valor |
|-------|-------|
| Datos | Valores catastrales, límites de lotes, propietarios |
| Formato | Varía por estado |
| Acceso | Varía (solicitud, portal web, OpenData) |
| Costo | Variable |
| Estado | 🔴 Por investigar |

**Preguntas de investigación:**
- [ ] ¿CDMX tiene portal de catastro con datos abiertos?
- [ ] ¿Qué datos son accesibles sin solicitud formal?
- [ ] ¿Hay restricciones legales para uso de datos catastrales?
- [ ] ¿Los valores catastrales se correlacionan con valores de mercado?

> **Nota**: Anteriormente se planeó un archivo `Catastro-Access.md` separado. Se consolida aquí.

---

### Banco de México

| Campo | Valor |
|-------|-------|
| Datos | Indicadores económicos: inflación, tipo de cambio, tasas |
| Formato | API (SIE) |
| Acceso | Público |
| Costo | $0 |
| Estado | 🔴 Por investigar |

**Preguntas de investigación:**
- [ ] ¿Qué indicadores son relevantes para el sector inmobiliario?
- [ ] ¿La API del SIE es fácil de consumir?
- [ ] ¿Con qué frecuencia se actualizan los datos?

---

### SEDATU / Planes de Desarrollo Urbano

| Campo | Valor |
|-------|-------|
| Datos | Zonificación, uso de suelo, planes de desarrollo |
| Formato | PDF, mapas, varía |
| Acceso | Varía |
| Estado | 🔴 Por investigar |

**Preguntas de investigación:**
- [ ] ¿CDMX tiene planes de desarrollo urbano digitalizados?
- [ ] ¿Se pueden extraer zonas de uso de suelo en formato geoespacial?
- [ ] ¿Existe SIGCDMX con capas útiles?

---

### DataMéxico

| Campo | Valor |
|-------|-------|
| URL | datamexico.org |
| Datos | Datos económicos integrados (comercio, empleo, industria) |
| Formato | Web, posible API |
| Acceso | Público |
| Estado | 🔴 Por investigar |

---

### Otros Gubernamentales

| Fuente | Datos | Relevancia | Estado |
|--------|-------|------------|--------|
| SIGCDMX | Info geográfica CDMX | Media-Alta | 🔴 |
| SCT/SICT | Infraestructura transporte | Media | 🔴 |
| CONAPO | Proyecciones población | Baja | 🔴 |
| IMSS | Datos de empleo | Baja | 🔴 |
| Registro Público | Titularidad | Baja | 🔴 |

---

## 3. Proveedores Comerciales

> **Nota**: Anteriormente se planeó un archivo `Commercial-Data-Providers.md` separado. Se difiere a post-MVP. Notas clave se capturan aquí.

| # | Proveedor | Datos | Costo Est. | Relevancia | Estado |
|---|-----------|-------|------------|------------|--------|
| 1 | CBRE Research | Market reports, vacancy | Alto | Alta | 🔵 Post-MVP |
| 2 | JLL Research | Market intelligence | Alto | Alta | 🔵 Post-MVP |
| 3 | Cushman & Wakefield | MarketBeat, analytics | Alto | Alta | 🔵 Post-MVP |
| 4 | Colliers | Research reports | Alto | Media | 🔵 Post-MVP |
| 5 | Real Capital Analytics | Transacciones | Muy alto | Media | 🔵 Post-MVP |
| 6 | CoStar | Datos RE completos | Muy alto | Alta (si opera en MX) | 🔵 Post-MVP |

**Preguntas diferidas a post-MVP:**
- ¿Alguno ofrece APIs o solo reportes PDF?
- ¿Cuál es el costo real de suscripción?
- ¿Los datos justifican el costo para una empresa del tamaño de Beiqa?

---

## 4. APIs de Mapas y Geolocalización

> Detalle completo en [Google-Maps-Platform.md](../../Geospatial/Research/Google-Maps-Platform.md) (ahora estrategia GIS completa)

| # | Servicio | Uso | Costo | Estado |
|---|----------|-----|-------|--------|
| 1 | Google Maps Platform | Mapas, Places, Geocoding, Routes | Pay per use ($200 crédito) | 🔴 |
| 2 | Mapbox | Mapas, isócronas, custom styles | Free tier generoso | 🔴 |
| 3 | OpenStreetMap + Leaflet | Mapas base + visualización | Gratis | 🔴 |
| 4 | Overture Maps Foundation | Datos abiertos de mapas | Gratis | 🔴 |
| 5 | OpenRouteService | Routing, isócronas | Gratis | 🔴 |
| 6 | Nominatim | Geocoding | Gratis | 🔴 |

---

## 5. Otras Fuentes Potenciales

| # | Fuente | Datos | Relevancia | Estado |
|---|--------|-------|------------|--------|
| 1 | Noticias del sector | Señales de inversión/expansión | Baja | 🔵 Post-MVP |
| 2 | LinkedIn | Señales de crecimiento empresarial | Baja | 🔵 Post-MVP |
| 3 | Bases internas de Beiqa | Histórico, conocimiento tribal | Alta | 🔴 MVP |
| 4 | Overture Maps | Building footprints, POIs | Media | 🔴 |

---

## Matriz de Priorización Actualizada

| Fuente | Valor de Negocio | Complejidad | Costo | Prioridad |
|--------|-----------------|-------------|-------|-----------|
| EasyBroker API | Alto | Baja | $0 | 🔴 **MVP** |
| INEGI DENUE API | Alto | Baja | $0 | 🔴 **MVP** |
| INEGI Cartografía | Alto (zonas) | Baja | $0 | 🔴 **MVP** |
| Mapas (Leaflet/Google) | Alto | Baja-Media | $0-15/mes | 🔴 **MVP** |
| Bases internas Beiqa | Alto | Baja | $0 | 🔴 **MVP** |
| Otros portales (scraping) | Medio | Media-Alta | Bajo | 🟡 Post-MVP |
| INEGI Censos | Medio | Baja | $0 | 🟡 Post-MVP |
| Banco de México | Bajo | Baja | $0 | 🟢 Diferido |
| Catastro CDMX | Medio | Alta | Bajo | 🟢 Diferido |
| Proveedores comerciales | Medio | Baja | Alto | 🔵 Diferido |

---

## Próximos Pasos

1. [ ] Investigar fuentes de prioridad **MVP** primero
2. [ ] Documentar hallazgos por fuente en este documento
3. [ ] Estimar costos donde aplique
4. [ ] Crear pipeline de ingestión para fuentes MVP
5. [ ] Decidir cuáles incluir en MVP final
