---
description: "Ejecuta el protocolo completo de cierre de sesión: resumen, commits pendientes, propagación cross-archivo, lecciones, GitHub Issues, presupuesto, MEMORY.md, CHANGELOG/README, y próximos pasos"
allowed-tools: ["Read", "Glob", "Grep", "Write", "Edit", "Bash", "Task", "AskUserQuestion", "TodoWrite"]
model: "sonnet"
---

# session-close

Ejecuta el protocolo completo de cierre de sesión para el repositorio de documentación de Beiqa. Genera un resumen estructurado, verifica commits, propagación, lecciones, GitHub Issues, presupuesto, CHANGELOG/README, MEMORY.md, y propone próximos pasos. Interactúa con el usuario cuando necesita aprobación.

## Cómo invocar

`/session-close`

---

## Contexto

Este skill implementa el protocolo documentado en `tasks/session-protocol.md` y en la sección "Protocolo de cierre de sesión" de `CLAUDE.md`. Se ejecuta **antes de cerrar cualquier sesión de trabajo**.

---

## Proceso de cierre (9 pasos)

Ejecutar cada paso en orden. Para cada paso, usar las herramientas indicadas, tomar decisiones autónomas donde se permite, y pedir aprobación donde se requiere.

### PASO 1 — Resumen de sesión

**Acción**: Generar un resumen estructurado de la sesión actual.

1. Revisar el historial de la conversación y listar:
   - **Qué se hizo**: cambios, decisiones tomadas, investigación realizada
   - **Qué NO se terminó**: tareas pendientes y por qué
   - **Bloqueantes descubiertos**: problemas que impiden avanzar
2. Presentar el resumen al usuario en formato bullet points

### PASO 2 — Commits y código

**Acción**: Verificar que todos los cambios están committeados correctamente.

1. Ejecutar `git status` (sin `-uall`) para ver archivos sin trackear y cambios no staged
2. Ejecutar `git diff --stat` para ver cambios pendientes
3. Si hay cambios sin committear:
   - Evaluar si deben committearse (¿son parte del trabajo de la sesión?)
   - Si sí: hacer commit siguiendo la convención `tipo(scope): descripción en español`
   - Si no: preguntar al usuario qué hacer
4. Si hay archivos sin trackear que parecen relevantes: preguntar al usuario
5. Verificar que NO hay archivos sensibles (.env, credentials, secrets) staged

**Convención de commits**:
- Formato: `<tipo>(<scope>): <descripción en español>`
- Tipos: `docs`, `feat`, `fix`, `refactor`, `chore`
- Scopes: por carpeta o módulo (`adr`, `arch`, `scraper`, `data`, `modules`, `roadmap`, `budget`)
- Español, lowercase, sin punto final, imperativo
- Incluir `Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>`

### PASO 3 — Propagación cross-archivo

**Acción**: Verificar que los cambios en archivos clave se propagaron a sus dependientes.

1. Leer `tasks/propagation-rules.md` para la tabla de impacto completa
2. Para cada archivo modificado en la sesión, consultar la tabla:

| Si se modificó... | Verificar que se actualizó... |
|-------------------|-------------------------------|
| `Roadmap.md` | Executive-Summary.md, README.md, 01-Modules/README.md |
| `Total-Budget.md` | Executive-Summary.md, Stack-Decidido.md, README.md |
| `Agent-Architecture.md` | Executive-Summary.md, AI-Brain/README.md, System-Architecture.md, 01-Modules/README.md |
| `Stack-Decidido.md` | CLAUDE.md (stack table), README.md, Executive-Summary.md |
| `ADRs/README.md` (nuevo ADR) | CLAUDE.md, README.md, Executive-Summary.md (count) |
| Estado de módulo | 01-Modules/README.md, CLAUDE.md, README.md |
| Equipo | CLAUDE.md, README.md, Executive-Summary.md, Roadmap.md |
| `Schema-Real.md` | README.md, Executive-Summary.md, Roadmap.md |

3. Si hay propagaciones faltantes:
   - **Correcciones menores** (conteos, nombres, estados): aplicar autónomamente
   - **Cambios de significado/estructura**: preguntar al usuario antes de aplicar
4. Reportar: "Propagación completa" o "Propagaciones pendientes: [lista]"

### PASO 4 — Lecciones aprendidas

**Acción**: Documentar correcciones y aprendizajes en `tasks/lessons.md`.

1. Revisar si durante la sesión el usuario corrigió algún error de Claude:
   - Error factual (dato incorrecto)
   - Ajuste de tono o enfoque
   - Decisión incorrecta
   - Patrón descubierto que debería recordarse
2. Si hay correcciones, proponer lección(es) con este formato:

```markdown
#### YYYY-MM-DD — [Título descriptivo]
**Contexto**: [Qué pasó y por qué fue un error]
**Regla**: [La regla concreta que previene el error en el futuro]
```

3. Presentar la(s) lección(es) al usuario para aprobación
4. Si el usuario aprueba: agregar a `tasks/lessons.md` bajo la categoría correspondiente (Documentación, Arquitectura y stack, Proceso y workflow, Comunicación y tono)
5. Si no hubo correcciones: reportar "Sin lecciones nuevas"

### PASO 5 — GitHub Issues

**Acción**: Sincronizar el trabajo con el backlog de GitHub Issues.

1. Ejecutar `gh issue list --repo pablo-beiqa/beiqa-real-estate-platform --limit 10 --state open` para ver issues abiertos
2. Para cada issue que se trabajó en la sesión:
   - **Si se completó**: proponer cerrarlo con `gh issue close <N> --repo pablo-beiqa/beiqa-real-estate-platform --comment "Completado en sesión YYYY-MM-DD: [descripción breve]"`
   - **Si hubo progreso parcial**: proponer comentar con `gh issue comment <N> --repo pablo-beiqa/beiqa-real-estate-platform --body "Progreso: [lo que se avanzó]. Pendiente: [lo que falta]"`
3. Para tareas nuevas descubiertas durante la sesión:
   - **PROPONER** la creación del issue al usuario (nunca crear sin aprobación)
   - Incluir: título, body, labels sugeridos
4. Pedir aprobación del usuario antes de ejecutar cualquier comando `gh`
5. Si no se trabajaron issues: reportar "Sin cambios en issues"

### PASO 6 — Presupuesto (condicional)

**Acción**: Actualizar `04-Validation/Total-Budget.md` si cambió el stack.

1. Evaluar si durante la sesión:
   - Se agregó o removió un servicio del stack
   - Cambió el pricing de un servicio existente
   - Se tomó una decisión que afecta costos
2. Si aplica:
   - Leer `04-Validation/Total-Budget.md`
   - Proponer los cambios al usuario
   - Actualizar si el usuario aprueba
3. Si no aplica: reportar "Sin cambios de presupuesto"

### PASO 7 — MEMORY.md del proyecto

**Acción**: Actualizar el MEMORY.md persistente del proyecto con contexto para futuras sesiones.

1. Leer el archivo `~/.claude/projects/-Users-pablobeitman-Documents-Sync-BEIQA-Producto-Beiqa-Beiqa-Real-Estate-Platform/memory/MEMORY.md`
2. Actualizar con:
   - Fecha de la sesión
   - Resumen conciso de lo que se hizo (1-3 líneas)
   - Estado actual del proyecto (qué módulo/sprint está en progreso)
   - Decisiones tomadas que afectan sesiones futuras
   - Contexto necesario para la próxima sesión
3. Mantener MEMORY.md bajo 200 líneas (se trunca después)
4. Si hay temas detallados, crear archivos separados y linkarlos desde MEMORY.md

### PASO 8 — CHANGELOG y README (condicional)

**Acción**: Actualizar CHANGELOG.md y/o README.md si hubo cambios significativos.

**CHANGELOG.md** — actualizar cuando:
- Se crearon/actualizaron ADRs
- Cambió el stack (nuevo servicio, removido, cambio de pricing)
- Se reorganizaron sprints o milestones
- Se creó infraestructura nueva (tablas, agentes, módulos)
- Se implementaron cambios de proceso (nuevo workflow, protocolo, reglas)

Formato: entrada por fecha con bullet points descriptivos. Seguir el patrón existente del archivo.

**README.md** — actualizar cuando:
- Cambió algo en "Lo que ya está funcionando" o "Próximas prioridades"
- La tabla de stack cambió
- Cambió el estado de un módulo
- Cambió el conteo de ADRs

Si aplica: proponer cambios y aplicar. Mantener fecha de "Última actualización".
Si no aplica: reportar "Sin cambios en CHANGELOG/README"

### PASO 9 — Próximos pasos

**Acción**: Proponer tareas concretas para la siguiente sesión.

1. Basándose en:
   - Tareas pendientes de esta sesión
   - Backlog de GitHub Issues (sprint actual)
   - Roadmap.md (próximo milestone)
   - Bloqueantes identificados
2. Proponer **2-3 tareas concretas** con:
   - Descripción clara
   - Prioridad (alta/media/baja)
   - Repo donde se ejecutaría
   - Issue de GitHub asociado (si existe)
3. Preguntar al usuario si está de acuerdo con las prioridades

---

## Formato de cierre

Al completar todos los pasos, generar este resumen final:

```
=== CIERRE DE SESION ===
Repo: beiqa-real-estate-platform
Fecha: YYYY-MM-DD
Commits: N nuevos (N archivos, +N/-N líneas)
Issues: N cerrados, N comentados, N propuestos
Lessons: N propuestas (N aprobadas)
Propagación: completa | N pendientes
MEMORY.md: actualizado | sin cambios
CHANGELOG: actualizado | sin cambios
Próximos pasos:
  1. [tarea] — [repo] — [prioridad]
  2. [tarea] — [repo] — [prioridad]
  3. [tarea] — [repo] — [prioridad]
========================
```

---

## Reglas de ejecución

1. **Ejecutar TODOS los pasos** — no saltar pasos aunque parezcan innecesarios; reportar "N/A" o "Sin cambios" si no aplican
2. **Pedir aprobación para**:
   - Cerrar o comentar GitHub Issues
   - Crear issues nuevos
   - Agregar lecciones a `tasks/lessons.md`
   - Cambios de presupuesto
3. **Hacer autónomamente**:
   - Verificar `git status` y reportar
   - Leer archivos para verificar propagación
   - Generar resumen de sesión
   - Actualizar MEMORY.md
   - Proponer próximos pasos
4. **Nunca**:
   - Crear archivos nuevos sin preguntar (excepto MEMORY.md)
   - Hacer push sin aprobación explícita del usuario
   - Cerrar issues sin aprobación
   - Inventar datos de progreso o métricas
5. **Si la sesión fue solo de investigación/consulta** (sin cambios a archivos): ejecutar solo Pasos 1, 4, 7, y 9 (resumen, lecciones, memory, próximos pasos)
6. **Convención de commits**: si hay cambios pendientes, usar el formato estándar del proyecto
7. **Cross-repo**: si se trabajó en otro repo (`beiqa-scraper`, `beiqa-frontend`, `beiqa-agents`), usar `gh --repo pablo-beiqa/beiqa-real-estate-platform` para sincronizar issues
