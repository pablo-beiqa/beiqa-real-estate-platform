# Schema Real de la Base de Datos

> **Estado**: ✅ En producción | **Fecha**: Marzo 2026
>
> ⚠️ **Nota**: Este documento describe el schema implementado. Para el SQL exacto con tipos de columna completos, ver las migrations en el repo de código (`supabase/migrations/`). Para el detalle de columnas de scrapers, ver `docs/infraestructura/supabase.md` en el repo `beiqa-scraper`.
>
> Este documento complementa [Data-Model.md](Data-Model.md), que describe la visión original de discovery. Lo que está aquí es lo que realmente existe.

---

## Stack de base de datos

- **Motor**: PostgreSQL 15 (vía Supabase)
- **Extensions**: PostGIS (geography, geometría), uuid-ossp
- **Auth**: Supabase Auth (Row Level Security habilitado)
- **API**: Supabase REST automático + RPC functions
- **Migrations**: 14+ activas (I24 originales + scraper tables)

---

## Tablas principales

### `inmuebles24_listings`

Tabla principal de propiedades scrapeadas de Inmuebles24. Es el corazón del sistema.

**Campos de identificación:**
- `id` — UUID, primary key
- `external_id` — ID del listing en Inmuebles24
- `source_url` — URL del listing original

**Campos de propiedad (búsqueda frecuente — columnas directas):**
- `property_type` — Tipo: oficina, bodega, local, terreno, industrial, etc.
- `operation_type` — Renta o Venta
- `price_rent` — Precio de renta
- `price_sale` — Precio de venta
- `currency` — MXN o USD
- `total_area_m2` — Superficie total
- `land_area_m2` — Superficie de terreno (cuando aplica)
- `construction_area_m2` — Superficie construida (cuando aplica)

**Campos geográficos:**
- `geo_point` — `geography(POINT)` — generado por trigger desde lat/lng
- `latitude` — Latitud original del listing
- `longitude` — Longitud original del listing
- `address_text` — Dirección como texto
- `colonia` — Colonia/barrio
- `municipio` — Municipio/alcaldía
- `estado` — Estado (CDMX, EdoMex, Morelos, Puebla)
- `postal_code` — Código postal

**Campos de seguimiento:**
- `first_seen_at` — Primera vez que apareció en scraping
- `last_seen_at` — Última vez visto en scraping
- `scrapes_sin_aparecer` — Contador de scrapes donde no apareció (para detectar listings retirados)
- `is_active` — Boolean, activo en el portal
- `price_history` — JSONB, historial de cambios de precio

**Campos de detalles (JSONB — detalles variables por tipo):**
- `features` — Características específicas del inmueble (número de baños, estacionamientos, etc.)
- `property_features` — Features procesadas con AI (extraídas de la descripción por Trigger.dev)
- `raw_data` — Datos crudos del scraping (para debug y reprocesamiento)

**Campos de texto:**
- `title` — Título del listing
- `description` — Descripción completa

**Timestamps:**
- `created_at` — Creación en la DB
- `updated_at` — Última actualización

**Índices:**
- GIST en `geo_point` — búsquedas geoespaciales
- Compuesto en `(property_type, operation_type, is_active)` — filtros frecuentes
- En `price_rent`, `price_sale`, `total_area_m2` — rangos de búsqueda
- En `last_seen_at` — frescura de datos

---

### `brokers`

Brokers/agentes inmobiliarios encontrados en los listings.

- `id` — UUID
- `name` — Nombre del broker
- `email` — Email (si disponible)
- `phone` — Teléfono (si disponible)
- `source` — Portal donde se encontró
- `hubspot_contact_id` — ID del contacto en HubSpot (cuando está sincronizado)
- `created_at`, `updated_at`

---

### `empresas_brokers`

Empresas/agencias a las que pertenecen los brokers.

- `id` — UUID
- `name` — Nombre de la empresa
- `website` — Sitio web
- `hubspot_company_id` — ID de empresa en HubSpot
- `created_at`, `updated_at`

Relación con `brokers`: una empresa tiene muchos brokers.

---

### `tenants`

Clientes de Beiqa (empresas que buscan espacio).

- `id` — UUID
- `company_name` — Nombre de la empresa cliente
- `contact_name` — Nombre del contacto principal
- `contact_email` — Email
- `contact_phone` — Teléfono
- `hubspot_deal_id` — ID del deal en HubSpot
- `status` — Estado del cliente (activo, en_proceso, cerrado)
- `created_at`, `updated_at`

---

### `portales_config`

Configuración y credenciales por portal de scraping.

- `id` — UUID
- `portal_name` — Nombre: inmuebles24, easybroker, etc.
- `apify_actor_id` — ID del actor de Apify
- `is_active` — Si el scraping está activo para este portal
- `last_run_at` — Última ejecución
- `config` — JSONB con configuración específica del portal (URLs base, filtros, etc.)
- `created_at`, `updated_at`

---

### `error_logs`

Tabla de tracking de errores del sistema.

- `id` — UUID
- `source` — Origen del error: n8n, trigger.dev, apify, etc.
- `error_type` — Tipo de error
- `error_message` — Mensaje de error
- `context` — JSONB con contexto adicional
- `resolved_at` — Cuando se resolvió (nullable)
- `created_at`

---

### `possible_duplicates`

Listings que el sistema identificó como posibles duplicados.

- `id` — UUID
- `listing_a_id` — FK a `inmuebles24_listings`
- `listing_b_id` — FK a `inmuebles24_listings`
- `similarity_score` — Score de similitud (0-1)
- `match_reasons` — JSONB: qué campos coincidieron (geo, precio, área, dirección)
- `status` — pending, confirmed_duplicate, false_positive
- `reviewed_by` — Quién revisó
- `reviewed_at`
- `created_at`

---

### `cbre_listings`

Staging table para propiedades scrapeadas de CBRE México (`propiedadescbre.net`).

- PK compuesto: `(listing_id, tenant_id)`
- 40+ columnas: título, precios (renta + venta separados), 5 dimensiones de área, ubicación completa con lat/lng, submarket, building class, delivery condition
- Campos JSONB: `brokers`, `spaces`, `images`, `features`, `raw_data`
- Campos de seguimiento: `is_active`, `scraped_at`, `last_seen_at`
- Campos de archivos: `image_storage_urls`, `pdf_storage_urls` (URLs en Supabase Storage)

> Para el schema completo de columnas ver `docs/scrapers/cbre.md` en el repo `beiqa-scraper`.

---

### `colliers_listings`

Staging table para propiedades scrapeadas de Colliers México (`colliers.com/en-mx`).

- PK compuesto: `(listing_id, tenant_id)`
- Similar a `cbre_listings` con campos específicos: `ceiling_height_m`, `docks`, `certifications` (JSONB)
- Campos JSONB: `brokers`, `images`, `raw_data`
- Campos de archivos: `image_storage_urls`, `pdf_storage_urls`

> Para el schema completo ver `docs/scrapers/colliers.md` en el repo `beiqa-scraper`.

---

### `finsa_listings`

Staging table para propiedades scrapeadas de FINSA (`finsa.net`). El scraper más maduro, con validación de ciclo de vida y H3 indexing.

- PK: `finsa_space_id` (sin `tenant_id`)
- 35+ columnas: áreas en m² y sqft, specs técnicas (HVAC, fire protection, electrical substation), certificaciones LEED/WELL/EDGE como booleans
- 7 índices H3: `h3_index_res5` a `h3_index_res11`
- Validación de ciclo de vida: `scrapes_sin_aparecer` (integer) — después de 2 ausencias consecutivas, `is_active = false`
- `flyer_url` — URL del PDF/flyer en Supabase Storage

> Para el schema completo ver `docs/scrapers/finsa.md` en el repo `beiqa-scraper`.

---

### `price_history`

Historial de cambios de precio para propiedades de todos los portales.

- `posting_id` — ID de la propiedad
- `tenant_id` — Tenant
- `price_amount` — Monto del precio
- `price_currency` — Moneda
- `created_at` — Fecha del registro

El primer precio se inserta manualmente en código (al crear la propiedad). Los cambios subsecuentes se capturan via trigger de DB en UPDATE.

---

## Storage Buckets (Supabase Storage)

| Bucket | Contenido | Scraper |
|--------|-----------|---------|
| `cbre-images` | Imágenes de propiedades (desde Google Cloud Storage) | CBRE |
| `cbre-pdfs` | PDFs/flyers de propiedades | CBRE |
| `colliers-images` | Imágenes de propiedades (desde Azure Blob Storage) | Colliers |
| `colliers-pdfs` | PDFs de propiedades | Colliers |
| `finsa-flyers` | Flyers PDF (desde documentdeliver.com) | FINSA |

---

## Triggers

### `populate_geo`

**Tabla**: `inmuebles24_listings`
**Evento**: BEFORE INSERT OR UPDATE
**Función**: Cuando se insertan o actualizan `latitude` y `longitude`, genera automáticamente el campo `geo_point` como `geography(POINT)` usando `ST_SetSRID(ST_MakePoint(longitude, latitude), 4326)`.

Esto permite queries geoespaciales como "propiedades en un radio de X km" sin calcular nada al vuelo.

### `price_history_on_update`

**Tablas**: `cbre_listings`, `colliers_listings`
**Evento**: AFTER UPDATE (cuando cambia el precio)
**Función**: Inserta un registro en `price_history` con el nuevo precio, capturando el historial de cambios automáticamente.

---

## RPC Functions (funciones de base de datos)

### `tendencia_precios`

Calcula la tendencia de precios para un tipo de propiedad en una zona geográfica.

**Parámetros**: `property_type`, `zone`, `period_months`
**Retorna**: Serie temporal de precio promedio por m2

---

### `increment_scrapes_sin_aparecer_finsa`

Incrementa atómicamente el contador de ausencias para un array de space IDs de FINSA. Usado por el post-scrape task para detectar propiedades que dejaron de aparecer.

**Parámetros**: `space_ids text[]` — array de IDs de espacios FINSA ausentes
**Efecto**: `UPDATE finsa_listings SET scrapes_sin_aparecer = scrapes_sin_aparecer + 1 WHERE finsa_space_id = ANY(space_ids)`

---

### `precio_promedio_m2`

Calcula el precio promedio por metro cuadrado para un tipo de propiedad y operación en una zona.

**Parámetros**: `property_type`, `operation_type`, `geometry_or_zone`
**Retorna**: `avg_price_per_m2`, `sample_size`, `period`

---

## Lo que está planificado (NO implementado aún)

Estos campos/tablas están en el backlog para Fase 2:

### Campos para H3 (geoespacial avanzado)
Recomendación de Alex (llamada 23 feb): precalcular dimensiones de búsqueda al momento del scraping.

```sql
-- Campos a agregar en migration futura:
ALTER TABLE inmuebles24_listings
ADD COLUMN h3_index_res7 TEXT,
ADD COLUMN h3_index_res9 TEXT;

-- Extensión a habilitar:
CREATE EXTENSION IF NOT EXISTS h3;

-- Trigger a crear:
-- Calcular H3 index al INSERT/UPDATE de geo_point
```

### Campo para AGEB (INEGI)
```sql
ALTER TABLE inmuebles24_listings ADD COLUMN ageb_id TEXT;
-- Se popula con spatial join cuando se cargue el shapefile de AGEBs
```

### Tabla `cache_api_responses`
Para cachear responses de Google Places, INEGI DENUE, etc. (Fase 2):
```sql
CREATE TABLE cache_api_responses (
  id UUID DEFAULT uuid_generate_v4(),
  api_source TEXT, -- 'google_places', 'inegi_denue', 'google_geocoding'
  query_key TEXT,  -- hash de los parámetros de la query
  response_data JSONB,
  fetched_at TIMESTAMPTZ,
  ttl_days INTEGER DEFAULT 30,
  h3_index TEXT   -- para cachear por zona geográfica
);
```

---

*Documento creado: 2026-02-24 | Actualizado: 2026-03-08 (tablas de scrapers, storage buckets, RPCs)*
*Para el SQL completo ver: `supabase/migrations/` en el repo de código*
