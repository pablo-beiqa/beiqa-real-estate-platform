# Validación de Arquitectura

**Fase**: R-3 (Validación y Factibilidad)
**Estado**: 🔴 Por completar (requiere hallazgos de R-1 y R-2)
**Depende de**: Todos los documentos de R-1 y R-2

---

## Objetivo

Revisar componente por componente la [Arquitectura del Sistema](../02-Architecture/System-Architecture.md) contra los hallazgos de la investigación. Determinar qué suposiciones son válidas, cuáles necesitan cambiar, y cuáles son innecesarias para el MVP.

> **Recordatorio**: El documento de arquitectura es una VISIÓN escrita ANTES de la investigación. No es un diseño validado.

---

## Metodología

Para cada componente de la arquitectura:

1. **¿Es necesario para MVP?** (Sí / No / Parcial)
2. **¿Está correctamente especificado?** (Sí / Necesita cambios / Incorrecto)
3. **¿Qué dice la investigación?** (Referencia al hallazgo específico)
4. **Recomendación** (Mantener / Modificar / Eliminar / Diferir)

---

## Capa de Ingestión

### Scraper Service

| Aspecto | Arquitectura Propuesta | Validación |
|---------|----------------------|------------|
| Existe en arquitectura | Sí -- Python (Scrapy o Playwright) |  |
| ¿MVP? | 🔴 _Depende de Data Acquisition findings_ |  |
| Suposición | Scraping automatizado de portales |  |
| Realidad según investigación | 🔴 _Podría ser API (EasyBroker), extensión Chrome, o híbrido_ |  |
| Frecuencia propuesta | Diaria/semanal |  |
| Realidad | 🔴 _Depende de respuestas del equipo en R-0_ |  |

**Referencia**: [Data-Acquisition-Strategy.md](../01-Modules/Scraper/Research/Data-Acquisition-Strategy.md)

**Validación**: 🔴 _Pendiente hallazgos de R-1_

**Recomendación**: 🔴 _Por determinar_

---

### Data Ingestion Service

| Aspecto | Arquitectura Propuesta | Validación |
|---------|----------------------|------------|
| Existe en arquitectura | Sí -- Python o Node.js |  |
| ¿MVP? | 🔴 _Depende de qué fuentes externas son MVP_ |  |
| Fuentes | INEGI, Google APIs, proveedores comerciales |  |
| Realidad según investigación | 🔴 _INEGI DENUE + Cartografía probablemente MVP, resto diferido_ |  |

**Referencia**: [Source-Catalog.md](../01-Modules/Data-Ingestion/Research/Source-Catalog.md), [INEGI-APIs.md](../01-Modules/Data-Ingestion/Research/INEGI-APIs.md)

**Validación**: 🔴 _Pendiente_

---

### Geo Data Loader

| Aspecto | Arquitectura Propuesta | Validación |
|---------|----------------------|------------|
| Existe en arquitectura | Sí -- Python (geopandas, fiona) |  |
| ¿MVP? | ✅ Probable (shapefiles del INEGI) |  |
| Formatos | KML, Shapefiles, GeoJSON |  |
| Herramientas | ogr2ogr, geopandas, shp2pgsql |  |

**Referencia**: [Google-Maps-Platform.md](../01-Modules/Geospatial/Research/Google-Maps-Platform.md) (estrategia GIS)

**Validación**: 🔴 _Pendiente_

---

### CRM Sync Service

| Aspecto | Arquitectura Propuesta | Validación |
|---------|----------------------|------------|
| Existe en arquitectura | Sí -- Node.js con HubSpot SDK |  |
| ¿MVP? | 🔴 _Probablemente no para día 1_ |  |
| Dirección | Bidireccional |  |
| Cuestión abierta | ¿Qué datos necesitan sincronizarse? ¿Es MVP? |  |

**Referencia**: Cuestionario [00-Vision-General.md](../00-Project/Product-Questions-Vision-General.md) pregunta 6

**Validación**: 🔴 _Pendiente respuestas de R-0_

---

## Core Services

### Base de Datos (PostgreSQL + PostGIS)

| Aspecto | Arquitectura Propuesta | Validación |
|---------|----------------------|------------|
| Motor | PostgreSQL 15+ con PostGIS | ✅ Validado -- mejor opción |
| Hosting | No especificado | 🔴 ADR-005 pendiente |
| Modelo de datos | PROPERTY, CLIENT, ZONE, etc. | 🔴 ¿Necesita PROPERTY_SOURCE para dedup? |

**Referencia**: [Database-Options.md](../02-Architecture/Database/Database-Options.md), [Deduplication-Strategy.md](../02-Architecture/Deduplication-Strategy.md)

**Cambio necesario**: Agregar tabla `PROPERTY_SOURCE` al modelo de datos para soportar deduplicación.

---

### Object Storage (S3/R2)

| Aspecto | Arquitectura Propuesta | Validación |
|---------|----------------------|------------|
| Existe en arquitectura | Sí -- AWS S3, GCP Cloud Storage, o Cloudflare R2 |  |
| ¿MVP? | 🔴 _Depende de si almacenamos imágenes localmente o solo URLs_ |  |
| Decisión abierta | ¿Guardar imágenes o solo URLs de portales? |  |

**Preguntas abiertas:**
- [ ] ¿Las URLs de imágenes en portales son estables o caducan?
- [ ] ¿El costo de almacenar imágenes justifica la confiabilidad?
- [ ] ¿Cuántas imágenes por propiedad en promedio?

---

### Cache Layer (Redis)

| Aspecto | Arquitectura Propuesta | Validación |
|---------|----------------------|------------|
| Existe en arquitectura | Sí -- Redis |  |
| ¿MVP? | ❌ Probablemente no |  |
| Justificación | 6-15 usuarios, volumen bajo de queries |  |
| Alternativa MVP | PostgreSQL puede manejar el volumen, in-memory cache en app |  |

**Recomendación preliminar**: Eliminar Redis del MVP. Agregar solo si hay problemas de performance.

---

## Procesamiento

### Market Intelligence Engine

| Aspecto | Arquitectura Propuesta | Validación |
|---------|----------------------|------------|
| Existe en arquitectura | Sí -- cálculos de precio, tendencias, vacancia |  |
| ¿MVP? | ⚠️ Parcial -- métricas básicas (precio/m2, conteo) |  |
| Funciones MVP | Precio promedio/zona, distribución, conteo |  |
| Funciones diferidas | Tendencias, vacancia, absorción |  |

**Referencia**: [Market-Intelligence-Strategy.md](../01-Modules/Market-Intelligence/Research/Market-Intelligence-Strategy.md)

---

### Geospatial Analysis Engine

| Aspecto | Arquitectura Propuesta | Validación |
|---------|----------------------|------------|
| Existe en arquitectura | Sí -- PostGIS queries, Python shapely |  |
| ¿MVP? | ✅ Sí (core del producto) |  |
| Funciones MVP | Búsqueda por radio/polígono, proximidad |  |
| Implementación | PostGIS queries (no necesita engine separado) |  |

**Referencia**: [Google-Maps-Platform.md](../01-Modules/Geospatial/Research/Google-Maps-Platform.md) (estrategia GIS)

**Nota**: Puede ser queries PostGIS directas, no necesariamente un "engine" separado.

---

### AI Brain (GenTik)

| Aspecto | Arquitectura Propuesta | Validación |
|---------|----------------------|------------|
| Existe en arquitectura | Sí -- OpenAI API, LangChain |  |
| ¿MVP? | ⚠️ Solo deduplicación asistida |  |
| Funciones MVP | Deduplicación de casos ambiguos |  |
| Funciones diferidas | NLP, matching, reportes, chat |  |

**Referencia**: [AI-Strategy.md](../01-Modules/AI-Brain/Research/AI-Strategy.md)

**Cambio necesario**: Reducir scope para MVP. No es un "brain" -- es un servicio de dedup.

---

### Property Matching

| Aspecto | Arquitectura Propuesta | Validación |
|---------|----------------------|------------|
| Existe en arquitectura | Sí -- Score basado en criterios + AI |  |
| ¿MVP? | ⚠️ Básico con SQL, sin IA |  |
| MVP | Filtros SQL (tipo, zona, superficie, precio) |  |
| Post-MVP | Scoring con IA |  |

---

## API Layer

### REST API

| Aspecto | Arquitectura Propuesta | Validación |
|---------|----------------------|------------|
| Existe en arquitectura | Sí -- FastAPI o Express |  |
| ¿MVP? | ✅ Sí |  |
| Framework | 🔴 ADR-002 pendiente |  |

### GraphQL API

| Aspecto | Arquitectura Propuesta | Validación |
|---------|----------------------|------------|
| Existe en arquitectura | Sí -- opcional |  |
| ¿MVP? | ❌ No necesario |  |
| Justificación | REST es suficiente para 6-15 usuarios, endpoints predefinidos |  |

**Recomendación**: Eliminar GraphQL del MVP. Solo REST.

---

## Interfaces

### Internal App (Web)

| Aspecto | Arquitectura Propuesta | Validación |
|---------|----------------------|------------|
| Existe en arquitectura | Sí -- React/Next.js |  |
| ¿MVP? | 🔴 _ADR-003 pendiente_ |  |
| Alternativas | Low-code, chatbot, Airtable-like, híbrido |  |

**Referencia**: [Interface-Strategy.md](../01-Modules/Internal-App/Research/Interface-Strategy.md)

### Tenant Portal (Web)

| Aspecto | Arquitectura Propuesta | Validación |
|---------|----------------------|------------|
| Existe en arquitectura | Sí -- React/Next.js |  |
| ¿MVP? | ❌ Probablemente no |  |
| Justificación | 🔴 _Depende de respuestas en cuestionario 07-Portal-Tenants_ |  |

---

## Flujos de Datos

### Flujo 1: Scraping de Propiedades

| Aspecto | Validación |
|---------|------------|
| ¿MVP? | ✅ Sí (core) |
| Cambios necesarios | Podría ser "Adquisición" (API/extensión), no "Scraping" |
| Agregar | Paso de deduplicación post-normalización |

### Flujo 2: Búsqueda de Cliente

| Aspecto | Validación |
|---------|------------|
| ¿MVP? | ✅ Sí (core) |
| Cambios necesarios | Matching sin IA para MVP (SQL puro) |

### Flujo 3: Portal de Tenant

| Aspecto | Validación |
|---------|------------|
| ¿MVP? | ❌ Probablemente post-MVP |
| Justificación | Depende de respuestas R-0 |

---

## Resumen de Validación

| Componente | Estado en Arquitectura | ¿MVP? | Cambio Necesario |
|------------|----------------------|--------|------------------|
| Scraper Service | Existe | ✅ (modificado) | Renombrar a "Data Acquisition", método flexible |
| Data Ingestion Service | Existe | ⚠️ Parcial | Solo INEGI para MVP |
| Geo Data Loader | Existe | ✅ | Sin cambios |
| CRM Sync Service | Existe | ❌ | Diferir |
| PostgreSQL + PostGIS | Existe | ✅ | Agregar PROPERTY_SOURCE |
| Object Storage | Existe | 🔴 | Decidir URLs vs. almacenamiento |
| Redis Cache | Existe | ❌ | Eliminar para MVP |
| Market Intelligence | Existe | ⚠️ Parcial | Solo métricas básicas |
| Geospatial Analysis | Existe | ✅ | PostGIS queries directas |
| AI Brain | Existe | ⚠️ Solo dedup | Reducir scope dramáticamente |
| Property Matching | Existe | ⚠️ Básico | SQL solo, sin IA |
| REST API | Existe | ✅ | Sin cambios |
| GraphQL API | Existe | ❌ | Eliminar para MVP |
| Internal App | Existe | ✅ | Interfaz por decidir (ADR-003) |
| Tenant Portal | Existe | ❌ | Diferir |

---

## Cambios al Modelo de Datos

| Cambio | Justificación | Referencia |
|--------|---------------|------------|
| Agregar `PROPERTY_SOURCE` | Deduplicación requiere tracking de fuentes | Deduplication-Strategy.md |
| Agregar `confidence_score` a PROPERTY | Score de confianza del golden record | Deduplication-Strategy.md |
| Considerar `MARKET_METRIC` por zona/periodo | Para inteligencia de mercado | Market-Intelligence-Strategy.md |

---

## Acciones Post-Validación

1. [ ] Actualizar diagrama de arquitectura con cambios de MVP
2. [ ] Crear nueva versión del diagrama "MVP Architecture"
3. [ ] Documentar componentes diferidos y criterios para incluirlos
4. [ ] Actualizar modelo de datos con cambios de deduplicación
5. [ ] Crear ADR formal para cada cambio significativo
