# Scraper

**Fase del proyecto**: Fase 1 — Scrapers & Inventario
**Estado**: 🟢 En desarrollo
**Owner**: Fabrizio

---

## Descripcion

El modulo de Scraper es el motor de captura automatizada de listados inmobiliarios comerciales e industriales en Mexico. Extrae propiedades de 4 portales inmobiliarios, las procesa a traves de un pipeline de normalizacion, enriquecimiento y validacion, y las almacena en HubSpot (CRM) y Supabase (base de datos operativa).

El pipeline incluye: extraccion de datos del portal, normalizacion basica de campos, parsing inteligente de descripciones via LLM, geocodificacion con Google APIs, deteccion de anomalias, y escritura dual a HubSpot + Supabase con logica de alta/actualizacion/baja.

---

## Estado Actual

### En produccion
- **Inmuebles24** via Apify: ~30K propiedades en Supabase al 24 feb 2026
- **n8n Cloud**: orquestacion actual (webhook Apify → n8n → Supabase), cron semanal + trigger manual desde Slack
- **Clay** como orquestador de enriquecimiento: parseo de descripciones con LLM, geocodificacion via Google API, normalizacion de campos, escritura a HubSpot
- **Deduplicacion**: tabla `possible_duplicates` + RPC en progreso
- **AI extraction**: Trigger.dev + Claude API para poblar `property_features`

### Por desarrollar
- Scrapers custom para Pincali, CBRE y Colliers via **TriggerDev** (TypeScript)
- Pipeline completo en TriggerDev reemplazando a Clay y n8n
- Escritura dual a HubSpot + Supabase
- Migracion del flujo de Inmuebles24 de Clay/n8n a TriggerDev

---

## Portales

| Portal | Extraccion | Procesamiento | Destino | Frecuencia | Volumen est. | Estado |
|--------|-----------|---------------|---------|------------|-------------|--------|
| Inmuebles24 | Apify | n8n + Clay (transitorio → TriggerDev) | HubSpot + Supabase | Mensual | ~30-40k | ✅ Produccion |
| Pincali (pincali.com) | TriggerDev | TriggerDev | HubSpot + Supabase | Mensual | ~30-40k | 🔴 Por desarrollar |
| CBRE (seccion Mexico) | TriggerDev | TriggerDev | HubSpot + Supabase | Semanal | ~cientos | 🔴 Por desarrollar |
| Colliers (seccion Mexico) | TriggerDev | TriggerDev | HubSpot + Supabase | Semanal | ~cientos | 🔴 Por desarrollar |

**Tipos de portal**:
- **Inmuebles24 y Pincali**: Portales inmobiliarios multi-broker. Multiples inmobiliarias y personas fisicas publican propiedades. Paginacion clasica con filtros, paginas de listado y detalle.
- **CBRE y Colliers**: Sitios corporativos de brokers internacionales. Solo listan sus propias propiedades en la seccion de Mexico (ej. colliers.com/es-mx/properties).

---

## Pipeline de Datos (TriggerDev)

Cada portal tiene su propio job independiente en TriggerDev. El pipeline por job es:

```
1. Extraccion          → Scraping del portal (paginacion, listados, detalle)
2. Normalizacion       → Precios→MXN, superficies→m², tipos→catalogo interno
3. Parsing LLM         → Extraer campos ocultos en descripcion (m², altura techo,
                          seguridad, antiguedad, amenidades) usando modelo costo-eficiente
4. Geocodificacion     → Google Geocoding API → coordenadas WGS84
5. Asignacion geo      → H3 hexagono + AGEB (para integracion futura con GIS)
6. Extraccion broker   → Nombre, telefono, WhatsApp, empresa (logica varia por portal)
7. Validacion          → Deteccion de anomalias (coordenadas falsas, precios atipicos,
                          datos inconsistentes, persona vs empresa)
8. Check existencia    → Verificar en HubSpot + Supabase si propiedad y broker ya existen
9. Push propiedades    → Crear/actualizar en Supabase + HubSpot (custom object "Propiedad")
                          Telefono del anunciante SIEMPRE en el registro de propiedad
10. Push broker/inmob  → Logica diferenciada por tipo de portal (ver abajo)
11. Logica de estados  → Alta / actualizacion / baja de propiedades
12. Imagenes           → Descarga y almacenamiento en Supabase Storage
13. Notificaciones     → Alertas a Slack en caso de errores o intervencion humana
```

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
| **n8n Cloud** | Orquestacion actual (webhook Apify → Supabase), cron + Slack trigger | ✅ Produccion (transitorio) |
| **Clay** | Enriquecimiento transitorio (sera reemplazado por TriggerDev) | ✅ Produccion (transitorio) |
| **TriggerDev** (TypeScript) | Orquestacion y ejecucion de jobs de scraping (futuro) | 🟡 En setup |
| **Trigger.dev + Claude API** | AI extraction de property_features | 🟡 En progreso |
| **Google Geocoding API** | Coordenadas a partir de direcciones | ✅ En uso via Clay |
| **LLM API** (Haiku/GPT-4o-mini) | Parsing de descripciones y deteccion de anomalias | ✅ En uso via Clay |
| **Supabase** (PostgreSQL + PostGIS) | Base de datos operativa principal | ✅ ~60K registros |
| **Supabase Storage** | Almacenamiento de imagenes de propiedades | 🔴 Por configurar |
| **HubSpot** | CRM — custom object "Propiedad", Contacts, Companies | ✅ En uso |
| **Slack** | Notificaciones de errores e intervencion humana | ✅ Canal existente |

---

## Roadmap

### Fase 1 — Actual
- ✅ Inmuebles24 via Apify + n8n + Clay en produccion
- ✅ ~30K propiedades en Hubspot
- ✅ Enriquecimiento y geocodificacion via Clay
- ✅ Escritura a HubSpot
- 🟡 ~30K propiedades en Supabase
- 🟡 Deduplicacion activa (`possible_duplicates` + RPC)
- 🟡 AI extraction (Trigger.dev + Claude) para `property_features`

### Fase 2 — En progreso
- 🔴 Investigacion de portales (CBRE, Colliers, Pincali)
- 🔴 Infraestructura compartida de TriggerDev (modulos de escritura, check existencia, Slack, etc.)
- 🔴 Desarrollo de 3 scrapers en paralelo
- 🔴 Escritura dual HubSpot + Supabase

### Fase 3 — Futuro
- Migracion de Inmuebles24 de Clay/n8n a TriggerDev
- Desactivacion de Clay y n8n
- Dashboard de ejecucion

---

## Objetivos

1. Automatizar la captura de propiedades comerciales/industriales desde 4 portales inmobiliarios mexicanos
2. Reducir en un 50% el tiempo que el equipo dedica a busqueda y registro de propiedades
3. Mantener datos frescos: listados actualizados segun la frecuencia de cada portal (semanal o mensual)
4. Escritura dual consistente a HubSpot + Supabase con logica de alta/actualizacion/baja

---

## Metricas de Exito / KPIs

| Metrica | Target | Estado actual |
|---------|--------|--------------|
| Propiedades en base de datos | >30,000 listados activos | ✅ ~63K al 24 feb |
| Portales soportados | 4 portales en produccion | 🟡 1 activo (Inmuebles24) |
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
| Pipeline n8n + Clay | Normalizacion + upsert en Supabase + enriquecimiento + HubSpot | ✅ Produccion |
| Cron + Slack trigger | Ejecucion automatica semanal + manual desde Slack | ✅ Activo |
| Deduplicacion | `possible_duplicates` + RPC + reviews | 🟡 En progreso |
| AI extraction | Trigger.dev extrae `property_features` de descripciones | 🟡 En progreso |
| Infraestructura TriggerDev | Modulos compartidos: escritura Supabase/HS, geocoding, LLM, Slack, imagenes | 🔴 |
| Scraper CBRE | Job TriggerDev con pipeline completo, cron semanal | 🔴 |
| Scraper Colliers | Job TriggerDev con pipeline completo, cron semanal | 🔴 |
| Scraper Pincali | Job TriggerDev con pipeline completo, cron mensual | 🔴 |
| Migracion I24 a TriggerDev | Replicar logica de Clay/n8n en TriggerDev, desactivar Clay | 🔴 |

---

## Dependencias

### Necesita (upstream)
- **Base de datos (Arquitectura)** → Esquema de tablas para propiedades, brokers, historial, y metadatos
- **Supabase** → Instancia configurada con PostGIS y Storage
- **HubSpot** → Custom object "Propiedad" con campos mapeados
- **Google Cloud** → API key para Geocoding API

### Depende de este (downstream)
- **Internal App** → Consume los listados capturados
- **Market Intelligence** → Usa datos historicos de precios y disponibilidad
- **AI Brain** → Utiliza descripciones y atributos para matching y NLP
- **Data (Normalization)** → Recibe datos para deduplicacion cross-portal y limpieza profunda

---

## Riesgos Clave

| # | Riesgo | Impacto | Probabilidad | Mitigacion |
|---|--------|---------|--------------|------------|
| 1 | Cambios en HTML/estructura de portales | Alto | Alta | TriggerDev: alertas de fallo, tests de estructura, selectores modulares. Notificacion Slack inmediata. Apify maneja parte de esto para I24 |
| 2 | Rate limiting o bloqueo de IP | Medio | Alta | Delays aleatorios, rotacion de user-agents, TriggerDev maneja retries con backoff exponencial. Apify maneja proxies para I24 |
| 3 | Datos inconsistentes entre portales | Medio | Alta | Validacion en pipeline, deteccion de anomalias, flag para revision humana |
| 4 | Costos de APIs externas (Google Geocoding, LLM) | Medio | Media | Cache de geocodificacion, modelo LLM costo-eficiente (Haiku/GPT-4o-mini), monitoreo de costos |
| 5 | Fallo en dual-write (HubSpot o Supabase falla) | Alto | Baja | Retry automatico, logs detallados, notificacion Slack, reconciliacion manual si necesario |
| 6 | Volumen alto de Pincali (~30-40k) satura TriggerDev | Medio | Media | Procesamiento en lotes, concurrencia controlada, ejecucion mensual en horario de bajo trafico |

---

## Documentos del Modulo

- [Product Questions](./Product-Questions.md) — Cuestionario de discovery (parcialmente respondido)
- [Requirements](./Requirements.md) — Capacidades y criterios de aceptacion
- [Research/](./Research/) — Investigacion tecnica y estrategia de adquisicion
- [EXECUTION-LOG.md](./EXECUTION-LOG.md) — Log de ejecuciones del scraper
