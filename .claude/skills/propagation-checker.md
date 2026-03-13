---
name: "propagation-checker"
description: "Tabla de reglas de propagación cross-file del repo Beiqa — qué actualizar cuando cambia un archivo clave"
user-invocable: false
---

# propagation-checker

Conocimiento de dominio sobre las reglas de propagación del repositorio Beiqa. Cuando un archivo fuente cambia, sus archivos derivados deben actualizarse.

## Mapa de propagación

| Si cambió... | Actualizar obligatoriamente... | Tipo de cambio |
|-------------|-------------------------------|----------------|
| `03-Roadmap/Roadmap.md` | `05-Communication/Executive-Summary.md`, `README.md`, `01-Modules/README.md` | Resumen de sprints/milestones |
| `04-Validation/Total-Budget.md` | `05-Communication/Executive-Summary.md`, `02-Architecture/Stack-Decidido.md`, `README.md` | Costos totales |
| `02-Architecture/Agent-Architecture.md` | `05-Communication/Executive-Summary.md`, `01-Modules/AI-Brain/README.md`, `02-Architecture/System-Architecture.md`, `01-Modules/README.md` | Estado agentes |
| `02-Architecture/Stack-Decidido.md` | `README.md`, `05-Communication/Executive-Summary.md` | Stack activo (**NUNCA modificar CLAUDE.md desde sync**) |
| `02-Architecture/ADRs/README.md` (nuevo ADR) | `README.md`, `05-Communication/Executive-Summary.md` | Conteo de ADRs |
| `02-Architecture/Database/Schema-Real.md` | `README.md`, `05-Communication/Executive-Summary.md`, `03-Roadmap/Roadmap.md` | Schema DB |
| Estado de módulo en `01-Modules/[X]/README.md` | `01-Modules/README.md`, `README.md` | Estado funcional |
| Equipo en `CLAUDE.md` | `README.md`, `05-Communication/Executive-Summary.md`, `03-Roadmap/Roadmap.md` | Roles |

## Cómo usar

Al modificar cualquier archivo del lado izquierdo, leer el archivo fuente y los archivos derivados, y verificar que los datos relevantes son consistentes. Aplicar correcciones menores autónomamente. Para cambios de significado o estructura: pedir aprobación al usuario.

## Regla crítica

**NUNCA modificar `CLAUDE.md` desde sync-repo o scripts automáticos.** Este archivo se mantiene manualmente por Pablo.

## Fuentes de verdad

| Dato | Archivo fuente |
|------|---------------|
| Sprint activo y milestones | `03-Roadmap/Roadmap.md` |
| Costos operativos | `04-Validation/Total-Budget.md` |
| Decisiones técnicas | `02-Architecture/Stack-Decidido.md` + `ADRs/README.md` |
| Agentes AI | `02-Architecture/Agent-Architecture.md` |
| Estado módulos | `01-Modules/[Módulo]/README.md` (cada uno) |
| Schema DB | `02-Architecture/Database/Schema-Real.md` |
| Equipo y roles | `CLAUDE.md` tabla de equipo |

## Verificación rápida post-cambio

Después de aplicar cambios propagados, verificar con Grep:
1. Que los conteos de ADRs coinciden en README y Executive-Summary
2. Que los costos coinciden entre Total-Budget y Executive-Summary
3. Que los estados de módulos coinciden entre `01-Modules/README.md` y `README.md`
