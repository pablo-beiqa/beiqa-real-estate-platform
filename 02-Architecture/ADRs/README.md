# Registro de Decisiones Arquitectónicas (ADRs)

> Las decisiones arquitectónicas de Beiqa se documentan como ADRs usando el formato [MADR 4.0](https://adr.github.io/madr/) adaptado al proyecto.

---

## Templates

| Template | Cuándo usar |
|----------|------------|
| [ADR-Template-MADR.md](ADR-Template-MADR.md) | Decisiones complejas: 3+ opciones, cross-team, difícil de revertir |
| [ADR-Template-Simple.md](ADR-Template-Simple.md) | Decisiones directas: 1-2 opciones, un solo equipo, fácil de revertir |

---

## Registro

### Aceptados (decisiones tomadas e implementadas)

| # | Título | Costo/mo | Fecha | Decisores |
|---|--------|----------|-------|-----------|
| [ADR-001](ADR-001-Supabase-Plataforma.md) | Supabase como Plataforma (DB + Auth + Storage + API) | $0-25 | 2026-02-04 | Fabrizio, Pablo |
| [ADR-002](ADR-002-Estrategia-Scraping.md) | Estrategia de Scraping (4 portales) | — | 2026-02-25 | Fabrizio, Pablo |
| [ADR-003](ADR-003-Trigger-dev.md) | Trigger.dev — Automatización y Ejecución | Free tier | 2026-02-25 | Fabrizio, Pablo |
| [ADR-004](ADR-004-Rube-MCP-Bridge.md) | Rube — MCP Bridge (interfaz humana) | Incluido | 2026-02-25 | Pablo |
| [ADR-005](ADR-005-HubSpot-CRM.md) | HubSpot CRM — Sync one-way | Plan existente | 2026-02-04 | Pablo |
| [ADR-006](ADR-006-Monitoreo.md) | Monitoreo — Slack + error_logs | $0 | 2026-02-24 | Fabrizio |
| [ADR-007](ADR-007-Firecrawl.md) | Firecrawl — Motor de Scraping | $99 | 2026-02-25 | Fabrizio |
| [ADR-008](ADR-008-Browserbase.md) | Browserbase — Cloud Browser | $20 | 2026-02-25 | Fabrizio |
| [ADR-009](ADR-009-H3-Indexing.md) | H3 Indexing — Sistema hexagonal geoespacial | $0 | 2026-02-27 | Fabrizio, Pablo |
| [ADR-010](ADR-010-AGEB-INEGI.md) | AGEB/INEGI — Polígonos geoespaciales | $0 | 2026-02-27 | Fabrizio, Pablo |
| [ADR-011](ADR-011-Google-Maps-Platform.md) | Google Maps Platform — APIs | ~$0 | 2026-02-25 | Pablo |
| [ADR-012](ADR-012-Multi-Portal-Data.md) | Estrategia Multi-Portal — Staging + Golden Record | $0 | 2026-02-27 | Fabrizio, Pablo |
| [ADR-020](ADR-020-Mastra.md) | Mastra — Framework de Orquestación de Agentes AI | $0 + LLM TBD | 2026-03-05 | Pablo, Fabrizio |
| [ADR-021](ADR-021-Separacion-Trigger-Mastra.md) | Separación de Responsabilidades — Trigger.dev vs Mastra | $0 | 2026-03-05 | Pablo, Fabrizio |
| [ADR-022](ADR-022-Hosting-Vercel-Mastra-Cloud.md) | Hosting Strategy — Vercel (Frontend) + Mastra Cloud (Agents) | $20 + TBD | 2026-03-05 | Pablo, Fabrizio |

### Propuestos (decisiones por tomar)

| # | Título | Cuándo decidir |
|---|--------|---------------|
| ADR-023 | Multi-unit Buildings Data Model — Jerarquía de propiedades en Golden Record | Sprint 2 (bloquea #104, #105). [Issue #133](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/133) |
| [ADR-013](ADR-013-OpenRouter.md) | OpenRouter — AI Routing y Orquestación | Con Mastra activo |
| [ADR-015](ADR-015-Frontend-Next-js.md) | Frontend Next.js — Detalles | Fase 2 |
| [ADR-016](ADR-016-Deduplicacion.md) | Algoritmo de Deduplicación | Con nuevos portales |
| [ADR-017](ADR-017-Plataforma-GIS.md) | Plataforma GIS | Fase 2 |
| [ADR-018](ADR-018-CI-CD.md) | Estrategia CI/CD — GitHub Actions (CI). Frontend deploy resuelto por [ADR-022](ADR-022-Hosting-Vercel-Mastra-Cloud.md) | Con frontend activo |

### Deprecados / Supersedidos

| # | Título | Supersedido por |
|---|--------|----------------|
| [ADR-014](ADR-014-Backboard.md) | Backboard.io — Memoria Persistente AI | [ADR-020](ADR-020-Mastra.md) (Mastra memory) |
| [ADR-019](ADR-019-n8n-Deprecado.md) | n8n Cloud | [ADR-003](ADR-003-Trigger-dev.md) |

---

## Cómo crear un nuevo ADR

1. Elegir template (MADR o Simple) según la complejidad
2. Crear archivo: `ADR-NNN-Titulo-Corto.md`
3. Llenar todas las secciones (sin placeholders vacíos)
4. Agregar a este índice en la categoría correcta
5. Actualizar [Stack-Decidido.md](../Stack-Decidido.md) si afecta costos

O usar el script del ADR skill:
```bash
node .agents/skills/adr-skill/scripts/new_adr.js --title "Mi decisión" --template madr --dir 02-Architecture/ADRs
```

---

## Convenciones

- **Idioma**: Español. Inglés solo para términos técnicos estándar.
- **Numeración**: Secuencial (ADR-001, ADR-002...)
- **Status**: propuesto → aceptado | rechazado | deprecado | supersedido
- **Costo**: Cada ADR incluye `costo-mensual` en el front matter
- **Formato**: MADR 4.0 adaptado (ver templates)
