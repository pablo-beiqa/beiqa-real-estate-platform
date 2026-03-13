---
name: "doc-formatter"
description: "Estándares de formato para documentos del repo Beiqa: ADRs, sprint files, backlog, módulos"
user-invocable: false
---

# doc-formatter

Estándares de formato y convenciones para el repositorio de documentación Beiqa.

## Convenciones generales

### Emojis de estado
- 🟢 = En desarrollo / Activo / En implementación
- 🟡 = En diseño / En pruebas / Draft
- 🔴 = Por iniciar
- ✅ = Completado / Producción

**Regla**: emoji y texto deben ser coherentes. 🟢 + "En diseño" = error.

### Commits
```
<tipo>(<scope>): <descripción en español>

Co-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>
```
- Tipos: `docs`, `feat`, `fix`, `refactor`, `chore`
- Scopes: `adr`, `arch`, `scraper`, `data`, `modules`, `roadmap`, `budget`, `sync`, `skills`
- Español, lowercase, imperativo, sin punto final

## ADRs — Formato MADR

Leer `02-Architecture/ADRs/ADR-Template-MADR.md` para el template completo. Campos obligatorios en frontmatter:

```yaml
---
status: "Propuesto" | "Aceptado" | "Deprecado" | "Supersedido por ADR-XXX"
date: YYYY-MM-DD
decision-makers: ["Pablo", "Fabrizio"]
costo-mensual: "$X/mes" | "Sin costo adicional" | "TBD"
---
```

Secciones obligatorias:
1. Contexto y problema
2. Factores de decisión
3. Opciones consideradas (mínimo 2)
4. Decisión tomada + justificación
5. Consecuencias positivas y negativas
6. Plan de implementación (si aplica)

**NUNCA crear ADRs stub o placeholder** — si se crea un ADR, debe ser completo.

Al crear un ADR:
1. Número = siguiente secuencial (leer `ADRs/README.md` para determinar)
2. Nombre de archivo: `ADR-{NNN}-{kebab-case-nombre}.md`
3. Agregar entrada en `ADRs/README.md` tabla correspondiente
4. Si depreca/supersede otro ADR: actualizar status del ADR anterior

## Sprint files — Formato

Leer `03-Roadmap/Q1-2026/Sprint-02.md` como ejemplo canónico. Campos obligatorios en el header:

```markdown
# Sprint {N} ({fecha inicio}-{fecha fin}) — {Tema}

> **Estado**: Planificado | Activo | Cerrado
> **Quarter**: Q{X}-2026
> **Tema**: {descripción corta}
> **Milestones**: {lista}
```

Secciones obligatorias: OKRs, Definition of Done, Deliverables por Track, Riesgos, Review.

La sección Review solo se llena al cerrar el sprint. Antes del cierre, contiene solo: `> Completar al cierre del sprint.`

## Backlog.md — Formato de filas

Cada issue en Backlog.md:
```
| [#{N}](URL) | {título} | {área} | `{label}` | S{N} / Backlog / TBD |
```

URLs de issues: `https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/{N}`

## Módulos — Formato de README

Cada `01-Modules/[Módulo]/README.md` debe tener:
```markdown
## Estado: {emoji} {texto}
## Sprint del proyecto: S{N}
## Owner: {persona}
```

## Tablas de módulos en README.md y 01-Modules/README.md

Columnas estándar:
```
| Módulo | Estado | Sprint | Owner |
```

## Números y métricas

- Presupuesto: usar siempre el rango `$747–$896 USD/mes`
- Volumen I24: `~30K propiedades` (NO 60K)
- Migrations: `14+`
- No inventar métricas sin fuente verificable

## Idioma

- Español por defecto en toda la documentación
- Términos técnicos en inglés: API, schema, endpoint, deploy, staging, migration, trigger, hook, scraper, pipeline, etc.
- Nombres propios en inglés: Supabase, Trigger.dev, Mastra, HubSpot, Vercel, Firecrawl, Google Maps
