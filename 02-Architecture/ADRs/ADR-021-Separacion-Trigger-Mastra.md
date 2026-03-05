---
status: "aceptado"
date: 2026-03-05
decision-makers: "Pablo (CEO), Fabrizio (Tech Lead)"
consulted: "Alex (Advisor)"
informed: "Pamela, Jerónimo"
costo-mensual: "$0 (decisión arquitectónica)"
---

# Separación de Responsabilidades — Trigger.dev vs Mastra

## Contexto y Problema

Con la adopción de Mastra ([ADR-020](ADR-020-Mastra.md)) como framework de agentes AI, el sistema ahora tiene dos plataformas de ejecución: Trigger.dev (ya en producción) y Mastra (nuevo). Ambas pueden ejecutar código TypeScript, ambas pueden interactuar con Supabase, y ambas pueden ejecutar lógica compleja.

¿Cómo dividimos las responsabilidades para evitar solapamiento, reducir acoplamiento, y maximizar la resiliencia del sistema?

## Factores de Decisión

* Trigger.dev ya está en producción con scrapers funcionando — moverlo todo sería disruptivo
* Mastra es óptimo para lógica que requiere razonamiento AI (selección de modelo, prompts, memory)
* Las tareas determinísticas (scraping, sync, cron) no necesitan AI — sobreingeniería usar Mastra
* La resiliencia mejora si los sistemas son independientes: falla de uno no afecta al otro
* El equipo es pequeño (2 devs) — la complejidad operativa debe ser mínima
* Supabase como shared database elimina necesidad de comunicación compleja entre repos

## Opciones Consideradas

* Todo en Trigger.dev (Mastra como librería dentro de tasks)
* Todo en Mastra (migrar scrapers)
* Separación por naturaleza de la tarea: determinístico vs AI

## Decisión

Opción elegida: "**Separación por naturaleza: Trigger.dev = determinístico, Mastra = AI reasoning**", porque cada plataforma se usa para lo que hace mejor. Trigger.dev excele en ejecución durable con retries; Mastra excele en orquestación de agentes AI con tools, memory, y evaluaciones.

**Tabla de responsabilidades:**

| Responsabilidad | Plataforma | Razón |
|----------------|-----------|-------|
| Scraping de portales | Trigger.dev | Determinístico. Retries, cron, rate limiting. |
| Persistencia a Supabase (staging) | Trigger.dev | Parte del pipeline de scraping. |
| Cron scheduling | Trigger.dev | Scheduling durable con dashboard. |
| HubSpot sync (one-way) | Trigger.dev | Determinístico. API-to-API sin AI. |
| Slack alertas | Trigger.dev | Notificaciones simples. |
| Address enrichment | Mastra | Requiere LLM (análisis de descripción), multi-señal, confidence scoring. |
| Data normalization | Mastra | Parte determinística + parte AI (feature extraction de texto libre). |
| Deduplication | Mastra | Scoring determinístico + LLM para casos ambiguos. |
| Scoring / matching | Mastra | Requiere LLM para evaluar fit propiedad-cliente. |
| Market intelligence | Mastra | Análisis + generación de narrativa con LLM. |
| GIS analysis | Mastra | H3/AGEB cálculo + análisis contextual. MCP con ArcGIS. |

**Comunicación entre sistemas:**

```
Trigger.dev (beiqa-scraper)          Mastra (beiqa-agents)
┌─────────────────────┐              ┌─────────────────────┐
│ Scraper completa     │──HTTP POST──→│ Enrichment pipeline │
│ (persiste staging)   │              │ (agentes procesan)  │
└─────────────────────┘              └──────────┬──────────┘
                                                │
                                     Escribe a Supabase
                                     (golden record)
                                                │
                                     ┌──────────▼──────────┐
                                     │   beiqa-frontend     │
                                     │ (lee golden record)  │
                                     └─────────────────────┘
```

### Consecuencias

* Bien, porque cada sistema hace lo que mejor sabe hacer
* Bien, porque falla en Mastra no detiene scraping (y viceversa)
* Bien, porque repos independientes — deploy y scale por separado
* Bien, porque Supabase como shared DB simplifica la comunicación
* Mal, porque dos plataformas que mantener y monitorear
* Mal, porque el HTTP trigger Trigger.dev → Mastra agrega un punto de falla
* Neutral, porque el equipo debe entender qué va en cada sistema (boundary clara ayuda)

## Plan de Implementación

* **Sistemas afectados**: `beiqa-scraper` (agregar HTTP trigger post-scrape), `beiqa-agents` (exponer API endpoint), Supabase (ambos leen/escriben)
* **Dependencias**: HTTP endpoint en Mastra (Hono server), webhook o HTTP task en Trigger.dev
* **Patrones a seguir**:
  - Trigger.dev llama a Mastra vía HTTP POST después de persistir datos en staging
  - Mastra expone endpoints para cada pipeline (enrichment, scoring, etc.)
  - Ambos usan Supabase client con las mismas credenciales
  - Monitoring: ambos notifican a Slack en caso de error
* **Patrones a evitar**:
  - No importar código de un repo en otro (son independientes)
  - No duplicar lógica entre repos
  - No usar Mastra para tareas sin componente AI
  - No usar Trigger.dev para orquestación de agentes AI
* **Configuración**: Mastra URL en env vars de Trigger.dev. Supabase URL/key compartida.
* **Migración**: (1) Batch AI extraction migra de Trigger.dev a Mastra, (2) Trigger.dev agrega HTTP trigger post-scrape, (3) Validar que el pipeline end-to-end funciona

### Verificación

- [ ] Trigger.dev solo ejecuta: scraping, persistencia, cron, sync, alertas
- [ ] Mastra solo ejecuta: enrichment, normalization, dedup, scoring, intelligence, GIS
- [ ] Trigger.dev envía HTTP trigger a Mastra post-scrape
- [ ] Ambos escriben a Supabase sin conflicto
- [ ] Falla en Mastra no detiene scraping
- [ ] Falla en scraping no detiene agentes (procesan datos existentes)

## Pros y Contras por Opción

### Separación por naturaleza (elegida)

* Bien, porque cada plataforma en su fortaleza, resiliencia por independencia
* Mal, porque dos plataformas que mantener

### Todo en Trigger.dev

* Bien, porque una sola plataforma, menos complejidad operativa
* Mal, porque Trigger.dev no tiene: agents, memory, evals, MCP, model routing
* Mal, porque código AI acoplado a código de scraping

### Todo en Mastra

* Bien, porque una sola plataforma para todo
* Mal, porque Mastra no es ideal para: cron scheduling durable, retries con backoff, rate limiting
* Mal, porque migrar scrapers es disruptivo y no agrega valor

## Información Adicional

- Trigger.dev scope actualizado: [ADR-003](ADR-003-Trigger-dev.md)
- Mastra como framework: [ADR-020](ADR-020-Mastra.md)
- Arquitectura de agentes: [Agent-Architecture.md](../Agent-Architecture.md)
- Data architecture: [ADR-012](ADR-012-Multi-Portal-Data.md) (staging → golden record)
