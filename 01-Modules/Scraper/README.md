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
- **Inmuebles24**: ~30K propiedades en Supabase via Apify — **en migración a Trigger.dev+Firecrawl** (Sprint 1-2, eliminar Apify+Clay)
- **Deduplicacion**: tabla `possible_duplicates` + RPC en progreso

### En desarrollo (Sprint 1)
- **Pincali**: scraping funciona (Firecrawl stealth + LLM, cron lunes 7am UTC), persistencia a Supabase **pendiente**
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
| Inmuebles24 | Apify → migrando a Firecrawl (Sprint 1-2) | Supabase (`inmuebles24_listings`) | Via Apify externo | ~30K | ⚠️ Migrando |
| Pincali | Firecrawl stealth (~9 créditos/prop) | Supabase (pendiente persist) | Lunes 7am | ~30-40K | 🟡 Persist pendiente |
| Cushman | Por definir | Supabase | TBD | TBD | 🔴 Sprint 3+ |
| Proximity Parks | Por definir | Supabase | TBD | TBD | 🔴 Sprint 3+ |

**Tipos de portal**:
- **Inmuebles24 y Pincali**: Portales inmobiliarios multi-broker. Multiples inmobiliarias y personas fisicas publican propiedades. Paginacion clasica con filtros, paginas de listado y detalle.
- **CBRE y Colliers**: Sitios corporativos de brokers internacionales. Solo listan sus propias propiedades en la seccion de Mexico (ej. colliers.com/es-mx/properties).
- **FinSA**: Portal de desarrollador industrial con API interna JSON. Scraper usa API directa (sin Firecrawl), H3 indexing, validación de ciclo de vida y notificaciones Slack.
- **Proximity Parks**: Portal de parques industriales planificado.

---

## Pipeline de Datos (TriggerDev)

Cada portal tiene su propio job independiente en TriggerDev. El pipeline por job se enfoca en scraping y persistencia:

```
1. Extraccion          → Scraping del portal via Firecrawl + Browserbase
                          (paginacion, listados, detalle)
2. Normalizacion       → Precios→MXN, superficies→m², tipos→catalogo interno
                          (normalizacion basica pre-guardado)
3. Check existencia    → Verificar en HubSpot + Supabase si propiedad y broker ya existen
4. Push propiedades    → Crear/actualizar en Supabase + HubSpot (custom object "Propiedad")
                          Telefono del anunciante SIEMPRE en el registro de propiedad
5. Push broker/inmob   → Logica diferenciada por tipo de portal (ver abajo)
6. Logica de estados   → Alta / actualizacion / baja de propiedades
7. Imagenes            → Descarga y almacenamiento en Supabase Storage
8. Notificaciones      → Alertas a Slack en caso de errores o intervencion humana
```

> **Enriquecimiento AI (Mastra)**: Los pasos de parsing LLM, geocodificacion, asignacion H3/AGEB, extraccion de broker avanzada y deteccion de anomalias se ejecutan como agentes de Mastra independientes, no dentro del job de TriggerDev. Ver [Agent-Architecture.md](../../02-Architecture/Agent-Architecture.md).

### Logica de estados de propiedades

| Escenario | Accion |
|-----------|--------|
| Nueva propiedad | Crear en Supabase + HubSpot, asociar a inmobiliaria/contacto segun portal |
| Propiedad existente sin cambios | Actualizar solo fecha de scraping |
| Propiedad existente con cambios | Actualizar campos modificados en ambos destinos |
| Propiedad no encontrada | Marcar como no disponible de inmediato en ambos destinos |

### Logica de brokers/inmobiliarias (diferente por tipo de portal)

**Inmuebles24 y Pincali** (portales multi-broker):
- Se extrae: nombre de la empresa/inmobiliaria anunciante + telefono
- El telefono se guarda directamente EN la propiedad (campo del listing)
- La empresa se registra como Company en HubSpot y se asocia a la propiedad
- **NO se crean Contacts individuales** — solo Company
- **NO se guardan brokers individuales en Supabase** — solo referencia a la empresa

**CBRE y Colliers** (portales corporativos):
- La inmobiliaria siempre es la misma (CBRE o Colliers)
- Se extrae: nombre del agente/contacto individual + telefono + datos de contacto
- Se crea/actualiza Contact en HubSpot asociado a la Company (CBRE o Colliers)
- Se asocia la propiedad al Contact individual
- El telefono del contacto tambien se guarda en la propiedad directamente

**En todos los casos**:
- El telefono esta SIEMPRE en el registro de la propiedad (para acceso rapido)
- Asociaciones en HubSpot: Propiedad ↔ Company (siempre), Propiedad ↔ Contact (solo CBRE/Colliers)

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
| **Supabase** (PostgreSQL + PostGIS) | Base de datos operativa principal | ✅ ~30K registros (I24) |
| **Supabase Storage** | Almacenamiento de imágenes y PDFs (5 buckets: cbre-images, cbre-pdfs, colliers-images, colliers-pdfs, finsa-flyers) | ✅ Activo |
| **HubSpot** | CRM — custom object "Propiedad", Contacts, Companies | ✅ En uso |
| **Slack** | Notificaciones de errores e intervencion humana | ✅ Canal existente |

---

## Roadmap

### Completado
- ✅ Inmuebles24 via Apify: ~30K propiedades en Supabase
- ✅ CBRE: scraper completo con persistencia + Storage + cron
- ✅ Colliers: scraper completo con persistencia + Storage + cron
- ✅ FINSA: scraper completo con H3, validación, Slack, flyers
- ✅ Escritura a HubSpot
- 🟡 Deduplicacion (`possible_duplicates` + RPC en progreso)

### Sprint 1 — Actual (Mar 2-15)
- 🟡 Pincali: persistencia a Supabase
- 🟡 I24: pruebas de migración a Trigger.dev+Firecrawl
- 🟡 Módulo Slack compartido (Issue #18)
- 🟡 Golden record migrations (5 tablas)
- 🟡 Detección de anomalías

### Sprint 2 (Mar 16-29)
- I24: migración completa (config + push + lógica) — eliminar Apify+Clay
- Validación/desactivación para CBRE y Colliers (Issue #15)
- Staging tables Colliers y Pincali (si no se completaron en Sprint 1)

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
| Propiedades en base de datos | >30,000 listados activos | ✅ ~30K en I24 al 24 feb |
| Portales soportados | 6 portales en produccion | 🟡 4 activos (I24, CBRE, Colliers, FINSA) + 1 parcial (Pincali) |
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
| Scraper Pincali | Firecrawl stealth, cron lunes 7am. Persist pendiente Sprint 1 | 🟡 Parcial |
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
