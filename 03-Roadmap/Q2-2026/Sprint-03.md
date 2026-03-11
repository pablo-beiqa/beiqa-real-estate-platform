# Sprint 3 (Mar 30 - Abr 12)

> **Estado**: Planificado
> **Quarter**: Q2-2026
> **Milestones**: Enrichment Agent Operativo (cierre), Portal Autenticado (cierre), Golden Record Pipeline (inicio), Design System Implementado (inicio)

---

## OKRs

| Objetivo | Key Result | Owner |
|----------|------------|-------|
| O1: Backfill masivo de I24 | KR1: ≥1K propiedades I24 enriquecidas via batch | Pablo |
| O2: Pipeline Trigger.dev → Mastra | KR2: HTTP trigger funcional post-scrape | Fabrizio |
| O3: RLS multi-tenant verificado | KR3: Tenant A no puede ver datos de Tenant B | Fabrizio |
| O4: Design system componentes core | KR4: Primeros componentes Figma → shadcn/ui implementados | Pablo + Pamela |

## Definition of Done

- [ ] Backfill procesando batches (≥1K props enriquecidas)
- [ ] Trigger.dev dispara Mastra post-scrape con payload real
- [ ] RLS verificado con test multi-tenant
- [ ] ≥3 componentes del design system implementados en beiqa-frontend

---

## Deliverables por Track

### Track: AI / Mastra (Pablo)

| Deliverable | Issue | AC | Estado |
|-------------|-------|----|--------|
| Backfill workflow: I24 en batches | [#91](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/91) | Batches de 500. Rate limits. Progreso en enrichment_queue. ≥1K procesadas. | Pendiente |
| Description address extractor (LLM tool) | [#92](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/92) | LLM extrae direccion de texto libre de descripcion. Accuracy en 50 props. | Pendiente |
| Data Normalization Agent v1 (inicio) | [#93](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/93) | I24 → golden record. Mapeo de campos basico. Feature extraction. | Pendiente |

### Track: Scraper / Infra (Fabrizio)

| Deliverable | Issue | AC | Estado |
|-------------|-------|----|--------|
| Trigger.dev → Mastra HTTP integration | [#94](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/94) | POST funcional con payload real. Retry logic. Error handling. | Pendiente |
| I24 migration a Trigger.dev (inicio) | NUEVO | Primeros pasos de migracion: config, estructura, pruebas. | Pendiente |
| Modulo Slack compartido | [#18](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/18) | Notificacion success/error reutilizable por todos los scrapers. | Pendiente |
| Multi-tenant RLS verificacion | NUEVO | Tests que verifican aislamiento de datos por tenant en Supabase. | Pendiente |

### Track: Frontend / Tenant Portal (Pablo)

| Deliverable | Issue | AC | Estado |
|-------------|-------|----|--------|
| Vincular auth con tenant | [#52](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/52) | auth_user_id en tenants. RLS via auth.uid(). | Pendiente |

### Track: Design (Pamela → Pablo)

| Deliverable | Issue | AC | Estado |
|-------------|-------|----|--------|
| Design system: core components | NUEVO | Figma → shadcn/ui: cards, tables, buttons, navigation, layout. ≥3 componentes. | Pendiente |

---

## Riesgos y Dependencias

| Riesgo / Dependencia | Impacto | Mitigacion |
|---------------------|---------|------------|
| Golden record schema (#104, #105) debe estar listo (Sprint 2) | Alto | Si no cierra en S2, priorizar al inicio de S3 |
| Backfill depende de Enrichment Agent funcional (Sprint 2) | Alto | Si #87 no cierra, backfill se pospone |
| Pamela designs deben estar listos para implementacion | Medio | Comunicar fechas de entrega claras |

---

## Review (Abr 12)

> Completar al cierre del sprint.

### Metricas de Cierre

| Metrica | Target | Resultado |
|---------|--------|-----------|
| Issues cerrados | 7-9 | ... |
| KRs completados | 4/4 | ... |

### Retrospectiva

- **Bien**: ...
- **Mejorar**: ...
- **Accion**: ...
