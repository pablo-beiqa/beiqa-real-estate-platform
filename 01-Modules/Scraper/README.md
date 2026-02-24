# Scraper

**Fase del proyecto**: Fase 1 — Scrapers & Inventario
**Estado**: 🟢 En desarrollo
**Owner**: Fabrizio

---

## Descripcion

El modulo de Scraper es el motor de captura automatizada de listados inmobiliarios comerciales e industriales en Mexico. Usa **Apify** (actor contratado) para extraer propiedades de portales inmobiliarios de forma periodica y sistematica. Cada ejecucion extrae los datos clave de cada propiedad (superficie, precio, ubicacion, tipo, contacto del broker) y los almacena en Supabase via n8n.

El actor de Apify maneja internamente rate limiting, proxies y anti-bot — no hay codigo Python de scraping. La normalizacion y el upsert en Supabase ocurren en n8n.

---

## Estado actual

| Componente | Estado | Notas |
|-----------|--------|-------|
| Apify actor Inmuebles24 | ✅ Activo | Actor contratado, corriendo |
| ~60K propiedades en Supabase | ✅ Activo | Al 24 feb 2026 |
| Cron semanal (n8n) | ✅ Activo | Ejecucion automatica |
| Trigger manual (Slack) | ✅ Activo | Jeronimo puede disparar desde Slack |
| Normalizacion en n8n | 🟡 En progreso | Todos los campos |
| Deduplicacion activa | 🟡 En progreso | Tabla `possible_duplicates` + RPC |
| AI extraction (Trigger.dev + Claude) | 🟡 En progreso | `property_features` por poblar |
| EasyBroker (segundo portal) | 🔴 Pendiente | Apify actor por contratar |

---

## Objetivos

1. Automatizar la captura de propiedades comerciales/industriales desde al menos 2 portales (Inmuebles24 + EasyBroker).
2. Reducir en un 50% el tiempo que el equipo dedica a la busqueda y registro de propiedades.
3. Mantener la base de datos con informacion fresca: al menos el 80% de los listados activos actualizados en los ultimos 7 dias.

---

## Metricas de Exito / KPIs

| Metrica | Target | Estado actual |
|---------|--------|--------------|
| Propiedades en base de datos | >60,000 listados activos | ✅ ~60K al 24 feb |
| Portales soportados | >=2 en produccion | 🟡 1 activo (Inmuebles24) |
| Frescura de datos | >=80% actualizados en 7 dias | 🟡 En progreso |
| Tasa de exito de ejecucion | >=95% sin error critico | ✅ Monitoreado via Slack + error_logs |
| property_features pobladas | >80% de listings | 🟡 Trigger.dev + Claude en progreso |

---

## Entregables Clave

| Entregable | Descripcion | Estado |
|-----------|-------------|--------|
| Apify actor Inmuebles24 | Extraccion de propiedades comerciales/industriales | ✅ Activo |
| Pipeline n8n | Normalizacion + upsert en Supabase | 🟡 En progreso |
| Cron + Slack trigger | Ejecucion automatica semanal + manual | ✅ Activo |
| Deduplicacion | `possible_duplicates` + RPC + reviews | 🟡 En progreso |
| AI extraction | Trigger.dev extrae `property_features` de descripciones | 🟡 En progreso |
| EasyBroker | Segundo portal de scraping | 🔴 Por iniciar |

---

## Stack del modulo

| Componente | Tecnologia | Notas |
|-----------|-----------|-------|
| Scraping | Apify (actor contratado) | Sin Python, sin Playwright propio |
| Orquestacion | n8n Cloud | Webhook Apify → n8n → Supabase |
| Jobs AI | Trigger.dev + Claude API | Batch extraction de property_features |
| Base de datos | Supabase (inmuebles24_listings) | 14 migrations, trigger populate_geo |
| Monitoring | Slack + error_logs table | Alertas automaticas |

---

## Dependencias

### Necesita (upstream)
- **Base de datos (Arquitectura)** → Esquema de tablas para propiedades, historial de precios, metadatos

### Depende de este (downstream)
- **Internal App** → Consume los listados capturados
- **Market Intelligence** → Datos historicos de precios para analisis
- **AI Brain** → Descripciones y atributos para matching y NLP

---

## Riesgos Clave

| # | Riesgo | Impacto | Probabilidad | Mitigacion |
|---|--------|---------|--------------|------------|
| 1 | Cambios en estructura del portal que rompan el actor de Apify | Medio | Media | Apify maneja gran parte, pero puede requerir actualizar el actor |
| 2 | Rate limiting adicional del portal | Bajo | Baja | Apify maneja proxies y rate limiting internamente |
| 3 | Datos inconsistentes o incompletos | Medio | Alta | Pipeline de validacion en n8n, logs de calidad |
| 4 | EasyBroker API / scraping diferente a Inmuebles24 | Medio | Media | Evaluar si hay API oficial; Apify puede tener actor ya disponible |

---

## Documentos del Modulo

- [Requirements](./Requirements.md) — Capacidades y criterios de aceptacion
- [Research/](./Research/) — Investigacion tecnica

---

*Actualizado: 2026-02-24*
