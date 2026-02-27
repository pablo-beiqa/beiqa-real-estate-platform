---
status: "propuesto"
date: 2026-02-27
decision-makers: "Pablo (CEO), Fabrizio (Tech Lead)"
costo-mensual: "por definir"
---

# OpenRouter — AI Routing y Orquestación de Modelos

## Contexto y Problema

Cuando BEIQA tenga un frontend propio (post-Rube), necesitará una capa que decida qué modelo de AI usar para cada consulta, optimizando costo y calidad. ¿Cómo orquestamos las llamadas a múltiples modelos (Claude, ChatGPT, open-source) con guardrails y optimización de costos?

OpenRouter permite seleccionar y rutear entre miles de modelos con una sola API. No tiene memoria persistente (para eso se evalúa Backboard.io — ver [ADR-014](ADR-014-Backboard.md)).

## Factores de Decisión

* Necesidad de usar múltiples modelos (Claude para español, GPT-4o para otros casos)
* Optimización de costos según el tipo de consulta (modelo barato para parsing, caro para análisis)
* Guardrails: control de qué modelo se usa cuándo y cómo
* API unificada para no acoplarse a un solo proveedor
* OpenRouter soporta ~17,000+ modelos

## Opciones a Evaluar

* OpenRouter (routing multi-modelo)
* Llamadas directas a cada proveedor (Claude API + OpenAI API por separado)
* LiteLLM (proxy open-source)
* No usar routing (un solo modelo para todo)

## Decisión

**Pendiente.** Se evaluará cuando se construya el frontend propio y se defina el módulo AI Brain.

## Información Adicional

- OpenRouter: [https://openrouter.ai](https://openrouter.ai)
- Complementar con: [ADR-014](ADR-014-Backboard.md) (Backboard.io para memoria)
- Hoy se usa Claude API vía Rube ([ADR-004](ADR-004-Rube-MCP-Bridge.md)) — OpenRouter sería el siguiente paso
- Relevante para: módulo AI Brain (Fase 3)
