# Investigación: INEGI APIs y Datos

**Fase**: R-2 (Tecnología y Plataforma)
**Estado**: 🔴 Por investigar
**Depende de**: Cuestionario 02-Ingestion-de-Datos (R-0)

---

## Objetivo

Documentar todas las APIs y fuentes de datos del INEGI relevantes para BEIQA, evaluar calidad para CDMX, y definir un plan de integración.

> **Prioridad para MVP**: DENUE (unidades económicas) y Cartografía (polígonos de zonas). Los censos y otros datos son post-MVP.

---

## Estructura de Investigación

Para cada fuente de datos del INEGI, se debe responder:

1. **¿Qué datos proporciona?** (campos, cobertura, granularidad)
2. **¿Cómo se accede?** (API, descarga, portal web)
3. **¿Qué calidad tienen para CDMX?** (completitud, actualización, precisión)
4. **¿Cómo se integra con BEIQA?** (pipeline, frecuencia, almacenamiento)
5. **¿Qué valor agrega?** (qué decisión informa, qué análisis habilita)

---

## 1. API del DENUE (Directorio Estadístico Nacional de Unidades Económicas)

### Información General

| Campo | Valor |
|-------|-------|
| URL Documentación | https://www.inegi.org.mx/servicios/api_denue.html |
| Tipo | REST API |
| Autenticación | Token (registro gratuito) |
| Costo | Gratuito |
| Última actualización conocida | Noviembre 2024 |

### Datos Disponibles

- Identificación (CLEE, nombre, razón social)
- Ubicación (dirección completa, coordenadas lat/long, CP)
- Actividad económica (código SCIAN)
- Estrato de personal ocupado
- Contacto (teléfono, correo, web)
- Tipo de establecimiento

### Endpoints Principales

| Método | Descripción | Parámetros |
|--------|-------------|------------|
| Buscar | Por nombre y ubicación | Nombre, entidad, radio (max 5000m) |
| Ficha | Info de un establecimiento | ID específico |
| Nombre | Por razón social | Nombre, entidad |
| BuscarEntidad | Por entidad federativa | Entidad, actividad |
| Cuantificar | Estadísticas | Parámetros de filtro |

### Códigos SCIAN Relevantes (Por Verificar)

| Código | Descripción | Relevancia |
|--------|-------------|------------|
| 531 | Servicios inmobiliarios | Alta -- competidores/brokers |
| 493 | Almacenamiento y bodegaje | Alta -- demanda de bodegas |
| 484 | Transporte de carga | Alta -- logística |
| 461-469 | Comercio al por menor | Media -- retail |
| 431-435 | Comercio al por mayor | Media -- bodegas |
| 311-339 | Manufactura | Alta -- naves industriales |

### Plan de Investigación DENUE

| # | Acción | Estado |
|---|--------|--------|
| 1 | Registrarse para obtener token de API | 🔴 |
| 2 | Hacer request de prueba (BuscarEntidad para CDMX) | 🔴 |
| 3 | Documentar formato de respuesta (JSON/XML) | 🔴 |
| 4 | Verificar rate limits con pruebas reales | 🔴 |
| 5 | Contar unidades económicas por SCIAN relevante en CDMX | 🔴 |
| 6 | Evaluar calidad de coordenadas (spot check en mapa) | 🔴 |
| 7 | Estimar volumen total de datos para CDMX | 🔴 |
| 8 | Diseñar pipeline de ingestión a PostGIS | 🔴 |

### Valor para BEIQA

- **Mapear demanda**: Donde hay empresas manufactureras/logísticas, hay demanda de bodegas
- **Análisis de proximidad**: "¿Cuántas empresas hay en un radio de 5km de esta propiedad?"
- **Market intelligence**: Densidad de actividad económica por zona
- **Matching**: Vincular tipo de empresa con tipo de propiedad

---

## 2. Cartografía INEGI (Marco Geoestadístico)

### Niveles Disponibles

| Nivel | Descripción | Formato | Relevancia |
|-------|-------------|---------|------------|
| Nacional | Límites del país | SHP | Baja |
| Estatal | 32 entidades | SHP | Baja |
| Municipal | ~2,500 municipios | SHP | Alta (filtro) |
| AGEB | Áreas geoestadísticas básicas | SHP | Alta (zonas) |
| Manzana | Nivel más granular | SHP | Baja (muy detallado) |

### Plan de Investigación Cartografía

| # | Acción | Estado |
|---|--------|--------|
| 1 | Localizar URL de descarga de shapefiles actualizados | 🔴 |
| 2 | Descargar shapefiles de CDMX (municipios y AGEBs) | 🔴 |
| 3 | Verificar sistema de coordenadas (EPSG) | 🔴 |
| 4 | Medir tamaño de archivos | 🔴 |
| 5 | Cargar en PostGIS con ogr2ogr | 🔴 |
| 6 | Verificar que queries geoespaciales funcionan | 🔴 |
| 7 | Visualizar en mapa (QGIS o Leaflet) | 🔴 |
| 8 | Evaluar si AGEBs son buen proxy para "zonas" | 🔴 |

### Valor para BEIQA

- **Definición de zonas**: AGEBs como unidad base para submercados
- **Polígonos oficiales**: Límites municipales para filtrado
- **Overlay**: Superponer propiedades sobre zonas geográficas
- **Agregaciones**: Estadísticas por AGEB, municipio, etc.

---

## 3. Censos y Encuestas (Post-MVP)

| Censo/Encuesta | Datos | Frecuencia | Relevancia |
|----------------|-------|------------|------------|
| Censo de Población 2020 | Demográficos por AGEB | 10 años | Media |
| Censo Económico 2019 | Unidades económicas detalladas | 5 años | Alta |
| ENOE | Empleo, salarios | Trimestral | Baja |

### Preguntas de Investigación (Diferidas)

- [ ] ¿Hay APIs para datos censales o solo descarga?
- [ ] ¿Qué granularidad geográfica tienen? (AGEB, municipio)
- [ ] ¿Los datos del Censo Económico complementan o duplican DENUE?

---

## 4. Mapa Digital de México (Post-MVP)

- Herramienta de visualización del INEGI con 200+ capas
- Más de 200 capas de información geográfica

### Preguntas de Investigación (Diferidas)

- [ ] ¿Qué capas son relevantes? (infraestructura, transporte, hidrología)
- [ ] ¿Se puede automatizar la descarga de capas específicas?

---

## Resumen de Prioridades

| Fuente INEGI | Prioridad | Valor para MVP | Complejidad |
|-------------|-----------|---------------|-------------|
| DENUE API | 🔴 **MVP** | Alto -- mapear demanda | Baja |
| Cartografía (shapefiles) | 🔴 **MVP** | Alto -- definir zonas | Baja |
| Censo Económico | 🟡 Post-MVP | Medio -- enriquecer análisis | Baja |
| Censo de Población | 🟢 Diferido | Bajo | Baja |
| Mapa Digital | 🟢 Diferido | Bajo | Media |

---

## Hallazgos

*(Documentar aquí los resultados de la investigación conforme se avance)*

### DENUE

**Fecha**: ____
**Investigador**: ____

**Hallazgos**:
-
-
-

**Conclusión**: ¿Integrar en MVP? ¿Cómo?

### Cartografía

**Fecha**: ____
**Investigador**: ____

**Hallazgos**:
-
-
-

**Conclusión**: ¿Qué niveles cargar en PostGIS?
