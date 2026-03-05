---
status: "aceptado"
date: 2026-03-05
decision-makers: "Pablo (CEO), Fabrizio (Tech Lead)"
consulted: "Alex (Advisor)"
informed: "Pamela, Jerónimo"
costo-mensual: "$0 (framework) + LLM TBD (~$67-88 estimado)"
---

# Mastra — Framework de Orquestación de Agentes AI

## Contexto y Problema

BEIQA tiene ~60K propiedades scrapeadas de 4 portales, pero 50%+ de las direcciones de portales de alto volumen (Inmuebles24, Pincali) son incorrectas. Esto bloquea: visualización GIS, portal de tenants, market intelligence, y deduplicación cross-portal.

Se necesita una capa de inteligencia artificial que:
- Enriquezca y corrija direcciones de propiedades (multi-señal: geocoding, reverse geocoding, análisis de descripción)
- Normalice datos de múltiples portales al golden record (`properties`)
- Deduplique propiedades cross-portal
- Genere scoring/matching entre requerimientos de clientes y propiedades
- Produzca inteligencia de mercado (tendencias, comparables, análisis por zona)
- Ejecute análisis geoespacial (H3, AGEB, proximidad)

Hoy, Trigger.dev maneja batch AI extraction con Claude API, pero es insuficiente: no hay orquestación entre tareas AI, no hay memory, no hay evaluación de calidad, y el código AI está acoplado al scraper.

¿Qué framework usamos para orquestar agentes AI de forma independiente del scraper?

## Factores de Decisión

* Stack TypeScript existente (Trigger.dev, Next.js, Supabase) — mantener coherencia
* Necesidad de múltiples agentes con herramientas (tools) especializadas
* Soporte para MCP (Model Context Protocol) — conectar con ArcGIS, Supabase, etc.
* Soporte para workflows multi-paso (enrichment pipeline: validar → geocodificar → normalizar → persistir)
* Memory persistente para contexto acumulado (preferencias de clientes, historial de matching)
* Evaluaciones (evals) para medir calidad de cada agente
* Selección de modelo por agente/tarea — cada operación puede usar un modelo diferente
* Open source preferido — control total, sin vendor lock-in
* Equipo pequeño (2 desarrolladores) — framework debe reducir boilerplate

## Opciones Consideradas

* Mastra (TypeScript, open source, agents + workflows + tools + memory + MCP)
* LangChain.js (TypeScript, ecosistema maduro, chains + agents + tools)
* CrewAI (Python, multi-agent collaboration)
* Custom con Claude SDK directo (sin framework)

## Decisión

Opción elegida: "**Mastra**" (https://mastra.ai), porque es TypeScript nativo (mismo stack), tiene integración oficial con Trigger.dev, soporta MCP como client y server, incluye agents + workflows + tools + memory + evals en un solo framework, y es open source (Apache 2.0, del equipo de Gatsby).

### Consecuencias

* Bien, porque TypeScript nativo — un solo lenguaje en todo el stack
* Bien, porque agents + workflows + tools + memory + MCP + evals en un solo framework
* Bien, porque integración oficial con Trigger.dev para ejecución durable
* Bien, porque MCP permite conectar agentes con ArcGIS, Supabase, y otros servicios
* Bien, porque open source (Apache 2.0) — se puede fork si el proyecto se abandona
* Bien, porque la lógica de agentes es TypeScript portable — no hay lock-in al framework
* Mal, porque framework relativamente joven (lanzado 2025) — riesgo de breaking changes
* Mal, porque menos ecosistema que LangChain (menos ejemplos, menos plugins)
* Neutral, porque requiere un repo separado (`beiqa-agents`) — más repos que mantener

## Plan de Implementación

* **Sistemas afectados**: Nuevo repo `beiqa-agents` (Mastra). `beiqa-scraper` (Trigger.dev) envía HTTP triggers. `Supabase` (shared DB, nuevas tablas). `beiqa-frontend` consume API de Mastra para scoring.
* **Dependencias**: `mastra` (core), `@mastra/core`, modelos LLM vía API directa (Claude, GPT, etc. — TBD por agente), `@supabase/supabase-js`, Google Maps APIs, MCP servers (ArcGIS, GIS-MCP)
* **Patrones a seguir**:
  - Cada agente tiene: instrucciones, modelo (TBD), tools, memory config
  - Tools son funciones TypeScript puras con schema Zod para input/output
  - Workflows orquestan multi-agente (ej: enrichment pipeline)
  - Comunicación con Trigger.dev vía HTTP POST (no imports directos)
  - Supabase como shared database (ambos repos leen/escriben)
* **Patrones a evitar**:
  - No acoplar código de agentes al scraper
  - No prescribir modelos LLM sin testing empírico
  - No usar Mastra para tareas determinísticas (eso es Trigger.dev)
  - No crear agentes monolíticos — preferir agentes especializados con tools discretas
* **Configuración**: `.env` con API keys de modelos, Supabase URL/key, Google Maps key. MCP config en `mastra.config.ts`.
* **Migración**: (1) Crear repo beiqa-agents, (2) Migrar batch AI extraction de Trigger.dev → Mastra, (3) Agregar Address Enrichment Agent, (4) Conectar Trigger.dev → Mastra vía HTTP, (5) Migrar scoring de frontend → Mastra

### Verificación

- [ ] Repo `beiqa-agents` creado con Mastra configurado
- [ ] Al menos un agente (Address Enrichment) procesando propiedades
- [ ] Trigger.dev enviando HTTP trigger a Mastra post-scrape
- [ ] Frontend consumiendo API de Mastra para scoring
- [ ] Agente escribiendo resultados a Supabase golden record
- [ ] Evals midiendo calidad de Address Enrichment (>80% accuracy target)
- [ ] Costos LLM monitoreados y dentro de presupuesto

## Pros y Contras por Opción

### Mastra

* Bien, porque TypeScript nativo, MCP, integración Trigger.dev, agents + workflows + tools + memory + evals
* Bien, porque Apache 2.0 — sin vendor lock-in, portable
* Mal, porque framework joven, comunidad pequeña

### LangChain.js

* Bien, porque ecosistema maduro, mucha documentación, muchos integraciones
* Mal, porque API inestable (breaking changes frecuentes), overhead de abstracciones
* Mal, porque no tiene integración nativa con Trigger.dev ni MCP server/client

### CrewAI

* Bien, porque diseñado para multi-agent collaboration
* Mal, porque Python — rompe coherencia del stack TypeScript
* Mal, porque requeriría bridge entre Python y TypeScript

### Custom con Claude SDK

* Bien, porque control total, sin dependencias de framework
* Mal, porque reinventar: workflows, memory, tool execution, evals
* Mal, porque significativamente más código boilerplate

## Información Adicional

- Mastra docs: [https://mastra.ai/docs](https://mastra.ai/docs)
- Mastra + Trigger.dev integration: [https://mastra.ai/docs/integrations/trigger-dev](https://mastra.ai/docs/integrations/trigger-dev)
- Mastra MCP support: client y server — permite consumir MCP servers (ArcGIS, GIS-MCP) y exponer tools como MCP server
- **Modelos LLM**: TBD por agente. Requiere evaluación empírica (costo vs calidad vs velocidad). Mastra soporta cualquier modelo vía API.
- **Arquitectura de agentes**: Ver [Agent-Architecture.md](../Agent-Architecture.md) para el diseño completo
- Separación de responsabilidades: [ADR-021](ADR-021-Separacion-Trigger-Mastra.md)
- Supersede: [ADR-014](ADR-014-Backboard.md) (Backboard.io) — Mastra memory reemplaza la necesidad de memoria AI externa
- Relacionado con: [ADR-003](ADR-003-Trigger-dev.md) (Trigger.dev — scope reducido), [ADR-012](ADR-012-Multi-Portal-Data.md) (golden record)
