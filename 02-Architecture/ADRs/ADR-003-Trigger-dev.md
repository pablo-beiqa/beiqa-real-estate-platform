---
status: "aceptado"
date: 2026-02-25
decision-makers: "Fabrizio (Tech Lead), Pablo (CEO)"
consulted: "Alex (Advisor)"
informed: "Pamela, Jerónimo"
costo-mensual: "Free tier"
---

# Trigger.dev — Automatización y Ejecución

## Contexto y Problema

BEIQA necesita una plataforma para ejecutar tareas automatizadas: scraping de portales, limpieza de datos con AI, push a HubSpot y Supabase, cron jobs, y notificaciones. Originalmente se usaba n8n Cloud para integraciones visuales y Trigger.dev solo para jobs pesados de AI. ¿Consolidamos todo en una sola plataforma o mantenemos dos?

## Factores de Decisión

* La lógica del scraper requiere TypeScript real, no workflows visuales
* Manejo de errores fino por listing (retry con backoff, human-in-the-loop)
* Cron nativo para scheduling semanal/mensual
* Dashboard de ejecuciones y logs para observabilidad
* n8n no soporta bien: parsing HTML complejo, lógica condicional por portal, batch AI
* Ya se tiene Trigger.dev configurado y funcionando

## Opciones Consideradas

* n8n (integraciones visuales) + Trigger.dev (jobs pesados)
* Solo Trigger.dev (todo consolidado)
* Temporal.io
* Inngest

## Decisión

Opción elegida: "**Solo Trigger.dev**", porque la lógica de los scrapers y automatizaciones requiere control fino en TypeScript que n8n no puede ofrecer. Consolidar en una sola plataforma reduce complejidad operativa.

**Scope de Trigger.dev (ES):**
- Scrapers: ejecución, orquestación, scheduling
- Push a Supabase: persistencia de datos scrapeados en staging tables
- Push a HubSpot: sync de propiedades y brokers (one-way, determinístico)
- Cron jobs: scheduling semanal/mensual
- Notificaciones: alertas a Slack
- HTTP trigger a Mastra: notificar post-scrape para iniciar enrichment pipeline

**Scope de Trigger.dev (NO ES):**
- No es el backend de la aplicación
- No es el cerebro central del sistema
- No es un componente de consulta para el frontend
- No es el motor de AI/NLP — la orquestación AI pasa a Mastra ([ADR-020](ADR-020-Mastra.md), [ADR-021](ADR-021-Separacion-Trigger-Mastra.md))

> **Actualizado 2026-03-05**: Limpieza de datos con AI y batch AI extraction migran a Mastra como agentes independientes. Ver [ADR-020](ADR-020-Mastra.md) y [ADR-021](ADR-021-Separacion-Trigger-Mastra.md).

### Consecuencias

* Bien, porque una sola plataforma de automatización (menos complejidad)
* Bien, porque TypeScript nativo — desarrollo rápido con tipado
* Bien, porque retries automáticos con backoff exponencial
* Bien, porque human-in-the-loop (pausa y notifica via Slack)
* Bien, porque logs y dashboard de ejecuciones
* Mal, porque se pierde la interfaz visual de n8n para workflows simples
* Neutral, porque requiere migrar flujos existentes de n8n a Trigger.dev

## Plan de Implementación

* **Sistemas afectados**: `beiqa-scraper` (repo principal de Trigger.dev), n8n Cloud (a deprecar), Supabase (destino de datos), HubSpot (sync)
* **Dependencias**: `@trigger.dev/sdk@^4.4.1`, `@trigger.dev/build@^4.4.1`
* **Patrones a seguir**: Patrón orchestrator + processor (tasks que orquestan sub-tasks). Config en `trigger.config.ts`. Tasks en `src/trigger/`.
* **Patrones a evitar**: No usar n8n para nuevos flujos. No construir backend Express/FastAPI.
* **Configuración**: Trigger.dev project `proj_atbupoppuamgwhnoernt`, MCP config en `mcp.json`
* **Migración**: Migración incremental de n8n: (1) cron jobs → Trigger.dev scheduled tasks, (2) webhooks Slack → Trigger.dev triggers, (3) sync HubSpot → Trigger.dev tasks, (4) deprecar n8n Cloud

### Verificación

- [x] Trigger.dev configurado con 3 scrapers (Pincali, CBRE, Colliers)
- [x] Batch AI extraction con Claude API funcionando → **migrado a Mastra** ([ADR-020](ADR-020-Mastra.md))
- [ ] Cron scheduling migrado de n8n a Trigger.dev
- [ ] Sync HubSpot migrado de n8n a Trigger.dev
- [ ] Notificaciones Slack migradas de n8n a Trigger.dev
- [ ] n8n Cloud deprecado (sin flujos activos)
- [ ] HTTP trigger a Mastra post-scrape implementado

## Pros y Contras por Opción

### Solo Trigger.dev

* Bien, porque una plataforma, un lenguaje (TypeScript), un dashboard
* Bien, porque control fino de errores, retries, human-in-the-loop
* Mal, porque se pierde la interfaz visual para flujos simples de integración

### n8n + Trigger.dev (estado anterior)

* Bien, porque n8n es visual y rápido para integraciones simples
* Mal, porque dos plataformas que mantener
* Mal, porque n8n no maneja bien lógica compleja de scraping

### Temporal.io

* Bien, porque battle-tested para workflows complejos
* Mal, porque overhead de setup y costo para el volumen actual

### Inngest

* Bien, porque TypeScript-first como Trigger.dev
* Mal, porque menos maduro, menos documentación

## Información Adicional

- Por qué no n8n: [Scraper/Research/Acquisition-Strategy.md](../../01-Modules/Scraper/Research/Acquisition-Strategy.md) sección "Por qué NO n8n"
- Repo del scraper: `github.com/pablo-beiqa/beiqa-scraper`
- Config Trigger.dev: `trigger.config.ts` en `beiqa-scraper`
- Supersede: La regla "n8n vs Trigger.dev" en Stack-Decidido.md queda obsoleta
- Relacionado con: [ADR-002](ADR-002-Estrategia-Scraping.md), [ADR-019](ADR-019-n8n-Deprecado.md) (n8n deprecado)
