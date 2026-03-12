# Reglas de Propagación de Cambios

> Cuando Claude modifica un archivo clave, debe verificar y actualizar los archivos dependientes listados aquí.
> Este archivo es la referencia para mantener consistencia cross-document.

---

## Tabla de impacto

| Si cambia... | Actualizar |
|-------------|-----------|
| **Roadmap.md** (milestones, sprint actual, OKRs) | Executive-Summary.md, README.md, 01-Modules/README.md |
| **Total-Budget.md** (costos, proyecciones) | Executive-Summary.md, Stack-Decidido.md, README.md |
| **Agent-Architecture.md** (agentes, tools, prioridad) | Executive-Summary.md, AI-Brain/README.md, System-Architecture.md, 01-Modules/README.md |
| **Stack-Decidido.md** (nueva tecnología, cambio de estado) | CLAUDE.md, README.md, Executive-Summary.md |
| **ADRs/README.md** (nuevo ADR agregado) | CLAUDE.md, README.md, Executive-Summary.md |
| **Estado de un módulo** (sprint, estado, owner) | 01-Modules/README.md, README.md |
| **Equipo** (persona entra/sale, cambio de rol) | CLAUDE.md, README.md, Executive-Summary.md, Roadmap.md (capacidad) |
| **Schema-Real.md** (nueva migration) | README.md, Executive-Summary.md, Roadmap.md (count de migrations) |
| **session-protocol.md** (nuevo paso, cambio de proceso) | tasks/todo.md (checklist) |
| **lessons.md** (nueva lección que afecta regla estricta) | CLAUDE.md (si modifica comportamiento) |

---

## Fuentes de verdad

Cada dato tiene **una sola fuente de verdad**. Los demás archivos solo resumen o referencian.

| Dato | Fuente de verdad |
|------|------------------|
| Sprints, milestones, OKRs | `03-Roadmap/Roadmap.md` |
| Costos operativos | `04-Validation/Total-Budget.md` |
| Stack y decisiones técnicas | `02-Architecture/Stack-Decidido.md` |
| Diseño de agentes AI | `02-Architecture/Agent-Architecture.md` |
| Estado de cada módulo | `01-Modules/[Módulo]/README.md` + `Roadmap.md` |
| Schema de DB | `02-Architecture/Database/Schema-Real.md` |
| Equipo y roles | `00-Project/Stakeholders.md` + `CLAUDE.md` |
| Visión y métricas | `00-Project/Vision-and-Goals.md` |

---

## Datos que más se replican (mayor riesgo de desincronización)

1. **Tabla de módulos** (sprint, estado, owner) — aparece en: README.md, 01-Modules/README.md
2. **Stack table** — aparece en: README.md, Executive-Summary.md, Stack-Decidido.md
3. **Presupuesto** ($xxx–$yyy/mes) — aparece en: Total-Budget.md, Executive-Summary.md, README.md
4. **Count de ADRs** — aparece en: README.md, Executive-Summary.md
5. **Milestones y fechas** (M1-M4) — aparece en: Roadmap.md, Executive-Summary.md

---

*Última actualización: 2026-03-11*
