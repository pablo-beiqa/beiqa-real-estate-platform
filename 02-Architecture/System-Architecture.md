# Arquitectura del Sistema - BEIQA Platform

## Objetivo

Documentar la arquitectura de alto nivel de la plataforma BEIQA.

**Estado**: 🟡 En diseño (borrador)

---

## Diagrama de Arquitectura

```mermaid
flowchart TB
    subgraph External[Fuentes Externas]
        Portals[Portales Inmobiliarios]
        PublicAPIs[APIs Públicas<br/>INEGI, datos.gob.mx]
        PrivateAPIs[APIs Privadas<br/>Google Maps, Comerciales]
        GeoFiles[Archivos Geo<br/>KML, Shapefiles]
        HubSpot[HubSpot CRM]
        CommChannels[Canales<br/>Email, WhatsApp]
    end
    
    subgraph Ingestion[Capa de Ingestión]
        Scraper[Scraper Service]
        DataIngester[Data Ingestion Service]
        GeoLoader[Geo Data Loader]
        CRMSync[CRM Sync Service]
    end
    
    subgraph Core[Core Services]
        DB[(PostgreSQL<br/>PostGIS)]
        FileStorage[(Object Storage<br/>S3/R2)]
        Cache[(Redis Cache)]
    end
    
    subgraph Processing[Procesamiento]
        MarketIntel[Market Intelligence<br/>Engine]
        GeoAnalysis[Geospatial<br/>Analysis Engine]
        AIBrain[AI Brain<br/>GenTik]
        Matching[Property<br/>Matching]
    end
    
    subgraph API[API Layer]
        RestAPI[REST API]
        GraphQLAPI[GraphQL API]
    end
    
    subgraph Frontend[Interfaces]
        InternalApp[Internal App<br/>Web]
        TenantPortal[Tenant Portal<br/>Web]
    end
    
    Portals --> Scraper
    PublicAPIs --> DataIngester
    PrivateAPIs --> DataIngester
    GeoFiles --> GeoLoader
    HubSpot <--> CRMSync
    CommChannels --> AIBrain
    
    Scraper --> DB
    DataIngester --> DB
    GeoLoader --> DB
    CRMSync --> DB
    
    DB --> MarketIntel
    DB --> GeoAnalysis
    DB --> AIBrain
    DB --> Matching
    Cache --> MarketIntel
    Cache --> GeoAnalysis
    
    MarketIntel --> RestAPI
    GeoAnalysis --> RestAPI
    AIBrain --> RestAPI
    Matching --> RestAPI
    DB --> RestAPI
    DB --> GraphQLAPI
    
    RestAPI --> InternalApp
    RestAPI --> TenantPortal
    GraphQLAPI --> InternalApp
```

---

## Componentes Principales

### 1. Capa de Ingestión

#### Scraper Service
- **Responsabilidad**: Extracción de propiedades de portales
- **Tecnología**: Python (Scrapy o Playwright)
- **Frecuencia**: Diaria/semanal
- **Output**: Propiedades normalizadas a BD

#### Data Ingestion Service
- **Responsabilidad**: Consumo de APIs externas estructuradas
- **Tecnología**: Python o Node.js
- **Fuentes**: INEGI, Google APIs, proveedores comerciales
- **Output**: Datos normalizados a BD

#### Geo Data Loader
- **Responsabilidad**: Carga de archivos geoespaciales
- **Tecnología**: Python (geopandas, fiona)
- **Formatos**: KML, Shapefiles, GeoJSON
- **Output**: Geometrías a PostGIS

#### CRM Sync Service
- **Responsabilidad**: Sincronización bidireccional con HubSpot
- **Tecnología**: Node.js con HubSpot SDK
- **Output**: Clientes y deals sincronizados

---

### 2. Core Services

#### Base de Datos Central
- **Tecnología**: PostgreSQL 15+ con PostGIS
- **Contenido**: 
  - Propiedades
  - Clientes
  - Búsquedas
  - Comunicaciones
  - Geometrías
  - Indicadores de mercado

#### Object Storage
- **Tecnología**: AWS S3, GCP Cloud Storage, o Cloudflare R2
- **Contenido**:
  - Imágenes de propiedades
  - Documentos
  - Reportes generados
  - Archivos geoespaciales originales

#### Cache Layer
- **Tecnología**: Redis
- **Uso**:
  - Resultados de queries frecuentes
  - Sesiones de usuario
  - Rate limiting

---

### 3. Procesamiento

#### Market Intelligence Engine
- **Responsabilidad**: Cálculos de inteligencia de mercado
- **Funciones**:
  - Cálculo de precios promedio por zona/tipo
  - Tendencias de precios
  - Tasas de vacancia
  - Análisis de oferta/demanda
- **Output**: KPIs y métricas almacenadas

#### Geospatial Analysis Engine
- **Responsabilidad**: Análisis espaciales
- **Funciones**:
  - Búsqueda por radio/polígono
  - Análisis de proximidad (POIs)
  - Cálculo de distancias
  - Overlay de capas
- **Tecnología**: PostGIS queries, Python (shapely)

#### AI Brain (GenTik)
- **Responsabilidad**: Procesamiento inteligente
- **Funciones**:
  - Matching propiedad-cliente
  - Interpretación de solicitudes NLP
  - Resumen de comunicaciones
  - Recomendaciones proactivas
- **Tecnología**: OpenAI API, LangChain

#### Property Matching
- **Responsabilidad**: Encontrar propiedades para búsquedas
- **Algoritmo**: Score basado en criterios + AI
- **Output**: Propiedades rankeadas por match

---

### 4. API Layer

#### REST API
- **Tecnología**: FastAPI (Python) o Express (Node.js)
- **Endpoints**:
  - CRUD de propiedades, clientes, búsquedas
  - Búsquedas avanzadas
  - Triggers de scraping
  - Reports

#### GraphQL API (Opcional)
- **Tecnología**: Apollo Server o Strawberry
- **Uso**: Queries flexibles para frontend
- **Ventaja**: Menos overfetching

---

### 5. Interfaces

#### Internal App (Web)
- **Usuarios**: Equipo interno de Beiqa
- **Tecnología**: React/Next.js + TypeScript
- **Funciones**:
  - Dashboard de propiedades
  - Gestión de clientes
  - Mapas interactivos
  - Reportes de inteligencia
  - Administración de scraper

#### Tenant Portal (Web)
- **Usuarios**: Clientes de Beiqa
- **Tecnología**: React/Next.js + TypeScript
- **Funciones**:
  - Ver propiedades shortlisted
  - Mapa de opciones
  - Comparativos
  - Reportes personalizados
  - Feedback y comunicación

---

## Decisiones de Arquitectura Pendientes (ADRs)

| ID | Decisión | Opciones | Estado |
|----|----------|----------|--------|
| ADR-001 | Lenguaje backend principal | Python vs Node.js | 🔴 Por decidir |
| ADR-002 | Framework web | FastAPI vs Express vs NestJS | 🔴 Por decidir |
| ADR-003 | Framework frontend | Next.js vs Remix | 🔴 Por decidir |
| ADR-004 | Cloud provider | AWS vs GCP vs Azure | 🔴 Por decidir |
| ADR-005 | Hosting DB | Managed vs Self-hosted | 🔴 Por decidir |
| ADR-006 | CI/CD | GitHub Actions vs GitLab CI | 🔴 Por decidir |

---

## Flujos de Datos Principales

### Flujo 1: Scraping de Propiedades

```mermaid
sequenceDiagram
    participant Cron as Scheduler
    participant Scraper as Scraper Service
    participant Portal as Portal Web
    participant Normalizer as Normalizer
    participant DB as PostgreSQL
    participant Geocoder as Geocoding API
    
    Cron->>Scraper: Trigger scheduled job
    loop Para cada portal
        Scraper->>Portal: HTTP Request
        Portal-->>Scraper: HTML Response
        Scraper->>Normalizer: Raw data
        Normalizer-->>Scraper: Normalized properties
    end
    loop Para propiedades sin coords
        Scraper->>Geocoder: Address
        Geocoder-->>Scraper: Coordinates
    end
    Scraper->>DB: INSERT/UPDATE properties
    Scraper->>DB: Log execution
```

### Flujo 2: Búsqueda de Cliente

```mermaid
sequenceDiagram
    participant User as Usuario Beiqa
    participant App as Internal App
    participant API as REST API
    participant Matching as Matching Engine
    participant AI as AI Brain
    participant DB as PostgreSQL
    
    User->>App: Crea búsqueda para cliente
    App->>API: POST /search-requests
    API->>DB: INSERT search_request
    API->>Matching: Find matching properties
    Matching->>DB: Query properties with criteria
    DB-->>Matching: Candidate properties
    Matching->>AI: Rank by fit
    AI-->>Matching: Scored properties
    Matching->>DB: INSERT search_properties
    Matching-->>API: Top matches
    API-->>App: Search created with matches
    App-->>User: Display results
```

### Flujo 3: Portal de Tenant

```mermaid
sequenceDiagram
    participant Tenant as Cliente Tenant
    participant Portal as Tenant Portal
    participant API as REST API
    participant DB as PostgreSQL
    
    Tenant->>Portal: Login
    Portal->>API: GET /my-searches
    API->>DB: Query search_requests for client
    DB-->>API: Active searches
    API-->>Portal: Searches with properties
    Portal-->>Tenant: Dashboard con opciones
    
    Tenant->>Portal: Ver propiedad
    Portal->>API: GET /properties/{id}
    API->>DB: Property details
    DB-->>API: Property + images + market data
    API-->>Portal: Full property data
    Portal-->>Tenant: Property detail view
    
    Tenant->>Portal: Dar feedback
    Portal->>API: PUT /search-properties/{id}
    API->>DB: UPDATE status + feedback
    API-->>Portal: Success
```

---

## Consideraciones de Seguridad

### Autenticación
- Internal App: Auth0 o similar (SSO)
- Tenant Portal: Magic link o password
- APIs: JWT tokens

### Autorización
- RBAC para usuarios internos
- Tenant solo ve sus búsquedas/propiedades

### Datos sensibles
- No almacenar datos personales de terceros (contactos de listings)
- Encriptar datos de clientes en reposo
- HTTPS en todos los endpoints

---

## Próximos Pasos

1. [ ] Tomar decisiones de ADRs pendientes
2. [ ] Diseñar API contracts detallados
3. [ ] Crear diagrama de deployment
4. [ ] Definir estrategia de testing
5. [ ] Estimar infraestructura necesaria
