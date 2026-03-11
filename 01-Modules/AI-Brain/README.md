# AI Brain

**Fase del proyecto**: Sprint 1+ (en desarrollo activo)
**Estado**: 🟢 En diseño detallado
**Owner**: Pablo (agentes de negocio) + Fabrizio (infraestructura, lógica)
**Repo de código**: `github.com/pablo-beiqa/beiqa-agents` (por crear)

---

## Descripción

El AI Brain es la **capa transversal de inteligencia** de la plataforma Beiqa. Orquesta 7 agentes especializados organizados en 3 tiers que procesan datos, atienden necesidades de clientes, y generan inteligencia avanzada. Implementado con [Mastra](https://mastra.ai) (TypeScript, Apache 2.0) — ver [ADR-020](../../02-Architecture/ADRs/ADR-020-Mastra.md).

No es un módulo aislado — el AI Brain consume datos de y provee inteligencia a: Scraper, Data, Geospatial, Market Intelligence, Internal App, y Tenant Portal.

> **Separación de responsabilidades ([ADR-021](../../02-Architecture/ADRs/ADR-021-Separacion-Trigger-Mastra.md))**: Trigger.dev = ejecución durable (scraping, cron, sync). Mastra = AI reasoning (enrichment, scoring, intelligence).

---

## Estado Actual

Nada funcional todavía. El repo `beiqa-agents` no existe. [Agent-Architecture.md](../../02-Architecture/Agent-Architecture.md) es la fuente de verdad para el diseño.

---

## 7 Agentes en 3 Tiers

| Agente | Tier | Prioridad | Sprint | Estado |
|--------|------|-----------|--------|--------|
| Address Enrichment | 1: Data Pipeline | P0 | 1 | 🔴 Por implementar |
| Data Normalization | 1: Data Pipeline | P0 | 1-2 | 🔴 Por implementar |
| Deduplication | 1: Data Pipeline | P1 | 3 | 🔴 Por implementar |
| Score Discovery | 2: Client Intelligence | P1 | 2-3 | 🔴 Por implementar |
| Property Search & Match | 2: Client Intelligence | P1 | 3-4 | 🔴 Por implementar |
| GIS Analysis | 3: Intelligence & Analysis | P2 | 5-6 | 🔴 Por implementar |
| Market Intelligence | 3: Intelligence & Analysis | P2 | 5-6 | 🔴 Por implementar |

### Tier 1: Data Pipeline (background, automatizado post-scrape)

Agentes que corren en background después de cada scrape. Corrección de direcciones, normalización al golden record unificado, y deduplicación cross-portal.

### Tier 2: Client Intelligence (interactivo + autónomo)

- **Score Discovery**: Extrae criterios de scoring a partir de transcripts de Circleback + input manual. Genera ScoringDocument con 160+ criterios organizados en 7 grupos (A-G).
- **Property Search & Match**: 2 modos de operación — chatbot interactivo para el equipo + matching autónomo con alertas proactivas cuando nueva propiedad matchea scoring activo. Memoria per-client.

### Tier 3: Intelligence & Analysis (análisis avanzado)

- **GIS Analysis**: Análisis geoespacial con 10+ fuentes, zone quality scoring compuesto y diferenciado por tipo de propiedad.
- **Market Intelligence**: Reportes automáticos + on-demand + narrativa comparativa de mercado.

**Modelos LLM**: TBD por agente. Requiere evaluación empírica (costo vs calidad vs velocidad). Mastra permite asignar diferentes modelos por agente.

---

## Objetivos

1. Corregir 50%+ de direcciones incorrectas a >80% accuracy
2. Normalizar datos de 5 portales a golden record con >95% campos correctos
3. Detectar duplicados cross-portal con >85% precisión
4. Generar ScoringDocument con 160+ criterios (7 grupos A-G) a partir de transcripts de llamadas
5. Buscar y matchear propiedades contra scoring de clientes con >75% concordancia vs evaluación humana
6. Alertas proactivas cuando nueva propiedad matchea scoring activo de un cliente
7. Análisis geoespacial con 10+ fuentes y zone quality composite score diferenciado por tipo de propiedad
8. Inteligencia de mercado automatizada (reportes + comparativos + narrativa)

---

## Métricas de Éxito / KPIs

| Métrica | Target | Cómo se mide |
|---------|--------|--------------|
| Address accuracy | >80% | Verificación manual de 100 propiedades random por portal |
| Campos normalizados correctamente | >95% | Comparación automática vs manual, 50 propiedades por portal |
| Precisión de deduplicación | >85% | Verificación manual de 50 pares marcados como duplicados |
| Score discovery accuracy | >80% criterios extraídos | 10 scorings con transcript real |
| Concordancia de scoring | >75% | 20 scorings vs criterio de Pablo |
| Relevancia de alertas proactivas | >50% alertas compartidas con cliente | % de alertas que el equipo decide compartir |
| Costo LLM mensual | Dentro de presupuesto (TBD) | Monitoreo de costos por agente en `agent_runs` |
| Volumen de HITL | Tendencia decreciente | Casos en `review_queue` por semana |

---

## Arquitecturas Cross-cutting

Diseños transversales que aplican a múltiples agentes. Detalle completo en [Agent-Architecture.md](../../02-Architecture/Agent-Architecture.md).

- **Memoria en 3 capas**: Layer 1 Supabase (persistente, compartida), Layer 2 Mastra agent (contexto por agente), Layer 3 Mastra system (contexto global)
- **Human-in-the-Loop (HITL)**: Review queue en Supabase + notificaciones Slack + interfaz en Internal App
- **Alertas proactivas**: Diferenciador de negocio — el equipo recibe alertas cuando nueva propiedad matchea scoring activo de un cliente, decide si compartir
- **Feedback loop**: No modifica scoring directamente, alimenta memoria del agente para mejorar futuras recomendaciones

---

## Dependencias

### Necesita (upstream)

- **Scraper** → Datos crudos en 5 staging tables + HTTP trigger post-scrape
- **Data** → Schema de golden record (tabla `properties`, por crear)
- **Supabase** → PostgreSQL + PostGIS + triggers para H3/AGEB
- **Circleback** → Transcripts de llamadas con clientes (para Score Discovery)
- **Google Maps Platform** → Geocoding + Places + Distance Matrix APIs
- **ArcGIS** → Análisis espacial vía MCP

### Depende de este (downstream)

- **Data** → Golden record poblado por agentes
- **Geospatial** → Zone quality scores del GIS Agent
- **Market Intelligence** → Reportes del Market Intelligence Agent
- **Internal App** → Interfaz HITL review, búsqueda interactiva (Sprint 5+)
- **Tenant Portal** → Display de scoring, shortlists, captura de feedback

---

## Riesgos Clave

| # | Riesgo | Impacto | Probabilidad | Mitigación |
|---|--------|---------|--------------|------------|
| 1 | Address accuracy insuficiente (<80%) | Alto | Media | 5 señales de validación, confidence score, HITL para confidence <50 |
| 2 | Costos LLM exceden presupuesto | Medio | Media | Monitoreo por agente, alertas al 80% del budget, testing de modelos baratos para bulk ops |
| 3 | Mastra framework inestable (breaking changes) | Medio | Baja | Apache 2.0 — se puede fork. Lógica de agentes es TypeScript portable |
| 4 | Google Maps API credit exhaustion | Alto | Baja | Cache agresivo (30d TTL), daily budget caps |
| 5 | Complejidad de Score Discovery (160+ criterios) | Medio | Media | Grupos condicionales (B-E solo si aplica), validación con equipo |
| 6 | Complejidad de GIS Agent (10+ fuentes) | Alto | Media | Incremental: INEGI + Google + ArcGIS primero, demás fuentes después |
| 7 | Ruido en alertas proactivas | Bajo | Media | Threshold configurable por cliente, equipo decide antes de que el cliente vea |

---

## Documentos del Módulo

- [Product Questions](./Product-Questions.md) — Cuestionario respondido (entrevista 2026-03-11)
- [Requirements](./Requirements.md) — Capacidades MUST / SHOULD / COULD
- [Research/AI-Strategy.md](./Research/AI-Strategy.md) — Estrategia de LLM y costos
- [Research/Scoring-Criteria.md](./Research/Scoring-Criteria.md) — Catálogo de 160+ criterios (7 grupos A-G)
- [Research/Memory-Architecture.md](./Research/Memory-Architecture.md) — Diseño de memoria 3 capas
- [Research/GIS-Analysis-Strategy.md](./Research/GIS-Analysis-Strategy.md) — Fuentes, análisis por tipo, zone quality score

**Fuente de verdad para diseño de agentes**: [Agent-Architecture.md](../../02-Architecture/Agent-Architecture.md)
