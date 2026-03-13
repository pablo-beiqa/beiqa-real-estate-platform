# Sprint 1 (Mar 2-15)

> **Estado**: Cerrado
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
- [x] ≥3 staging tables creadas con schema alineado
- [x] Modulo de escritura a Supabase reutilizable y testeado
- [x] Codigo commiteado y pusheado

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
| Staging table CBRE | [#108](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/108) | `cbre_listings` creada. Schema alineado a CbreProperty. | ✅ Done |
| Staging table Colliers | [#109](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/109) | `colliers_listings` creada. Schema alineado. | ✅ Done |
| Staging table Pincali | [#110](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/110) | `pincali_listings` ya existe con 1,761 props. Alinear schema. | ✅ Done |
| Modulo escritura Supabase | [#13](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/13) | Upsert a staging tables. Testeada con CBRE. ON CONFLICT. | ✅ Done |
| Testing Pincali validacion | [#26](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/26) | Ejecucion de prueba, datos validados en Supabase. | ✅ Done |

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
| TriggerDev paid plan + cloud deploy | [#136](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/136) | ✅ Done |
| I24 scraper migrado a Firecrawl stealth (Apify desactivado Mar 10) | — | ✅ Done |
| Pincali biweekly cron + Slack notifications | — | ✅ Done |

---

## Riesgos y Dependencias

| Riesgo / Dependencia | Impacto | Mitigacion |
|---------------------|---------|------------|
| Mastra Cloud en beta | Medio | Fallback a Vercel deployer |
| Staging tables requieren schema alignment | Bajo | Fabrizio tiene experiencia con los 5 portales |
| Pablo split entre Mastra + planning | Medio | Sprint 1 solo tiene 2 deliverables de AI |

---

## Review (Mar 15)

### Métricas de Cierre

| Métrica | Target | Resultado |
|---------|--------|-----------|
| Issues cerrados (sprint) | 5-7 | 6 (#108, #109, #110, #26, #136, #13) + pre-sprint (#21-25, #84, #85) |
| KRs completados | 3/3 | 2/3 (KR1 parcial → S2) |

### OKRs — Evaluación

| Objetivo | Key Result | Estado | Notas |
|----------|------------|--------|-------|
| **O1: Infraestructura AI agents** | KR1: `beiqa-agents` repo con `mastra dev` funcionando | 🟡 Parcial | Repo creado, mastra dev corre. Sin estructura de agentes. Carryover a S2 como #86. |
| **O2: Scrapers persisten con staging tables** | KR2: ≥3 staging tables creadas (CBRE, Colliers, Pincali) | 🟢 Completo | #108, #109, #110 cerrados Mar 12. |
| O2 | KR3: Módulo de escritura a Supabase funcional | 🟢 Completo | #13 cerrado. Módulo reutilizable y testeado con CBRE. |

> 🟢 ≥80% completado / 🟡 40-79% / 🔴 <40% o no iniciado

### Deliverables — Estado Final

| Deliverable | Issue | Estado | Causa (si incompleto) |
|-------------|-------|--------|----------------------|
| Staging table CBRE | #108 | ✅ Completo | - |
| Staging table Colliers | #109 | ✅ Completo | - |
| Staging table Pincali | #110 | ✅ Completo | - |
| Módulo escritura Supabase | #13 | ✅ Completo | - |
| Testing Pincali validación | #26 | ✅ Completo | - |
| TriggerDev paid plan + cloud deploy | #136 | ✅ Completo | Adelantado (completado antes del sprint formal) |
| beiqa-agents: Mastra init | #86 | ⚠️ Parcial | Priorización deliberada: tiempo ocupado en scrapers/infra/trabajo de clientes. Carryover a S2. |
| Address Enrichment skeleton | #87 | ❌ No iniciado | Postergado — reemplazado por enrichment experiments en S2 |

**Completado fuera del plan original:**
- I24 scraper migrado a Firecrawl (Apify desactivado Mar 10)
- FinSA scraper + integración Supabase (#84, #85)
- Pincali biweekly cron + Slack notifications

### Retrospectiva

- **Bien**: Capacidad de adaptación a cambios imprevistos (I24 migration, TriggerDev). Fabrizio ejecutó infra rápido. El equipo pivotó sin conflicto cuando surgieron urgencias.
- **Mejorar**: Sprint planning sin estimación de esfuerzo ni capacity real por persona. Trabajo de clientes/ops desplazó trabajo del sprint sin estar documentado. Pablo en 4 tracks simultáneos genera carryover acumulado.
- **Acción**: Sprint 2 arranca con capacity real (días disponibles × 0.8) y buffer explícito para imprevistos + aprendizaje. Pamela migra a GitHub Issues para mayor visibilidad.

### Autoevaluación del Equipo

| Persona | Puntuación | Fortaleza | Área de mejora |
|---------|------------|-----------|----------------|
| Pablo | 60/100 | Adaptabilidad, visión de producto | Estimación de carga, reducir trabajo de clientes, foco en construir |
| Fabrizio | 55/100 | Ejecución de código y DB | Investigación, decisiones técnicas, planeamiento |
| Pamela | 70/100 | Diseño en Figma | Agilidad, ownership de deliverables, migrar a GitHub |

### Carryover a Sprint 2

| Issue | Deliverable | Razón del Carryover | Destino |
|-------|-------------|---------------------|---------|
| #86 | beiqa-agents init + estructura de agentes | KR1 parcial — init básico hecho, falta estructura de agentes implementada | S2 (ya está en plan) |
| (sprint planning S2) | Visualización/análisis de datos scrapeados | Contemplado pero no planeado en S1 | Crear en sprint planning S2 |
| (sprint planning S2) | Brochures/templates por ICP + presentaciones | Trabajo ops; Pamela lidera como primer issue en GitHub | Crear en sprint planning S2 |

### Lecciones Aprendidas en este Sprint

1. **Capacity planning**: El sprint debe planificarse con capacity real por persona (días disponibles × 0.8), reservando el 20% para trabajo imprevisto, bugs, y trabajo de cliente no anticipado.

2. **Bloque de aprendizaje como deliverable formal**: El tiempo de aprendizaje (Mastra, GIS, nuevas herramientas) debe estar en el sprint plan explícitamente. Si no está en el plan, siempre se sacrifica.
