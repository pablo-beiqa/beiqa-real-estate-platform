# AI Brain

**Fase del proyecto**: Actual — Sprint 1+
**Estado**: 🟢 En desarrollo
**Owner**: Pablo + Fabrizio

---

## Descripción

El módulo AI Brain es la **capa transversal de inteligencia** de la plataforma BEIQA. Implementado con [Mastra](https://mastra.ai) ([ADR-020](../../02-Architecture/ADRs/ADR-020-Mastra.md)), orquesta agentes AI especializados que proveen inteligencia a todos los demás módulos del sistema.

No es un módulo aislado — el AI Brain consume datos de y provee inteligencia a: Scraper, Data, Geospatial, Market Intelligence, Internal App, y Tenant Portal.

**Framework**: Mastra (TypeScript, open source, Apache 2.0)
**Repo**: `github.com/pablo-beiqa/beiqa-agents`
**Separación de responsabilidades**: Trigger.dev = ejecución durable (scraping, cron, sync). Mastra = AI reasoning (enrichment, scoring, intelligence). Ver [ADR-021](../../02-Architecture/ADRs/ADR-021-Separacion-Trigger-Mastra.md).

---

## Objetivos

1. **Address Enrichment**: Corregir el 50%+ de direcciones incorrectas en portales de alto volumen, alcanzando >80% de accuracy medida contra verificación manual.
2. **Data Normalization**: Mapear datos de 4 portales con schemas diferentes al golden record unificado (`properties`), con >95% de campos mapeados correctamente.
3. **Deduplication**: Detectar y consolidar propiedades duplicadas cross-portal con precisión >85%.
4. **Scoring / Matching**: Generar shortlists de propiedades relevantes para requerimientos de clientes con concordancia >75% vs evaluación humana. Migrado del frontend.
5. **Market Intelligence**: Producir inteligencia de mercado automatizada (tendencias, comparables, análisis por zona).
6. **GIS Analysis**: Cálculo de H3, asignación de AGEB, análisis de proximidad y calidad de zona.

---

## Métricas de Éxito / KPIs

| Métrica | Target | Cómo se mide |
|---------|--------|--------------|
| Accuracy de Address Enrichment | >80% | Verificación manual de 100 propiedades random por portal |
| Campos normalizados correctamente | >95% | Comparación automática vs manual de 50 propiedades por portal |
| Precisión de deduplicación | >85% | Verificación manual de 50 pares marcados como duplicados |
| Concordancia de scoring | >75% | 20 scorings comparados con criterio de Pablo |
| Tiempo de enrichment (nuevas propiedades) | <60 min | Medición de latencia scrape → golden record |
| Costo LLM mensual | Dentro de presupuesto (TBD) | Monitoreo de costos por agente en `agent_runs` |

---

## Agentes

Ver arquitectura completa en [Agent-Architecture.md](../../02-Architecture/Agent-Architecture.md).

| Agente | Prioridad | Módulo que sirve | Estado |
|--------|-----------|-----------------|--------|
| Address Enrichment | P0 (bloqueador) | Data, Geospatial | 🔴 Por implementar |
| Data Normalization | P0 | Data | 🔴 Por implementar |
| Deduplication | P1 | Data | 🔴 Por implementar |
| Scoring / Matching | P1 (migra de frontend) | Internal App, Tenant Portal | 🔴 Por implementar |
| Market Intelligence | P2 | Market Intelligence | 🔴 Por implementar |
| GIS Analysis | P2 | Geospatial | 🔴 Por implementar |

**Modelos LLM**: TBD por agente. Requiere evaluación empírica (costo vs calidad vs velocidad). Mastra permite asignar diferentes modelos por agente.

---

## Dependencias

### Necesita (upstream — todos los módulos)
- **Scraper** → Datos crudos de propiedades en staging tables + HTTP trigger post-scrape
- **Data** → Schema de golden record, staging tables existentes
- **Geospatial** → Datos de coordenadas, shapefiles AGEB, configuración H3
- **Market Intelligence** → Definición de métricas de mercado, zonas de interés
- **Base de datos** → Supabase con PostGIS, tablas nuevas (properties, agent_runs, enrichment_queue, etc.)

### Depende de este (downstream — todos los módulos)
- **Data** → Golden record poblado y mantenido por agentes
- **Geospatial** → H3 y AGEB calculados por GIS Agent
- **Market Intelligence** → Reportes y análisis generados por Market Intelligence Agent
- **Internal App** → Scoring on-demand, datos enriquecidos, analytics
- **Tenant Portal** → Scoring, shortlists, recomendaciones personalizadas

---

## Riesgos Clave

| # | Riesgo | Impacto | Probabilidad | Mitigación |
|---|--------|---------|--------------|------------|
| 1 | Address accuracy insuficiente (<80%) | Alto | Media | 4 señales de validación (geocoding, reverse geocoding, descripción, coordenadas). Confidence score permite filtrar. <50 → revisión humana. |
| 2 | Costos LLM exceden presupuesto | Medio | Media | Monitoreo por agente en `agent_runs`. Alertas al 80% del budget. Testing de modelos baratos para bulk ops. |
| 3 | Mastra framework inestable (breaking changes) | Medio | Baja | Apache 2.0 — se puede fork. Lógica de agentes es TypeScript portable. Fallback: Trigger.dev direct tasks. |
| 4 | Google Maps API credit exhaustion | Alto | Baja | Cache agresivo (30d TTL). Daily budget caps. Procesar portales premium last (ya tienen direcciones correctas). |
| 5 | Equipo limitado (2 devs para todo) | Alto | Alta | Implementación incremental (2-3 agentes/sprint). Address Enrichment es el único bloqueador real. Los demás pueden diferirse. |

---

## Documentos del Módulo

- [Product Questions](./Product-Questions.md) — Cuestionario de discovery
- [Requirements](./Requirements.md) — Capacidades y criterios de aceptación
- [Research/](./Research/) — Investigación técnica
- **[Agent-Architecture.md](../../02-Architecture/Agent-Architecture.md)** — Arquitectura completa de agentes (tools, flujos, schema, evals)
- **[ADR-020](../../02-Architecture/ADRs/ADR-020-Mastra.md)** — Decisión de usar Mastra
- **[ADR-021](../../02-Architecture/ADRs/ADR-021-Separacion-Trigger-Mastra.md)** — Separación Trigger.dev vs Mastra
