# Scraper

**Sprint del proyecto**: Sprint 1+ — Scrapers & Inventario
**Estado**: 🟢 En desarrollo
**Owner**: Fabrizio
**Repo de código**: [github.com/pablo-beiqa/beiqa-scraper](https://github.com/pablo-beiqa/beiqa-scraper)

---

## Descripcion

El modulo de Scraper es el motor de captura automatizada de listados inmobiliarios comerciales e industriales en Mexico. Extrae propiedades de multiples portales inmobiliarios, las procesa a traves de un pipeline de persistencia y cron via TriggerDev, y las almacena en HubSpot (CRM) y Supabase (base de datos operativa).

El pipeline incluye: extraccion de datos del portal, normalizacion basica de campos, persistencia en Supabase + HubSpot, y programacion de ejecuciones via cron. El enriquecimiento inteligente (parsing de descripciones via LLM, geocodificacion, deteccion de anomalias) se delega a agentes de Mastra como capa separada (ver ADR-021: separacion de responsabilidades).

> **Separacion de responsabilidades (ADR-021)**: TriggerDev se encarga de scraping + persistencia + cron. Mastra se encarga del enriquecimiento AI (normalización profunda, geocodificación, parsing LLM, detección de anomalías). Esta separación mantiene los jobs de scraping simples y predecibles.

---

## Estado Actual

### En produccion
- **CBRE**: scraper completo con persistencia a `cbre_listings`, imágenes y PDFs en Supabase Storage, cron martes 6am UTC. Usa Firecrawl + RSC payload parsing (Next.js)
- **Colliers**: scraper completo con persistencia a `colliers_listings`, archivos en Storage, cron lunes 6am UTC. Usa Firecrawl + LLM JSON extraction + Browserbase para downloads
- **FINSA**: scraper más maduro — API directa (sin Firecrawl), persistencia a `finsa_listings` con H3 indexing (res 5-11), validación de ciclo de vida (`scrapes_sin_aparecer`), notificaciones Slack, flyers PDF. Cron día 1 y 15, 5am UTC
- **Pincali**: scraper completo con persistencia a `pincali_listings` (1,761 propiedades), imágenes en Supabase Storage (`pincali-images`), H3 indexing (res 5-11), cron lunes 7am UTC. Usa Firecrawl stealth + LLM extraction + Browserbase
- **Inmuebles24**: ~25K propiedades en Supabase via Apify — **en migración a Trigger.dev+Firecrawl** (Sprint 1-2, eliminar Apify+Clay)
- **Deduplicacion**: tabla `possible_duplicates` + RPC en progreso

### En desarrollo (Sprint 1)
- **I24 testing**: pruebas de viabilidad técnica y económica para reemplazar Apify con Trigger.dev+Firecrawl
- **Módulo Slack compartido**: notificaciones reutilizables para todos los scrapers (Issue #18)
- **Golden record**: tablas `properties` + soporte por crear

### Por desarrollar (Sprint 3+)
- **Cushman**: scraper planificado
- **Proximity Parks**: scraper planificado
- **I24 migración completa**: Sprint 2 — config + push + lógica, eliminar Apify y Clay

### Pain points operativos
- **Costos impredecibles**: créditos de Firecrawl/Browserbase varían según cambios en portales y volumen
- **Observabilidad deficiente**: no hay trazabilidad centralizada de ejecuciones de todos los scrapers
- **Alertas incompletas**: solo FINSA tiene Slack post-scrape; los demás no notifican errores
- **Timeout planning**: timeouts y fallos necesitan mejor configuración por scraper

---

## Portales

| Portal | Motor de scraping | Destino | Cron (UTC) | Volumen est. | Estado |
|--------|-------------------|---------|-----------|-------------|--------|
| CBRE | Firecrawl (RSC parsing, sin LLM) | Supabase (`cbre_listings`) + Storage | Martes 6am | ~cientos | ✅ Produccion |
| Colliers | Firecrawl (LLM extraction) + Browserbase | Supabase (`colliers_listings`) + Storage | Lunes 6am | ~cientos | ✅ Produccion |
| FinSA | API directa (sin Firecrawl, $0 créditos) | Supabase (`finsa_listings`) + Storage | Día 1 y 15, 5am | ~cientos | ✅ Produccion |
| Inmuebles24 | Apify → migrando a Firecrawl (Sprint 1-2) | Supabase (`inmuebles24_listings`) | Via Apify externo | ~25K | ⚠️ Migrando |
| Pincali | Firecrawl stealth (~9 créditos/prop) + Browserbase | Supabase (`pincali_listings`) + Storage | Lunes 7am | ~1,760 | ✅ Produccion |
| Cushman | Por definir | Supabase | TBD | TBD | 🔴 Sprint 3+ |
| Proximity Parks | Por definir | Supabase | TBD | TBD | 🔴 Sprint 3+ |

**Tipos de portal**:
- **Inmuebles24 y Pincali**: Portales inmobiliarios multi-broker. Multiples inmobiliarias y personas fisicas publican propiedades. Paginacion clasica con filtros, paginas de listado y detalle.
- **CBRE y Colliers**: Sitios corporativos de brokers internacionales. Solo listan sus propias propiedades en la seccion de Mexico (ej. colliers.com/es-mx/properties).
- **FinSA**: Portal de desarrollador industrial con API interna JSON. Scraper usa API directa (sin Firecrawl), H3 indexing, validación de ciclo de vida y notificaciones Slack.
- **Proximity Parks**: Portal de parques industriales planificado.

---

## Pipeline de Datos (TriggerDev)

Cada portal tiene su propio job independiente en TriggerDev. El pipeline por job se enfoca en scraping y persistencia directa a Supabase:

```
1. Discovery           → Paginar URLs de categorías del portal
                          Guardar en discovered-urls.json (Pincali) o en memoria
2. Check existencia    → Cargar listing IDs existentes desde Supabase
                          Separar: nuevas (scrape) vs existentes (solo touchLastSeen)
3. Scrape nuevas       → Firecrawl stealth + LLM JSON extraction (9 créditos/prop)
                          Batch de 10 propiedades con idempotency keys
4. Upsert a Supabase   → Crear/actualizar en tabla de staging del portal
                          Insertar precio inicial en price_history (solo primer insert)
5. Download archivos   → Descargar imágenes/PDFs → Supabase Storage (bucket por portal)
6. Update storage URLs → Actualizar image_storage_urls / pdf_storage_urls en DB
7. Notificaciones      → Alertas a Slack en caso de errores (solo FINSA por ahora)
```

> **Enriquecimiento AI (Mastra)**: Los pasos de parsing LLM avanzado, geocodificación, asignación H3/AGEB, y detección de anomalías se ejecutan como agentes de Mastra independientes, no dentro del job de TriggerDev. Ver [Agent-Architecture.md](../../02-Architecture/Agent-Architecture.md).

### Destino de datos por portal

| Portal | Supabase (staging) | Supabase Storage | HubSpot |
|--------|-------------------|------------------|---------|
| Pincali | `pincali_listings` (upsert) | `pincali-images` | No |
| CBRE | `cbre_listings` (upsert) | `cbre-files` (imgs + PDFs) | No |
| Colliers | `colliers_listings` (upsert) | `colliers-files` (imgs + PDFs) | No |
| FINSA | `finsa_listings` (upsert) | `finsa-flyers` (PDFs) | No |
| I24 (Apify) | `inmuebles24_listings` | — | Sí (legacy via Clay) |

> **Nota**: La integración con HubSpot es legacy del pipeline I24/Apify+Clay. Los scrapers de Trigger.dev escriben **exclusivamente a Supabase**. La sincronización a HubSpot se manejará como paso separado cuando se implemente.

### Lógica de estados de propiedades

| Escenario | Acción |
|-----------|--------|
| Nueva propiedad | Upsert en tabla de staging + insertar precio inicial en `price_history` |
| Propiedad existente (re-encontrada) | Solo actualizar `last_seen_at` (touchLastSeen — 0 créditos Firecrawl) |
| Propiedad existente con cambio de precio | Trigger de DB inserta automáticamente en `price_history` |
| Propiedad no encontrada en N scrapes | `scrapes_sin_aparecer` incrementa. FINSA: 2 ausencias → `is_active = false`. Otros portales: pendiente |

### Lógica de brokers/inmobiliarias

**Pincali** (portal multi-broker):
- Se extrae: nombre del broker + company via regex HTML (más confiable que LLM)
- Se guardan en `publisher_name`, `publisher_company`, `broker_photo_url`, `broker_profile_url` directamente en `pincali_listings`
- **No se crean registros separados en `brokers`** — datos inline en el listing

**CBRE y Colliers** (portales corporativos):
- Se extrae: array de brokers como JSONB en el listing
- Datos de contacto (nombre, email, teléfono) almacenados en campo `brokers` JSONB

**FINSA** (API directa):
- No extrae datos de broker (portal de desarrollador, no hay agentes individuales)

---

## Stack Tecnologico

| Herramienta | Uso | Estado |
|-------------|-----|--------|
| **Apify** | Extraccion de Inmuebles24 (actor pre-construido) — **migrando a Trigger.dev+Firecrawl Sprint 1-2** | ⚠️ Migrando |
| **TriggerDev** (TypeScript) | Plataforma primaria de ejecucion: scraping, persistencia, cron | ✅ Activo |
| **Firecrawl** | Motor de scraping HTTP + LLM extraction para portales custom | ✅ Activo |
| **Browserbase** | Cloud browser sessions para portales que requieren JS rendering | ✅ Activo |
| **Mastra** | Enriquecimiento AI (agentes de normalizacion, geocodificacion, parsing) | 🟢 En implementacion |
| **Trigger.dev + Claude API** | AI extraction de property_features | 🟡 En progreso |
| **Google Geocoding API** | Coordenadas a partir de direcciones (via Mastra tool) | ✅ En uso |
| **LLM API** (via Mastra) | Parsing de descripciones y deteccion de anomalias | 🟡 En implementacion |
| **Supabase** (PostgreSQL 17 + PostGIS) | Base de datos operativa principal | ✅ ~25K registros (I24) + ~3K otros portales |
| **Supabase Storage** | Almacenamiento de imágenes y PDFs (6 buckets: cbre-images, cbre-pdfs, colliers-images, colliers-pdfs, finsa-flyers, pincali-images) | ✅ Activo |
| **HubSpot** | CRM — custom object "Propiedad", Contacts, Companies | ✅ En uso |
| **Slack** | Notificaciones de errores e intervencion humana | ✅ Canal existente |

---

## Roadmap

### Completado
- ✅ Inmuebles24 via Apify: ~25K propiedades en Supabase
- ✅ Pincali: scraper completo con persistencia + Storage + H3 + cron
- ✅ CBRE: scraper completo con persistencia + Storage + cron
- ✅ Colliers: scraper completo con persistencia + Storage + cron
- ✅ FINSA: scraper completo con H3, validación, Slack, flyers
- ✅ Escritura a HubSpot
- 🟡 Deduplicacion (`possible_duplicates` + RPC en progreso)

### Sprint 1 — Actual (Mar 2-15)
- 🟡 I24: pruebas de migración a Trigger.dev+Firecrawl
- 🟡 Módulo Slack compartido (Issue #18)
- 🟡 Golden record migrations (5 tablas)
- 🟡 Detección de anomalías

### Sprint 2 (Mar 16-29)
- I24: migración completa (config + push + lógica) — eliminar Apify+Clay
- Validación/desactivación para CBRE y Colliers (Issue #15)
- Staging tables completadas (colliers_listings, pincali_listings ya en producción)

### Sprint 3+ — Futuro
- Cushman scraper
- Proximity Parks scraper
- Dashboard de ejecución y observabilidad

---

## Objetivos

1. Automatizar la captura de propiedades comerciales/industriales desde multiples portales inmobiliarios mexicanos
2. Reducir en un 50% el tiempo que el equipo dedica a busqueda y registro de propiedades
3. Mantener datos frescos: listados actualizados segun la frecuencia de cada portal (semanal o mensual)
4. Escritura dual consistente a HubSpot + Supabase con logica de alta/actualizacion/baja

---

## Metricas de Exito / KPIs

| Metrica | Target | Estado actual |
|---------|--------|--------------|
| Propiedades en base de datos | >30,000 listados activos | ✅ ~25K en I24 + ~3K en otros portales |
| Portales soportados | 6 portales en produccion | ✅ 5 activos (I24, CBRE, Colliers, FINSA, Pincali) |
| Tasa de exito de ejecucion | >=95% sin error critico | ✅ Monitoreado via Slack + error_logs |
| Consistencia dual-write | 100% propiedades en ambos destinos | 🔴 Por implementar |
| Anomalias detectadas | <5% de propiedades con anomalias sin resolver | 🔴 Por implementar |
| property_features pobladas | >80% de listings | 🟡 Trigger.dev + Claude en progreso |
| Tiempo de busqueda manual ahorrado | Reduccion del 50% vs baseline | Encuesta al equipo antes/despues |

---

## Entregables Clave

| Entregable | Descripcion | Estado |
|-----------|-------------|--------|
| Apify actor Inmuebles24 | Extraccion de propiedades comerciales/industriales | ✅ Activo |
| FinSA scraper | API directa + H3 + validación + Slack + flyers en Storage | ✅ Producción |
| Cron + Slack trigger | Ejecucion automatica semanal + manual desde Slack | ✅ Activo |
| Deduplicacion | `possible_duplicates` + RPC + reviews | 🟡 En progreso |
| AI extraction | Trigger.dev extrae `property_features` de descripciones | 🟡 En progreso |
| Infraestructura TriggerDev | Módulos compartidos: Supabase client, Browserbase sessions | ✅ Activo |
| Scraper CBRE | Firecrawl + RSC parsing, cron martes 6am, Storage | ✅ Producción |
| Scraper Colliers | Firecrawl + LLM + Browserbase, cron lunes 6am, Storage | ✅ Producción |
| Scraper Pincali | Firecrawl stealth + Browserbase, cron lunes 7am, Storage (pincali-images), H3 | ✅ Producción |
| Módulo Slack compartido | Notificaciones reutilizables (success/error/anomalías) | 🔴 Sprint 1 |
| Migracion I24 a TriggerDev | Reemplazar Apify+Clay por Trigger.dev+Firecrawl | 🟡 Sprint 1-2 |
| Scraper Cushman | Por desarrollar | 🔴 Sprint 3+ |
| Scraper Proximity Parks | Por desarrollar | 🔴 Sprint 3+ |

---

## Dependencias

### Necesita (upstream)
- **Base de datos (Arquitectura)** → Esquema de tablas para propiedades, brokers, historial, y metadatos
- **Supabase** → Instancia configurada con PostGIS y Storage
- **HubSpot** → Custom object "Propiedad" con campos mapeados
- **Google Cloud** → API key para Geocoding API
- **Mastra** → Agentes de enriquecimiento (Data Normalization Agent, Address Enrichment Agent)

### Depende de este (downstream)
- **Internal App** → Consume los listados capturados
- **Market Intelligence** → Usa datos historicos de precios y disponibilidad
- **AI Brain** → Utiliza descripciones y atributos para matching y NLP
- **Data (Normalization)** → Recibe datos para deduplicacion cross-portal y limpieza profunda via Mastra agents

---

## Riesgos Clave

| # | Riesgo | Impacto | Probabilidad | Mitigacion |
|---|--------|---------|--------------|------------|
| 1 | Cambios en HTML/estructura de portales | Alto | Alta | TriggerDev: alertas de fallo, tests de estructura, selectores modulares. Notificacion Slack inmediata. Apify maneja parte de esto para I24. Firecrawl con LLM extraction es mas resiliente a cambios de HTML |
| 2 | Rate limiting o bloqueo de IP | Medio | Alta | Delays aleatorios, rotacion de user-agents, TriggerDev maneja retries con backoff exponencial. Apify maneja proxies para I24. Browserbase provee cloud browser sessions |
| 3 | Datos inconsistentes entre portales | Medio | Alta | Validacion en pipeline, deteccion de anomalias via Mastra, flag para revision humana |
| 4 | Costos de APIs externas (Google Geocoding, LLM) | Medio | Media | Cache de geocodificacion, modelo LLM costo-eficiente, monitoreo de costos |
| 5 | Fallo en dual-write (HubSpot o Supabase falla) | Alto | Baja | Retry automatico, logs detallados, notificacion Slack, reconciliacion manual si necesario |
| 6 | Volumen alto de Pincali (~30-40k) satura TriggerDev | Medio | Media | Procesamiento en lotes, concurrencia controlada, ejecucion mensual en horario de bajo trafico |

---

## Documentos del Modulo

- [Product Questions](./Product-Questions.md) — Cuestionario de discovery (parcialmente respondido)
- [Requirements](./Requirements.md) — Capacidades y criterios de aceptacion
- [Research/](./Research/) — Investigacion tecnica y estrategia de adquisicion
- [EXECUTION-LOG.md](./EXECUTION-LOG.md) — Log de ejecuciones del scraper
