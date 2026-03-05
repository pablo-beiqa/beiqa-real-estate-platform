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
- **Inmuebles24** via Apify: ~30K propiedades en Supabase (I24)
- **FinSA**: Scraper activo en repo separado (`beiqa-scraper`), trabajando con Supabase + PDFs
- **Deduplicacion**: tabla `possible_duplicates` + RPC en progreso
- **AI extraction**: Trigger.dev + Claude API para poblar `property_features`

### Por desarrollar
- Scrapers custom para Pincali, CBRE, Colliers y Proximity Parks via **TriggerDev** (TypeScript) con **Firecrawl + Browserbase** como motores de scraping
- **Proximity Parks**: portal planificado
- Escritura dual a HubSpot + Supabase
- Migracion del flujo de Inmuebles24 a TriggerDev

---

## Portales

| Portal | Extraccion | Motor de scraping | Destino | Frecuencia | Volumen est. | Estado |
|--------|-----------|-------------------|---------|------------|-------------|--------|
| Inmuebles24 | Apify (actor pre-construido) | Apify | HubSpot + Supabase | Mensual | ~30K | ✅ Produccion |
| FinSA | TriggerDev | Firecrawl + Browserbase | Supabase + PDFs | Mensual | TBD | ✅ Activo (repo separado) |
| Pincali (pincali.com) | TriggerDev | Firecrawl + Browserbase | HubSpot + Supabase | Mensual | ~30-40k | 🔴 Por desarrollar |
| CBRE (seccion Mexico) | TriggerDev | Firecrawl + Browserbase | HubSpot + Supabase | Semanal | ~cientos | 🔴 Por desarrollar |
| Colliers (seccion Mexico) | TriggerDev | Firecrawl + Browserbase | HubSpot + Supabase | Semanal | ~cientos | 🔴 Por desarrollar |
| Proximity Parks | TriggerDev | Firecrawl + Browserbase | HubSpot + Supabase | Semanal | TBD | 🔴 Planificado |

**Tipos de portal**:
- **Inmuebles24 y Pincali**: Portales inmobiliarios multi-broker. Multiples inmobiliarias y personas fisicas publican propiedades. Paginacion clasica con filtros, paginas de listado y detalle.
- **CBRE y Colliers**: Sitios corporativos de brokers internacionales. Solo listan sus propias propiedades en la seccion de Mexico (ej. colliers.com/es-mx/properties).
- **FinSA**: Portal de parques industriales con PDFs de fichas tecnicas. Scraper activo en repo separado con Supabase.
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
| **Apify** | Extraccion de Inmuebles24 (actor pre-construido) | ✅ Produccion |
| **TriggerDev** (TypeScript) | Plataforma primaria de ejecucion: scraping, persistencia, cron | ✅ Activo |
| **Firecrawl** | Motor de scraping HTTP + LLM extraction para portales custom | ✅ Activo |
| **Browserbase** | Cloud browser sessions para portales que requieren JS rendering | ✅ Activo |
| **Mastra** | Enriquecimiento AI (agentes de normalizacion, geocodificacion, parsing) | 🟢 En implementacion |
| **Trigger.dev + Claude API** | AI extraction de property_features | 🟡 En progreso |
| **Google Geocoding API** | Coordenadas a partir de direcciones (via Mastra tool) | ✅ En uso |
| **LLM API** (via Mastra) | Parsing de descripciones y deteccion de anomalias | 🟡 En implementacion |
| **Supabase** (PostgreSQL + PostGIS) | Base de datos operativa principal | ✅ ~30K registros (I24) |
| **Supabase Storage** | Almacenamiento de imagenes de propiedades | 🔴 Por configurar |
| **HubSpot** | CRM — custom object "Propiedad", Contacts, Companies | ✅ En uso |
| **Slack** | Notificaciones de errores e intervencion humana | ✅ Canal existente |

---

## Roadmap

### Sprint 1 — Actual
- ✅ Inmuebles24 via Apify en produccion
- ✅ ~30K propiedades en I24 (Supabase)
- ✅ FinSA activo en repo separado (beiqa-scraper)
- ✅ Escritura a HubSpot
- 🟡 Deduplicacion activa (`possible_duplicates` + RPC)
- 🟡 AI extraction (Trigger.dev + Claude) para `property_features`

### Sprint 2 — En progreso
- 🔴 Investigacion de portales (CBRE, Colliers, Pincali, Proximity Parks)
- 🔴 Infraestructura compartida de TriggerDev (modulos de escritura, check existencia, Slack, etc.)
- 🔴 Desarrollo de scrapers custom con Firecrawl + Browserbase
- 🔴 Escritura dual HubSpot + Supabase

### Sprint 3+ — Futuro
- Migracion de Inmuebles24 a TriggerDev
- Dashboard de ejecucion

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
| Portales soportados | 6 portales en produccion | 🟡 2 activos (Inmuebles24, FinSA) |
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
| FinSA scraper | Scraper activo en repo separado (beiqa-scraper) con Supabase + PDFs | ✅ Activo |
| Cron + Slack trigger | Ejecucion automatica semanal + manual desde Slack | ✅ Activo |
| Deduplicacion | `possible_duplicates` + RPC + reviews | 🟡 En progreso |
| AI extraction | Trigger.dev extrae `property_features` de descripciones | 🟡 En progreso |
| Infraestructura TriggerDev | Modulos compartidos: escritura Supabase/HS, Firecrawl, Browserbase, Slack, imagenes | 🔴 |
| Scraper CBRE | Job TriggerDev con Firecrawl + Browserbase, cron semanal | 🔴 |
| Scraper Colliers | Job TriggerDev con Firecrawl + Browserbase, cron semanal | 🔴 |
| Scraper Pincali | Job TriggerDev con Firecrawl + Browserbase, cron mensual | 🔴 |
| Scraper Proximity Parks | Job TriggerDev con Firecrawl + Browserbase | 🔴 Planificado |
| Migracion I24 a TriggerDev | Replicar logica actual en TriggerDev | 🔴 |

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
