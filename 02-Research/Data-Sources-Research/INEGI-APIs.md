# Investigación: INEGI APIs

## Objetivo

Documentar todas las APIs y fuentes de datos del INEGI relevantes para BEIQA.

**Estado**: 🔴 Por investigar

---

## API del DENUE (Directorio Estadístico Nacional de Unidades Económicas)

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

### Preguntas de Investigación

- [ ] ¿Cuáles son los códigos SCIAN para bodegas, naves industriales, parques industriales?
- [ ] ¿Cuál es el rate limit de la API?
- [ ] ¿Cómo obtener el token de acceso?
- [ ] ¿Qué formato devuelve (JSON, XML)?
- [ ] ¿Hay límite de registros por consulta?
- [ ] ¿Se pueden hacer queries masivas?

### Códigos SCIAN Relevantes (Por Verificar)

| Código | Descripción |
|--------|-------------|
| 531??? | Servicios inmobiliarios |
| 493??? | Almacenamiento |
| 484??? | Transporte de carga |
| (investigar) | Parques industriales |

---

## Cartografía INEGI

### Marco Geoestadístico

| Nivel | Descripción | Formato |
|-------|-------------|---------|
| Nacional | Límites del país | SHP |
| Estatal | 32 entidades | SHP |
| Municipal | ~2,500 municipios | SHP |
| AGEB | Áreas geoestadísticas básicas | SHP |
| Manzana | Nivel más granular | SHP |

### Preguntas de Investigación

- [ ] ¿Dónde descargar los shapefiles actualizados?
- [ ] ¿Qué sistema de coordenadas usan?
- [ ] ¿Cuál es el peso de los archivos?
- [ ] ¿Hay API o solo descarga manual?
- [ ] ¿Con qué frecuencia se actualizan?

---

## Mapa Digital de México

- Herramienta de visualización del INEGI
- Más de 200 capas de información
- Permite descarga de datos

### Preguntas

- [ ] ¿Qué capas son relevantes para nosotros?
- [ ] ¿Se puede automatizar la descarga?

---

## Censos y Encuestas

| Censo/Encuesta | Datos | Frecuencia |
|----------------|-------|------------|
| Censo de Población | Demográficos | 10 años |
| Censo Económico | Unidades económicas | 5 años |
| ENOE | Empleo | Trimestral |
| (otros) | | |

### Preguntas

- [ ] ¿Hay APIs para acceder a datos censales?
- [ ] ¿Qué granularidad geográfica tienen?

---

## Acciones de Investigación

1. [ ] Registrarse para obtener token de API DENUE
2. [ ] Hacer pruebas de los endpoints
3. [ ] Identificar códigos SCIAN relevantes
4. [ ] Descargar shapefiles de muestra
5. [ ] Documentar formatos y estructuras
6. [ ] Evaluar calidad de datos para nuestro caso de uso

---

## Hallazgos

*(Documentar aquí los resultados de la investigación)*

### Fecha: ____

**Investigador**: ____

**Hallazgos**:
- 
- 
- 

**Conclusiones**:
- 
- 

**Recomendación**: ¿Usar / No usar? ¿Cómo?
