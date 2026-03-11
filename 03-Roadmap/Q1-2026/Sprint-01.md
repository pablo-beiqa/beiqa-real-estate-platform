# Sprint 1 (Mar 2-15)

> **Estado**: Activo
> **Quarter**: Q1-2026
> **Milestones**: Scrapers Consolidados, Enrichment Agent Operativo (inicio)

---

## OKRs

| Objetivo | Key Result | Owner |
|----------|------------|-------|
| O1: Infraestructura AI agents | KR1: `beiqa-agents` repo con `mastra dev` funcionando | Pablo |
| O2: Scrapers persisten con staging tables | KR2: ≥3 staging tables creadas (CBRE, Colliers, Pincali) | Fabrizio |
| O2 | KR3: Modulo de escritura a Supabase funcional | Fabrizio |

## Definition of Done

- [ ] `mastra dev` corre localmente en beiqa-agents
- [ ] ≥3 staging tables creadas con schema alineado
- [ ] Modulo de escritura a Supabase reutilizable y testeado
- [ ] Codigo commiteado y pusheado

---

## Deliverables por Track

### Track: AI / Mastra (Pablo)

| Deliverable | Issue | AC | Estado |
|-------------|-------|----|--------|
| Inicializar repo beiqa-agents | [#86](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/86) | `mastra dev` arranca. Estructura agents/tools/workflows. README + .env.example. | Next on List |
| Address Enrichment Agent skeleton | [#87](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/87) | Estructura basica del agente. Tools definidos. No necesita procesar E2E aun. | Backlog |

### Track: Scraper / Infra (Fabrizio)

| Deliverable | Issue | AC | Estado |
|-------------|-------|----|--------|
| Staging table CBRE | [#108](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/108) | `cbre_listings` creada. Schema alineado a CbreProperty. | In Progress |
| Staging table Colliers | [#109](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/109) | `colliers_listings` creada. Schema alineado. | In Progress |
| Staging table Pincali | [#110](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/110) | `pincali_listings` ya existe con 1,761 props. Alinear schema. | In Progress |
| Modulo escritura Supabase | [#13](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/13) | Upsert a staging tables. Testeada con CBRE. ON CONFLICT. | In Progress |
| Testing Pincali validacion | [#26](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/26) | Ejecucion de prueba, datos validados en Supabase. | In Progress |

### Track: Frontend / Tenant Portal (Pablo)

> Sin deliverables en Sprint 1. Auth y frontend se mueven a Sprint 2.

### Track: Design (Pamela)

| Deliverable | Issue | AC | Estado |
|-------------|-------|----|--------|
| Design system en Figma (inicio) | — | Componentes base: cards, tables, buttons, navigation. Referencias para frontend. | En progreso |

---

## Completado previamente (pre-Sprint 1)

Trabajo realizado antes de Mar 2 que contribuye a los milestones de Q1:

| Deliverable | Issue | Estado |
|-------------|-------|--------|
| Job TriggerDev CBRE | [#21](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/21) | ✅ Done |
| Testing CBRE | [#24](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/24) | ✅ Done |
| Job TriggerDev Colliers | [#22](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/22) | ✅ Done |
| Testing Colliers | [#25](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/25) | ✅ Done |
| Job TriggerDev Pincali | [#23](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/23) | ✅ Done |
| Scraper FinSA | [#84](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/84) | ✅ Done |
| Integracion FinSA Supabase | [#85](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/85) | ✅ Done |
| Schema DB documentado | [#41](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/41) | ✅ Done |
| Arquitectura agentes AI | — | ✅ Agent-Architecture.md |
| H3 indexing en produccion | — | ✅ res 5-11, calcula durante scraping |
| PostGIS trigger populate_geo | — | ✅ Funcionando |
| Scoring dashboard Phase 0 | — | ✅ beiqa-frontend |

---

## Riesgos y Dependencias

| Riesgo / Dependencia | Impacto | Mitigacion |
|---------------------|---------|------------|
| Mastra Cloud en beta | Medio | Fallback a Vercel deployer |
| Staging tables requieren schema alignment | Bajo | Fabrizio tiene experiencia con los 5 portales |
| Pablo split entre Mastra + planning | Medio | Sprint 1 solo tiene 2 deliverables de AI |

---

## Review (Mar 15)

> Completar al cierre del sprint.

### Metricas de Cierre

| Metrica | Target | Resultado |
|---------|--------|-----------|
| Issues cerrados | 5-7 | ... |
| KRs completados | 3/3 | ... |

### Retrospectiva

- **Bien**: ...
- **Mejorar**: ...
- **Accion**: ...
