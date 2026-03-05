---
status: "supersedido por [ADR-020](ADR-020-Mastra.md)"
date: 2026-02-27
decision-makers: "Pablo (CEO), Fabrizio (Tech Lead)"
costo-mensual: "$0 (supersedido)"
---

# Backboard.io — Memoria Persistente para AI

> **⚠️ SUPERSEDIDO**: Esta decisión fue supersedida por [ADR-020 — Mastra](ADR-020-Mastra.md) (2026-03-05). Mastra incluye persistent memory, RAG, y shared memory entre agentes como funcionalidad built-in, eliminando la necesidad de un servicio externo de memoria AI.

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

**Supersedido por [ADR-020 — Mastra](ADR-020-Mastra.md).** Mastra (mastra.ai) incluye:
- **Persistent memory**: almacenamiento de contexto entre ejecuciones de agentes
- **RAG**: retrieval-augmented generation integrado
- **Shared memory**: múltiples agentes compartiendo contexto
- **PostgreSQL backend**: usa la misma Supabase que el resto del sistema

No se necesita un servicio externo de memoria AI.

## Información Adicional

- Backboard.io: [https://backboard.io](https://backboard.io)
- **Supersedido por**: [ADR-020](ADR-020-Mastra.md) (Mastra — framework que incluye memory)
- Mastra memory docs: [https://mastra.ai/docs/memory](https://mastra.ai/docs/memory)
