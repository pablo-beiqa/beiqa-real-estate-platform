# Q2-2026 — Inteligencia + Portal

> **Periodo**: Abril - Junio 2026
> **Sprints**: 3-8
> **Milestones activos**: Enrichment Agent Operativo, Portal Autenticado, Design System Implementado, Golden Record Pipeline, Scoring Automatizado, Portal con Shortlists, Inteligencia Geoespacial, Busqueda Inteligente, Inteligencia de Mercado, Portal en Produccion, Dashboard Interno, Operacion Estable

---

## Objetivo del Quarter

Poner en operacion los 4 agentes AI core (enrichment, normalization, dedup, scoring), lanzar el Tenant Portal en produccion con auth, shortlists y feedback, y establecer inteligencia geoespacial y de mercado. Al cerrar Q2, al menos 1 tenant corporativo esta usando el portal activamente.

---

## OKRs

| Objetivo | Key Results |
|----------|-------------|
| **O1**: AI Agents operativos (core 4) | KR1: Address Enrichment Agent con >80% accuracy |
| | KR2: Data Normalization Agent para ≥3 portales |
| | KR3: Deduplication Agent con >95% precision |
| | KR4: Property Search & Match Agent generando shortlists automaticamente |
| | KR5: ≥40K propiedades enriquecidas y normalizadas |
| **O2**: Tenant Portal en produccion | KR6: Auth magic link + RLS (aislamiento por tenant) |
| | KR7: Design system implementado (Figma → componentes shadcn/ui) |
| | KR8: Scoring pages leyendo de Supabase via Mastra |
| | KR9: Shortlist UI con feedback estructurado (Si/No/Quiza + razon) |
| | KR10: portal.beiqa.com deployado en Vercel |
| | KR11: ≥1 tenant activo usando el portal |
| **O3**: Inteligencia geoespacial | KR12: H3 indexer post-enrichment calculando para golden record |
| | KR13: AGEB shapefiles importados + spatial join operativo |
| | KR14: GIS Analysis Agent con zone quality score |
| **O4**: Inteligencia de mercado | KR15: Market Intelligence Agent operativo |
| | KR16: Reportes de precio/m² por zona automaticos |
| | KR17: Comparables finder funcional |
| **O5**: Pipeline de datos confiable | KR18: Golden record pipeline E2E (staging → properties) |
| | KR19: Data quality monitoring con alertas |
| | KR20: I24 migrado completamente a Trigger.dev (Apify+Clay eliminados) |
| | KR21: HubSpot sync bidireccional |
| **O6**: Infraestructura y operaciones | KR22: Alertas Slack para errores de scrapers y agentes |
| | KR23: CI/CD pipeline (GitHub Actions) |
| | KR24: Cost tracking por agente (agent_runs) |
| | KR25: API rate limiting / cost caps para Google + LLMs |
| | KR26: Evals definidos y ejecutandose para los 4 agentes core |

---

## Capacidad del Equipo

| Persona | Rol | Split estimado |
|---------|-----|---------------|
| **Pablo** | CEO / PO + Dev | 40% AI/Mastra, 35% Frontend/Portal, 25% Planning |
| **Fabrizio** | Tech Lead | 40% Scraper/Infra, 30% DB/Pipeline, 20% AI/Agents, 10% DevOps |
| **Pamela** | Design | Figma designs → handoff componentes (Tenant Portal, Internal App) |

---

## Sprints

| Sprint | Periodo | Milestones que cierra | Foco | Estado |
|--------|---------|----------------------|------|--------|
| [Sprint 03](./Sprint-03.md) | Mar 30 - Abr 12 | Enrichment Agent, Portal Autenticado | Backfill, Normalization inicio, RLS, design system | Planificado |
| [Sprint 04](./Sprint-04.md) | Abr 13-26 | Design System, Golden Record Pipeline | Normalization E2E, scoring pages, AGEB | Planificado |
| [Sprint 05](./Sprint-05.md) | Abr 27 - May 10 | Scoring Automatizado | Property Search & Match Agent, dedup inicio, shortlist UI, Vercel | Planificado |
| [Sprint 06](./Sprint-06.md) | May 11-24 | Portal con Shortlists, Int. Geoespacial, Busqueda Inteligente | Feedback UI, GIS Agent, dedup finish | Planificado |
| [Sprint 07](./Sprint-07.md) | May 25 - Jun 7 | Int. de Mercado, Portal en Produccion | Market Intel Agent, portal polish, CI/CD | Planificado |
| [Sprint 08](./Sprint-08.md) | Jun 8-21 | Dashboard Interno, Operacion Estable | Internal App, evals, monitoring, docs | Planificado |

---

## Dependencias Externas

- Mastra Cloud: hosting de agentes (beta, gratis → pricing TBD). Fallback: Vercel
- Google Maps Platform: credito mensual para geocoding masivo
- Clientes activos: necesitamos ≥1 tenant dispuesto a usar el portal para KR11
- Pamela: designs de Figma deben estar listos antes de que Pablo los implemente (Sprint 3-4)
