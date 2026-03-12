# sync-repo

Actualiza el repositorio de documentación de Beiqa para reflejar el trabajo reciente en los repos de código. Entrevista rápida al usuario, detecta inconsistencias, y actualiza archivos afectados.

## Cómo invocar

`/sync-repo`

---

## Contexto

Este skill se usa **cada 2-3 días** o cuando Pablo ha trabajado en repos de código (`beiqa-scraper`, `beiqa-frontend`, `beiqa-agents`) y necesita que la documentación refleje el estado real. Es una sincronización rápida, no un planning completo.

---

## Proceso completo

### FASE 1 — Entrevista rápida

Preguntar a Pablo usando AskUserQuestion (máximo 2 bloques):

**Bloque 1 — Trabajo reciente:**
- ¿En qué trabajaste estos últimos días? (¿en qué repo: beiqa-scraper, beiqa-frontend, beiqa-agents?)
- ¿Qué cambios hiciste? (código, config, DB, diseño)
- ¿Se cerró algún issue? ¿Se abrió algo nuevo?

**Bloque 2 — Cambios de contexto:**
- ¿Cambió algo en la arquitectura o el stack?
- ¿Hay decisiones nuevas que documentar? (¿nuevo ADR?)
- ¿Cambió el estado de algún módulo?

### FASE 2 — Detectar cambios necesarios (automático)

Basándose en las respuestas de Pablo:

1. Ejecutar `gh issue list --repo pablo-beiqa/beiqa-real-estate-platform --limit 50 --state open` — comparar con Backlog.md
2. Verificar si el estado de módulos en `01-Modules/README.md` sigue correcto
3. Leer sprint actual (`Sprint-{N}.md`) — ¿hay deliverables que cambiaron de estado?
4. Verificar `02-Architecture/Stack-Decidido.md` si Pablo mencionó cambios de stack

Presentar a Pablo un resumen de lo que necesita actualizarse antes de tocar archivos.

### FASE 3 — Actualizar

Con aprobación de Pablo:

1. **Actualizar archivos afectados**:
   - Estado de módulos en `01-Modules/README.md` y `01-Modules/{Módulo}/README.md`
   - Sprint actual: marcar deliverables completados
   - Backlog.md: actualizar sprint asignado, cerrar completados
   - Stack-Decidido.md si cambió algo
   - ADRs/README.md si hay nuevo ADR

2. **Comentar/cerrar issues en GitHub** (con aprobación):
   - `gh issue close <N> --repo pablo-beiqa/beiqa-real-estate-platform --comment "Completado: {descripción}"`
   - `gh issue comment <N> --repo pablo-beiqa/beiqa-real-estate-platform --body "Progreso: {lo avanzado}"`

3. **Verificar propagación** según `tasks/propagation-rules.md`:
   - Si cambió estado de módulo → CLAUDE.md, README.md
   - Si cambió stack → CLAUDE.md, README.md, Executive-Summary.md
   - Si cambió roadmap → Executive-Summary.md, README.md

4. **Commit** con convención: `docs({scope}): sincronizar estado — {resumen}`

### FASE 4 — Resumen

Presentar al usuario:
- Qué se actualizó y por qué
- Issues cerrados/comentados
- Qué queda pendiente para la próxima sync
- Próxima sync sugerida (en 2-3 días o después del siguiente bloque de trabajo)

---

## Reglas de ejecución

1. **Entrevista CORTA** — máximo 2 bloques de preguntas. No es un planning completo.
2. **Presentar cambios antes de aplicar** — mostrar qué se va a tocar y por qué
3. **No inventar progreso** — solo actualizar lo que Pablo confirma que cambió
4. **Propagación obligatoria** — verificar dependientes según tabla en `tasks/propagation-rules.md`
5. **Pedir aprobación** para cerrar/comentar issues en GitHub
6. **Un commit al final** — agrupar todos los cambios de sync en un solo commit
