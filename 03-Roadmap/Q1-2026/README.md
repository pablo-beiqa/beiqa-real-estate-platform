# Q1-2026 — Fundaciones

> **Periodo**: Enero - Marzo 2026
> **Sprints**: 1-2
> **Milestones activos**: Scrapers Consolidados, Enrichment Agent Operativo (inicio)

---

## Objetivo del Quarter

Consolidar la capa de datos en Supabase con 5 scrapers produciendo automaticamente, establecer la infraestructura de AI agents con Mastra, y sentar las bases del frontend (scoring dashboard, design system en Figma, vibe coding del portal). Tambien se pone en produccion el indexing geoespacial H3.

---

## OKRs

| Objetivo | Key Results | Status |
|----------|-------------|--------|
| **O1**: Capa de datos consolidada en Supabase | KR1: 5 scrapers produciendo datos en staging tables | ✅ |
| | KR2: ≥28K propiedades en Supabase (I24 + portales custom) | ✅ |
| | KR3: Estructura Supabase definida (32 migrations, PostGIS, RLS) | ✅ |
| | KR4: Modulo de escritura a Supabase reutilizable | 🟡 [#13](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/13) |
| **O2**: Geoespacial operativo | KR5: H3 indexing en produccion (res 5-11, calcula durante scraping) | ✅ |
| | KR6: PostGIS trigger `populate_geo` funcionando | ✅ |
| **O3**: Infraestructura AI agents | KR7: Repo beiqa-agents con Mastra inicializado (`mastra dev`) | 🟡 [#86](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/86) |
| | KR8: Arquitectura de 7 agentes (3 tiers) diseñada y documentada | ✅ |
| **O4**: Fundaciones del frontend | KR9: Scoring dashboard funcional (Phase 0) | ✅ |
| | KR10: Design system iniciado en Figma (Pamela) | 🟡 |
| | KR11: MVP / vibe coding del portal | 🟡 |

---

## Capacidad del Equipo

| Persona | Rol | Split Sprint 1 | Split Sprint 2 |
|---------|-----|-----------------|-----------------|
| **Pablo** | CEO / PO + Dev | 60% AI/Mastra, 20% Frontend, 20% Planning | 40% AI/Mastra, 20% Scrapers, 20% Frontend, 20% Planning |
| **Fabrizio** | Tech Lead | 70% Scraper/Infra, 30% DB/Migrations | 40% Data/Golden Record, 30% Scraper test/deploy, 20% Infra, 10% Learning |
| **Pamela** | Design | Figma designs (shortlist, feedback, dashboard) | Figma designs |

---

## Sprints

| Sprint | Periodo | Milestones | Foco | Estado |
|--------|---------|------------|------|--------|
| [Sprint 01](./Sprint-01.md) | Mar 2-15 | Scrapers Consolidados | Staging tables, Mastra init | Activo |
| [Sprint 02](./Sprint-02.md) | Mar 16-29 | Scrapers Consolidados, Enrichment Agent, Portal Autenticado | Fundaciones + Experimentación: golden record, scrapers nuevos (Cushman + Proximity), enrichment multimodal experiments, auth frontend | Planificado |

---

## Dependencias Externas

- Apify: actor I24 sigue activo mientras se migra a Trigger.dev
- Google Maps Platform: credito $200/mes para Geocoding API
- Mastra Cloud: en beta, gratis — fallback a Vercel si no esta listo
