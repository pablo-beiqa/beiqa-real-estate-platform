# Schema Real de la Base de Datos

> **Estado**: ✅ En producción | **Fecha**: Marzo 2026
>
> ⚠️ **Nota**: Este documento describe el schema implementado. Para el SQL exacto con tipos de columna completos, ver las migrations en el repo de código (`supabase/migrations/`). Para el detalle de columnas de scrapers, ver `docs/infraestructura/supabase.md` en el repo `beiqa-scraper`.
>
> Este documento complementa [Data-Model.md](Data-Model.md), que describe la visión original de discovery. Lo que está aquí es lo que realmente existe.

---

## Stack de base de datos

- **Motor**: PostgreSQL 17 (vía Supabase)
- **Extensions**: PostGIS 3.3.7 (geography, geometría), uuid-ossp, pgcrypto, pg_trgm (fuzzy search), pg_stat_statements, pg_graphql, supabase_vault
- **Auth**: Supabase Auth (Row Level Security habilitado)
- **API**: Supabase REST automático + RPC functions
- **Migrations**: 32 activas (I24 originales + scrapers + brokers + cache + H3)

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
- `portal_name` — Nombre: inmuebles24, cbre, colliers, finsa, pincali, etc.
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
- `listing_a_id` — FK a `inmuebles24_listings` (nota: migrar a FK en `properties` golden record para dedup cross-portal)
- `listing_b_id` — FK a `inmuebles24_listings` (nota: migrar a FK en `properties` golden record para dedup cross-portal)
- `similarity_score` — Score de similitud (0-1)
- `match_reasons` — JSONB: qué campos coincidieron (geo, precio, área, dirección)
- `status` — pending, confirmed_duplicate, false_positive
- `reviewed_by` — Quién revisó
- `reviewed_at`
- `created_at`

---

### `cbre_listings`

Staging table para propiedades scrapeadas de CBRE México (`propiedadescbre.net`). ✅ En producción (203 propiedades).

- PK compuesto: `(listing_id, tenant_id)`
- 40+ columnas: título, precios (renta + venta separados), 5 dimensiones de área, ubicación completa con lat/lng, submarket, building class, delivery condition
- Campos JSONB: `brokers`, `spaces`, `images`, `features`, `raw_data`
- Campos de seguimiento: `is_active`, `scraped_at`, `last_seen_at`, `scrapes_sin_aparecer`
- Campos de archivos: `image_storage_urls`, `pdf_storage_urls` (URLs en Supabase Storage)
- Campos calculados: `price_per_m2` (via trigger), `ubicacion_geo` (geography(POINT) via trigger)
- Triggers: `trg_cbre_populate_geo`, `trg_cbre_log_price_change`, `trg_cbre_price_per_m2`, `trg_cbre_update_last_seen`

> Para el schema completo de columnas ver `docs/scrapers/cbre.md` en el repo `beiqa-scraper`.

---

### `colliers_listings`

Staging table para propiedades scrapeadas de Colliers México (`colliers.com/en-mx`). ✅ En producción (240 propiedades).

- PK compuesto: `(listing_id, tenant_id)`
- Similar a `cbre_listings` con campos específicos: `ceiling_height_m`, `docks`, `certifications` (JSONB)
- Campos JSONB: `brokers`, `images`, `raw_data`
- Campos de seguimiento: `is_active`, `scraped_at`, `last_seen_at`, `scrapes_sin_aparecer`
- Campos de archivos: `image_storage_urls`, `pdf_storage_urls`
- Campos calculados: `price_per_m2` (via trigger), `ubicacion_geo` (geography(POINT) via trigger)
- Triggers: `trg_colliers_populate_geo`, `trg_colliers_log_price_change`, `trg_colliers_price_per_m2`, `trg_colliers_update_last_seen`

> Para el schema completo ver `docs/scrapers/colliers.md` en el repo `beiqa-scraper`.

---

### `finsa_listings`

Staging table para propiedades scrapeadas de FINSA (`finsa.net`). ✅ En producción (713 propiedades). El scraper más maduro, con validación de ciclo de vida y H3 indexing.

- PK: `finsa_space_id` (sin `tenant_id`)
- 35+ columnas: áreas en m² y sqft, specs técnicas (HVAC, fire protection, electrical substation), certificaciones LEED/WELL/EDGE como booleans
- 7 índices H3: `h3_index_res5` a `h3_index_res11`
- Validación de ciclo de vida: `scrapes_sin_aparecer` (integer) — después de 2 ausencias consecutivas, `is_active = false`
- `flyer_url` — URL del PDF/flyer en Supabase Storage
- Triggers: `trg_finsa_populate_geo`, `trg_finsa_log_price_change`, `trg_finsa_price_per_m2`, `trg_finsa_update_last_seen`, `trg_finsa_generate_posting_id`

> Para el schema completo ver `docs/scrapers/finsa.md` en el repo `beiqa-scraper`.

---

### `pincali_listings`

Staging table para propiedades scrapeadas de Pincali. ✅ En producción (1,761 propiedades).

- PK compuesto: `(listing_id, tenant_id)`
- 58 columnas: título, descripción, precios (amount, currency, per_m2, period, text), 3 dimensiones de área (total, built, land), ubicación completa (address, neighborhood, municipality, city, state, zip_code)
- Campos geográficos: `latitude`, `longitude`, `ubicacion_geo` (geography(POINT) via trigger), `google_maps_url`
- Campos industriales: `ceiling_height_m`, `docks`, `front_m`, `length_m`, `year_built`, `floors`, `parking_spaces`
- Campos de broker: `publisher_name`, `publisher_company`, `broker_photo_url`, `broker_profile_url`, `broker_id` (FK)
- Campos JSONB: `images`, `features`, `raw_data`
- Campos de archivos: `image_storage_urls` (array, URLs en Supabase Storage bucket `pincali-images`)
- 7 índices H3: `h3_index_res5` a `h3_index_res11`
- Campos de seguimiento: `is_active`, `scraped_at`, `last_seen_at`, `scrapes_sin_aparecer`, `extraction_method` (default: 'firecrawl')
- Campos de relación: `busqueda_id` (FK), `hubspot_deal_id`
- Triggers: `trg_pincali_populate_geo`, `trg_pincali_log_price_change`, `trg_pincali_price_per_m2`, `trg_pincali_update_last_seen`

---

### `cliente_propiedades`

Shortlist de propiedades enviadas a clientes. Tracking de qué propiedades se enviaron a qué contacto de HubSpot.

- `id` — UUID, primary key
- `tenant_id` — FK a `tenants`
- `hubspot_contact_id` — ID del contacto en HubSpot al que se envió
- `posting_id` — ID de la propiedad enviada
- `status` — Estado: 'enviada' (default), 'vista', 'descartada', etc.
- `notas` — Notas adicionales sobre la propiedad para este cliente
- `created_at`, `updated_at`

---

### `cache_api_responses`

Cache de responses de APIs externas (Google Places, INEGI DENUE, Google Geocoding). ✅ Tabla creada (0 rows).

- `id` — UUID, primary key
- `tenant_id` — FK a `tenants`
- `api_source` — Origen: 'google_places', 'inegi_denue', 'google_geocoding'
- `query_key` — Hash de los parámetros de la query
- `response_data` — JSONB con la respuesta completa
- `fetched_at` — Timestamp de la consulta
- `ttl_days` — Días de validez del cache (default: 30)
- `h3_index` — Para cachear por zona geográfica

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
| `pincali-images` | Imágenes de propiedades | Pincali |

---

## Triggers

Todas las tablas de listings (inmuebles24, cbre, colliers, finsa, pincali) comparten un patrón consistente de triggers:

### Trigger functions

| Función | Tipo | Descripción |
|---------|------|-------------|
| `populate_geo` | BEFORE INSERT/UPDATE | Genera `geo_point`/`ubicacion_geo` como geography(POINT) desde lat/lng |
| `calculate_price_per_m2` | BEFORE INSERT/UPDATE | Calcula `price_per_m2` automáticamente desde precio y área |
| `log_price_change` | BEFORE UPDATE | Inserta registro en `price_history` cuando cambia el precio |
| `update_last_seen` | BEFORE UPDATE | Actualiza `last_seen_at` en cada upsert |
| `generate_finsa_posting_id` | BEFORE INSERT | Genera posting_id único para FINSA (solo `finsa_listings`) |

> Nota: Cada portal tiene su propia versión prefijada (ej. `log_cbre_price_change`, `log_colliers_price_change`, etc.) pero la lógica es equivalente.

### Triggers por tabla

| Tabla | populate_geo | price_per_m2 | log_price_change | update_last_seen | generate_posting_id |
|-------|:---:|:---:|:---:|:---:|:---:|
| `inmuebles24_listings` | ✅ | ✅ | ✅ (AFTER) | ✅ | — |
| `cbre_listings` | ✅ | ✅ | ✅ | ✅ | — |
| `colliers_listings` | ✅ | ✅ | ✅ | ✅ | — |
| `finsa_listings` | ✅ | ✅ | ✅ | ✅ | ✅ |
| `pincali_listings` | ✅ | ✅ | ✅ | ✅ | — |

---

## RPC Functions (funciones de base de datos)

### Funciones de mercado / analytics

| Función | Parámetros | Descripción |
|---------|-----------|-------------|
| `tendencia_precios` | `p_zona text, p_tipo_operacion text, p_meses int` | Tendencia de precios por zona y tipo de operación |
| `precio_promedio_m2` | `p_zona text, p_tipo_operacion text, p_meses int` | Precio promedio por m² en una zona |
| `resumen_mercado` | `p_zona text` | Resumen de mercado para una zona (precios, inventario, actividad) |

### Funciones de búsqueda

| Función | Parámetros | Descripción |
|---------|-----------|-------------|
| `buscar_listings` | `filtros jsonb` | Búsqueda de listings con filtros dinámicos (tipo, precio, área, etc.) |
| `buscar_por_radio` | `p_lat, p_lng, p_radio_km, filtros jsonb` | Búsqueda geoespacial por radio + filtros opcionales |
| `listings_recientes` | `p_dias int, p_tenant_id uuid` | Listings nuevos o actualizados en los últimos N días |

### Funciones de búsquedas (gestión)

| Función | Parámetros | Descripción |
|---------|-----------|-------------|
| `resumen_busquedas` | `p_tenant_id uuid` | Resumen de búsquedas activas para un tenant |
| `obtener_busquedas_para_rescrape` | — | Busca búsquedas que necesitan re-scraping |
| `detectar_busquedas_atoradas` | `p_timeout_minutos int` | Detecta búsquedas que llevan demasiado tiempo sin completar |

### Funciones de brokers

| Función | Parámetros | Descripción |
|---------|-----------|-------------|
| `broker_stats` | `p_broker_id uuid` | Estadísticas de un broker (listings, portales, actividad) |
| `empresa_broker_stats` | `p_empresa_id uuid` | Estadísticas de una empresa de brokers |
| `upsert_broker` | `p_publisher_id, p_publisher_name, p_whatsapp, p_portal_source, p_tenant_id` | Crea o actualiza un broker desde datos de scraping |

### Funciones de ciclo de vida

| Función | Parámetros | Descripción |
|---------|-----------|-------------|
| `increment_scrapes_sin_aparecer_finsa` | `space_ids text[]` | Incrementa contador de ausencias para FINSA |
| `marcar_inactivos` | `p_portal text, p_dias int` | Marca como inactivos listings que no aparecen en N días |
| `marcar_inactivos_por_scrapes` | `p_umbral int` | Marca inactivos por número de scrapes sin aparecer |
| `resumen_frescura` | `p_tenant_id uuid` | Resumen de frescura de datos (cuántos listings activos, stale, etc.) |
| `detectar_duplicados` | `p_posting_id text, p_tenant_id uuid` | Detecta posibles duplicados para un listing |

### Funciones de utilidad

| Función | Parámetros | Descripción |
|---------|-----------|-------------|
| `get_tenant_id` | — | Retorna el tenant_id del usuario autenticado (para RLS) |
| `n8n_upsert_listing` | `p jsonb` | ⚠️ **Legacy** (n8n deprecado, ADR-019). Upsert de listing desde JSON. Pendiente de eliminar |

---

## Lo que está planificado (NO implementado aún)

### Campos H3 para `inmuebles24_listings`
Los H3 indices ya existen en `finsa_listings` y `pincali_listings` (res5-11), pero faltan en `inmuebles24_listings`. Pendiente agregar y backfill.

### Campo para AGEB (INEGI)
```sql
ALTER TABLE inmuebles24_listings ADD COLUMN ageb_id TEXT;
-- Se popula con spatial join cuando se cargue el shapefile de AGEBs
```

### Golden record `properties`
Tabla consolidada de propiedades deduplicadas (ADR-012). Pendiente de crear junto con el pipeline de normalización (Mastra agent).

### Cleanup legacy
- Eliminar función `n8n_upsert_listing` (legacy de n8n, ADR-019)

---

## Resumen de tablas (Marzo 2026)

| Tabla | Rows | Estado | Descripción |
|-------|------|--------|-------------|
| `inmuebles24_listings` | ~24,700 | ✅ Producción | Listings I24 (tabla principal) |
| `pincali_listings` | ~1,760 | ✅ Producción | Listings Pincali |
| `finsa_listings` | ~710 | ✅ Producción | Listings FINSA |
| `colliers_listings` | ~240 | ✅ Producción | Listings Colliers |
| `cbre_listings` | ~200 | ✅ Producción | Listings CBRE |
| `price_history` | ~2,000 | ✅ Producción | Historial de precios (todos los portales) |
| `brokers` | 3 | ✅ Producción | Brokers encontrados |
| `empresas_brokers` | 3 | ✅ Producción | Empresas de brokers |
| `tenants` | 1 | ✅ Producción | Clientes Beiqa |
| `usuarios` | 4 | ✅ Producción | Usuarios del sistema |
| `busquedas` | 1 | ✅ Producción | Búsquedas de propiedades |
| `busqueda_listings` | 3 | ✅ Producción | Resultados de búsquedas |
| `cliente_propiedades` | 0 | ✅ Creada | Shortlist de props enviadas a clientes |
| `cache_api_responses` | 0 | ✅ Creada | Cache de APIs externas |
| `portales_config` | 2 | ✅ Producción | Config de portales |
| `possible_duplicates` | 0 | ✅ Creada | Duplicados detectados |
| `error_logs` | 0 | ✅ Creada | Logs de errores |

---

*Documento creado: 2026-02-24 | Actualizado: 2026-03-09 (sync completo con Supabase: PG 17, 32 migrations, pincali_listings, cliente_propiedades, cache_api_responses, triggers y RPCs expandidos)*
*Para el SQL completo ver: `supabase/migrations/` en el repo de código*
