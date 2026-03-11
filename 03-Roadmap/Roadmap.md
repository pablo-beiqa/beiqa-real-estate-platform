# Roadmap — Beiqa Platform

> **Actualizado**: 2026-03-11 | **Metodología**: Scrum (sprints de 2 semanas, cross-cutting)
>
> **North Star**: Pipeline automatizado que genera shortlists con scoring desde Supabase para tenants corporativos.
>
> **Backlog**: [Backlog.md](Backlog.md) | **GitHub Issues**: [beiqa-real-estate-platform](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues) | **Project Board**: [Beiqa Product](https://github.com/users/pablo-beiqa/projects/1)

---

## OKRs H1 2026

| Quarter | Tema | Objetivos clave |
|---------|------|-----------------|
| **Q1** (Ene-Mar) | Fundaciones | Scrapers en Supabase, PostGIS+H3, Mastra agents inicio, frontend Phase 0 |
| **Q2** (Abr-Jun) | Inteligencia + Portal | 4 agentes AI core, tenant portal live, golden record E2E, inteligencia geoespacial y de mercado |

Detalle completo: [Q1-2026/](Q1-2026/README.md) | [Q2-2026/](Q2-2026/README.md)

---

## Milestones

| # | Milestone | Outcome | Due | Sprint(s) |
|---|-----------|---------|-----|-----------|
| 1 | **Scrapers Consolidados** | 5 portales con staging tables en Supabase | Mar 29 | 1-2 |
| 2 | **Enrichment Agent Operativo** | Address Enrichment procesando E2E con confidence score | Abr 12 | 2-3 |
| 3 | **Portal Autenticado** | Login magic link, RLS, aislamiento por tenant | Abr 12 | 2-3 |
| 4 | **Design System Implementado** | Figma → componentes shadcn/ui usados en portal | Abr 26 | 3-4 |
| 5 | **Golden Record Pipeline** | Staging → normalización → properties E2E | Abr 26 | 3-4 |
| 6 | **Scoring Automatizado** | Property Search & Match Agent genera shortlists, frontend consume API | May 10 | 4-5 |
| 7 | **Portal con Shortlists** | UI de shortlists + feedback + mapa | May 24 | 5-6 |
| 8 | **Inteligencia Geoespacial** | H3 post-enrichment, AGEB, GIS Agent, zone quality | May 24 | 5-6 |
| 9 | **Búsqueda Inteligente** | Dedup >95%, datos unificados cross-portal | May 24 | 5-6 |
| 10 | **Inteligencia de Mercado** | Market Intel Agent, precio/m², comparables, reportes | Jun 7 | 6-7 |
| 11 | **Portal en Producción** | Vercel live, ≥1 tenant activo, feedback loop | Jun 7 | 6-7 |
| 12 | **Dashboard Interno** | Internal App (app.beiqa.com) leyendo golden record | Jun 21 | 7-8 |
| 13 | **Operación Estable** | CI/CD, evals, monitoring, docs operativas | Jun 21 | 7-8 |

---

## Quarters

| Quarter | Periodo | Sprints | Estado |
|---------|---------|---------|--------|
| [Q1-2026](Q1-2026/README.md) | Ene - Mar | [Sprint 1](Q1-2026/Sprint-01.md), [Sprint 2](Q1-2026/Sprint-02.md) | 🟢 Activo |
| [Q2-2026](Q2-2026/README.md) | Abr - Jun | [Sprint 3](Q2-2026/Sprint-03.md) — [Sprint 8](Q2-2026/Sprint-08.md) | 🟡 Planificado |
| [Q3-2026](Q3-2026/README.md) | Jul - Sep | TBD | 🔴 Visión |

---

## Referencias

- **Backlog completo**: [Backlog.md](Backlog.md) (issues agrupados por módulo)
- **Templates**: [Sprint](templates/Sprint-Template.md) | [Quarter](templates/Q-README-Template.md)
- **Archivo original**: [archive/Roadmap-Monolitico-v1.md](archive/Roadmap-Monolitico-v1.md)
- **Stack decidido**: [Stack-Decidido.md](../02-Architecture/Stack-Decidido.md)
- **Arquitectura agentes**: [Agent-Architecture.md](../02-Architecture/Agent-Architecture.md)

---

*Source of truth para estado de issues: [GitHub Project Board](https://github.com/users/pablo-beiqa/projects/1). Este archivo es un índice de navegación.*
