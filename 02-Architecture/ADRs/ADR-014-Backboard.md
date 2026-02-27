---
status: "propuesto"
date: 2026-02-27
decision-makers: "Pablo (CEO), Fabrizio (Tech Lead)"
costo-mensual: "por definir"
---

# Backboard.io — Memoria Persistente para AI

## Contexto y Problema

OpenRouter ([ADR-013](ADR-013-OpenRouter.md)) no tiene memoria persistente entre conversaciones. Para el módulo AI Brain de BEIQA (matching inteligente, recomendaciones, análisis contextual), se necesita que el sistema "recuerde" interacciones previas, preferencias de clientes, y contexto acumulado. ¿Cómo agregamos memoria persistente a los agentes de AI?

Backboard.io es una plataforma que ofrece "the world's smartest AI memory" — persistent long-term memory, embeddings, RAG, y shared memory entre agentes. Tiene integración nativa con OpenRouter via BYOK (Bring Your Own Key).

## Factores de Decisión

* OpenRouter no tiene memoria nativa
* El módulo AI Brain necesitará contexto acumulado (historial de búsquedas, preferencias de tenants)
* Backboard lidera benchmarks de AI memory (93.4% en LongMemEval, 90.1% en LoCoMo)
* Integración directa con OpenRouter via BYOK
* Soporte para shared memory entre agentes (múltiples agentes compartiendo contexto)

## Opciones a Evaluar

* Backboard.io (memoria AI como servicio)
* Implementación custom con PostgreSQL + pgvector (embeddings propios)
* Mem0 (open-source AI memory)
* No usar memoria persistente (cada conversación empieza de cero)

## Decisión

**Pendiente.** Se evaluará cuando se defina el módulo AI Brain (Fase 3). La decisión depende de: volumen de interacciones, complejidad de contexto requerido, y costo.

## Información Adicional

- Backboard.io: [https://backboard.io](https://backboard.io)
- Complementa a: [ADR-013](ADR-013-OpenRouter.md) (OpenRouter para routing)
- Relevante para: módulo AI Brain (Fase 3), matching tenant-propiedad
- Condición para activar esta decisión: cuando se inicie el diseño del AI Brain
