# sync-repo

Sincroniza el repositorio de documentación de Beiqa con el trabajo real del equipo. Entrevista dinámica y profunda al usuario, verifica repos de código, detecta drift entre documentación y realidad, y ejecuta todas las actualizaciones necesarias — incluyendo ADRs completos, lecciones aprendidas, y creación de issues nuevos.

## Cómo invocar

`/sync-repo`

---

## Contexto

Este skill es el mecanismo de sincronización continua del repositorio. Se ejecuta **cada 2-3 días** o después de un bloque significativo de trabajo en cualquier repo de código (`beiqa-scraper`, `beiqa-frontend`, `beiqa-agents`). Es parte de la trilogía del sprint: `/sprint-planning` → `/sync-repo` (repetir) → `/sprint-retro`.

Puede ser ejecutado por **Pablo o Fabrizio** — el skill se adapta al dominio de cada persona. No es un planning: documenta lo que YA pasó.

---

## Mecánica de confianza

La entrevista usa **AskUserQuestion** (nunca texto libre) y un sistema de confianza explícito. Si la confianza es **<95**, el skill:

1. Reporta el score: `"Confianza: [N]/100 — necesito saber más sobre X, Y."`
2. Lista qué información falta o es ambigua
3. Genera preguntas adicionales con AskUserQuestion (dinámicas, basadas en lo que falta)
4. Repite hasta ≥95/100

Solo cuando confianza ≥95, el skill avanza a la siguiente fase. Si el usuario dice "no recuerdo" o "avanza", documentar los aspectos sin confirmar como **(pendiente de verificar)** y proceder.

### Factores de confianza por fase

**Después de FASE 3** (entendimiento del trabajo realizado — máximo: 100):

| Factor | Puntaje |
|--------|---------|
| Repos identificados con claridad | +15 |
| Tipo de trabajo confirmado por repo (código, DB, infra, diseño) | +15 |
| Estado de deliverables del sprint confirmado | +20 |
| Decisiones arquitectónicas identificadas o descartadas | +15 |
| Issues impactados identificados (cerrar, crear, editar) | +15 |
| Lecciones aprendidas preguntadas y respondidas | +10 |
| Stack o dependencias nuevas confirmadas o descartadas | +10 |
| Respuesta vaga sin follow-up ("hice algo", "avancé") | -15 |
| Módulo afectado sin confirmar estado | -10 |
| Decisión técnica mencionada sin detalle | -10 |

**Después de FASE 5** (confianza en el impacto detectado — máximo: 100):

| Factor | Puntaje |
|--------|---------|
| Todos los deliverables activos con estado propuesto | +25 |
| Issues a ejecutar claramente mapeados | +25 |
| Módulos con cambio de estado evaluados | +20 |
| ADR/Stack changes evaluados | +15 |
| Propagación mapeada según `tasks/propagation-rules.md` | +15 |
| Deliverable con estado ambiguo | -15 |
| Issue sin contexto suficiente para ejecutar | -10 |
| Propagación no verificada | -10 |

---

## Proceso completo

### FASE 1 — Recolectar contexto (automático, silencioso)

Ejecutar antes de hacer cualquier pregunta. No reportar progreso al usuario.

Leer en paralelo:
1. `03-Roadmap/Roadmap.md` — identificar sprint activo y quarter
2. Sprint activo: `03-Roadmap/Q{X}-2026/Sprint-{N}.md` — deliverables, estados, OKRs
3. `01-Modules/README.md` — snapshot de estado de módulos
4. `03-Roadmap/Backlog.md` — mapeo issue-sprint
5. `02-Architecture/ADRs/README.md` — índice de ADRs existentes
6. `02-Architecture/Stack-Decidido.md` — stack actual para drift detection
7. `tasks/lessons.md` — lecciones existentes (evitar duplicar)
8. `tasks/propagation-rules.md` — guardar para FASE 6

Ejecutar:
```bash
gh issue list --repo pablo-beiqa/beiqa-real-estate-platform --limit 200 --state open
```

Guardar todo internamente como baseline para drift detection en FASE 5.

---

### FASE 2 — Identificar al usuario (AskUserQuestion)

Usar AskUserQuestion:
- **question**: "¿Quién está corriendo este sync?"
- **header**: "Usuario"
- **options**: Pablo (scrapers dev, AI, frontend, producto) | Fabrizio (schema, infra, testing, deploy)

Adaptar las preguntas de FASE 3 según el dominio:

| Usuario | Dominio principal | Profundizar en... |
|---------|------------------|-------------------|
| **Pablo** | Scrapers (dev), AI experiments, frontend, producto | Portales, templates, Mastra, agentes, páginas, decisiones de producto |
| **Fabrizio** | Schema, migrations, RLS, infra, testing, deploy | Tablas, H3/PostGIS, TriggerDev, staging tables, triggers, Supabase config |

---

### FASE 3 — Entrevista dinámica (AskUserQuestion + loop de confianza)

**Toda pregunta al usuario usa AskUserQuestion.** Nunca texto libre. Máximo 4 preguntas por call.

**Pregunta semilla** (ronda 1 — siempre la misma):

Usar AskUserQuestion:
- **question**: "¿En qué repo(s) trabajaste estos últimos días?"
- **header**: "Repo"
- **options**: beiqa-scraper | beiqa-frontend | beiqa-agents | Múltiples repos

**Follow-ups dinámicos** (rondas 2+) — el agente genera las opciones basándose en la respuesta anterior. Profundizar en cada área mencionada con opciones relevantes al contexto. Ejemplos de profundidad por categoría:

| Si menciona... | Opciones sugeridas en el siguiente AskUserQuestion |
|----------------|---------------------------------------------------|
| Scraper | ¿Qué portal? / ¿Staging table creada? / ¿TriggerDev job deployed? / ¿H3 + triggers? |
| Schema/DB | ¿Tablas creadas? / ¿Migrations aplicadas? / ¿RLS configurado? / ¿Datos ingested? |
| Frontend | ¿Qué páginas/rutas? / ¿Auth/middleware? / ¿Componentes nuevos? / ¿Deploy a Vercel? |
| AI/Mastra | ¿Qué agente? / ¿Modelos probados? / ¿Resultados documentados? / ¿Costo estimado? |
| Decisión arquitectónica | ¿Qué se decidió? / ¿Alternativas evaluadas? / ¿Merece ADR? / ¿Quién participó? |
| Stack change | ¿Nueva dep o servicio? / ¿Depreca algo? / ¿Impacto en costo? / ¿Documentado? |
| Bug fix | ¿Causa raíz identificada? / ¿Issue cerrado? / ¿Afecta otros módulos? |
| Tests | ¿Tipo de tests? / ¿Cobertura? / ¿Bugs encontrados y trackeados? |
| Infra/DevOps | ¿TriggerDev? / ¿Supabase config? / ¿Variables de entorno nuevas? |

No preguntar sobre temas fuera del dominio del usuario (no preguntar a Fabrizio sobre decisiones de producto; no preguntar a Pablo sobre RLS en detalle).

**Loop de confianza** — antes de cada ronda de preguntas:
1. Reportar: `"Confianza: [N]/100 — necesito saber más sobre X, Y."` (usando factores de la sección Mecánica de confianza)
2. Lanzar AskUserQuestion con las preguntas más críticas para subir la confianza
3. Repetir hasta ≥95/100

**Fallback**: Si el usuario dice "no recuerdo", "no sé" o "avanza" — documentar el aspecto como **(pendiente de verificar)** y proceder.

**Pregunta obligatoria de cierre** (SIEMPRE, última ronda — AskUserQuestion):
- **question**: "¿Alguna lección aprendida estos días? ¿Algo que descubrieron que no sabían, un error que no quieren repetir, o un patrón que funcionó bien?"
- **header**: "Lecciones"
- **options**: Sí, hay algo que documentar | No hay lecciones esta vez | Hay varias cosas

**Sin límite de rondas**: 2 rondas si el trabajo fue simple, 8-10 si hubo cambios complejos. La profundidad la dicta el contenido. Solo avanzar a FASE 4 cuando confianza ≥95.

---

### FASE 4 — Verificación de repos (automático + interactivo)

Verificar los repos de código que el usuario mencionó en la entrevista.

**4.1 — Determinar rutas de repos**

Intentar acceso local primero (directorios hermanos):
```bash
ls "../Beiqa Scraper/"        # beiqa-scraper
ls "../Beiqa Frontend/"       # beiqa-frontend
ls "../Beiqa AI Architecture Mastra/"  # beiqa-agents
```

Si no están locales, usar GitHub API como fallback:
```bash
gh api repos/pablo-beiqa/{repo-name}/commits --jq '.[0:10] | .[] | {sha: .sha[0:7], message: .commit.message, date: .commit.author.date}'
```

**4.2 — Scan completo por repo mencionado**

Para cada repo que el usuario mencionó:
- `git log --oneline -20` (o API equivalente) — commits recientes
- Leer `package.json` — dependencias cambiaron?
- Leer `README.md` — actualizado?
- Leer `.env.example` — variables nuevas?
- `gh pr list --repo pablo-beiqa/{repo}` — PRs abiertos
- `gh issue list --repo pablo-beiqa/{repo} --limit 20 --state open` — issues del repo

**4.3 — Cross-reference**

Comparar lo dicho en entrevista vs lo encontrado en repos:
- ¿Git log confirma lo que el usuario dijo?
- ¿Hay commits que el usuario NO mencionó? → Preguntar: "Vi commits sobre X en {repo} que no mencionaste. ¿Es relevante para este sync?"
- ¿Hay PRs abiertos que deberían mencionarse?

**REGLA CRÍTICA**: La palabra del usuario tiene prioridad sobre los repos. Los repos pueden no estar pusheados. Si el usuario dice que trabajó en algo pero git log no lo muestra, confiar en el usuario y notar "(no pusheado aún)" en las actualizaciones.

---

### FASE 5 — Drift detection + propuestas (automático + aprobación)

**Antes de presentar propuestas**, evaluar confianza en el impacto detectado usando la tabla de FASE 5 de la sección Mecánica de confianza. Si confianza <95: reportar `"Confianza en impacto: [N]/100 — necesito clarificar X."` y hacer preguntas adicionales con AskUserQuestion. Solo cuando confianza ≥95, presentar el bloque de propuestas.

Comparar estado documentado (FASE 1) vs estado real (FASE 3 + FASE 4) en 6 dimensiones. Construir un **mapa completo de impacto** y presentar TODAS las propuestas agrupadas para aprobación.

**5a. Sprint deliverables**

Para cada deliverable en Sprint-{N}.md, determinar si el estado debe cambiar:

```
| Deliverable | Estado documentado | Estado propuesto | Evidencia |
|-------------|-------------------|-----------------|-----------|
```

Usar los mismos valores de estado que el sprint actual (Pendiente / En progreso / Completado). Actualizar checkboxes de Definition of Done si aplica.

**5b. Estado de módulos**

Comparar `01-Modules/README.md` contra lo reportado. Proponer cambios de estado si corresponde.

**5c. GitHub Issues — control absoluto**

El skill tiene control total sobre issues. Cualquier operación que `gh issue` permita está disponible:
- **Cerrar / Reabrir**: issues completados o que necesitan reabrirse
- **Comentar**: notas de progreso, contexto adicional, decisiones
- **Crear nuevos**: trabajo descubierto que no tiene issue. Proponer título, body, labels, milestone, assignees
- **Editar**: cambiar título, body, labels, milestone, assignees de issues existentes
- **Mover de sprint**: cambiar la iteración/sprint en el Project Board
- **Cambiar prioridad**: actualizar labels de prioridad (P0, P1, P2)

Proponer cada cambio con justificación antes de ejecutar.

**5d. ADRs**

Si el usuario mencionó una decisión arquitectónica:
1. Determinar el siguiente número de ADR (leer ADRs/README.md)
2. Proponer ADR completo en formato MADR: frontmatter (status, date, decision-makers, costo-mensual), contexto, factores, opciones, decisión, consecuencias, plan de implementación
3. Si el ADR nuevo depreca/supersede uno existente, incluir el cambio al ADR anterior
4. Leer `02-Architecture/ADRs/ADR-Template-MADR.md` como referencia de formato

**5e. Stack changes**

Si el usuario mencionó cambios de stack, proponer actualizaciones específicas a `02-Architecture/Stack-Decidido.md`.

**5f. Lecciones aprendidas**

Basado en la pregunta obligatoria de cierre de FASE 3, proponer lecciones en el formato exacto de `tasks/lessons.md`:

```markdown
#### YYYY-MM-DD — {Título breve}
**Contexto**: {contexto completo del error/situación}
**Regla**: {regla específica y actionable}
```

Asignar categoría: Documentación / Arquitectura y stack / Proceso y workflow / Comunicación y tono.

**Presentar TODO como un bloque de aprobación**: "Estos son los cambios que propongo. ¿Apruebas todos, o quieres ajustar alguno?"

---

### FASE 6 — Ejecutar cambios (con aprobación)

Ejecutar solo lo aprobado en FASE 5, en este orden:

1. **Sprint-{N}.md** — actualizar estados de deliverables + checkboxes de DoD
2. **01-Modules/README.md** + READMEs individuales de módulos si aplica
3. **Backlog.md** — actualizar sprint asignado, marcar completados
4. **ADR completo** (si aprobado):
   - Crear `02-Architecture/ADRs/ADR-{NNN}-{nombre}.md` con todas las secciones MADR
   - Actualizar `02-Architecture/ADRs/README.md` con nueva entrada en tabla
   - Si depreca/supersede otro ADR: actualizar status del ADR anterior
5. **Stack-Decidido.md** — aplicar cambios aprobados
6. **tasks/lessons.md** — agregar lecciones bajo la categoría correcta
7. **GitHub Issues — control absoluto**:
   ```bash
   # Cerrar / Reabrir
   gh issue close {N} --repo pablo-beiqa/beiqa-real-estate-platform --comment "Completado: {descripción}"
   gh issue reopen {N} --repo pablo-beiqa/beiqa-real-estate-platform
   # Comentar
   gh issue comment {N} --repo pablo-beiqa/beiqa-real-estate-platform --body "Progreso sync {FECHA}: {avance}"
   # Crear nuevo
   gh issue create --repo pablo-beiqa/beiqa-real-estate-platform --title "{título}" --body "{body}" --label "{labels}" --milestone "{milestone}" --assignee "{user}"
   # Editar (título, body, labels, milestone, assignees)
   gh issue edit {N} --repo pablo-beiqa/beiqa-real-estate-platform --title "{nuevo}" --add-label "{label}" --remove-label "{label}" --milestone "{milestone}" --add-assignee "{user}"
   # Mover de sprint en Project Board
   gh project item-edit --project-id {PROJECT_ID} --id {ITEM_ID} --field-id {SPRINT_FIELD_ID} --iteration-id {ITERATION_ID}
   ```
8. **Propagación** — seguir `tasks/propagation-rules.md` (**NUNCA tocar CLAUDE.md**):
   - Roadmap cambió → Executive-Summary, README, 01-Modules/README
   - Stack-Decidido cambió → README, Executive-Summary
   - ADRs/README cambió → README, Executive-Summary
   - Estado módulo cambió → 01-Modules/README, README
   - Schema-Real cambió → README, Executive-Summary, Roadmap
9. **Commit**:
   ```bash
   git commit -m "$(cat <<'EOF'
   docs(sync): sincronizar estado — {resumen de lo actualizado}

   Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>
   EOF
   )"
   ```

---

### FASE 7 — Resumen final

**Tabla de cambios** (output principal):

```
| Archivo | Cambio | Razón |
|---------|--------|-------|
| Sprint-02.md | Deliverable X: Pendiente → En progreso | Confirmado por usuario |
| 01-Modules/README.md | Scraper: 🔴 → 🟢 | Primer scraper funcional |
| ADRs/ADR-024.md | Creado | Decisión sobre {tema} |
| ... | ... | ... |
```

**Resumen estructurado**:

```
=== SYNC COMPLETADO ===
Fecha: YYYY-MM-DD
Usuario: {Pablo/Fabrizio}
Sprint activo: S{N}

Archivos actualizados: {N}
Issues: cerrados {N} | modificados {N} | creados {N} | comentados {N}
ADRs creados: {N}
Lecciones agregadas: {N}
Propagación: completa | {N} archivos pendientes

Próxima sync sugerida: {fecha o condición}
===========================
```

**Sugerencia de próxima sync**: basada en el progreso del sprint y trabajo pendiente (ej: "en 2-3 días después de que Fabrizio termine migrations" o "después del siguiente bloque de trabajo en beiqa-agents").

---

## Reglas de ejecución

1. **Entrevista PRIMERO, repos DESPUÉS** — nunca mostrar datos de repos antes de que el usuario describa su trabajo. Evitar sesgar las respuestas.
2. **Juicio para profundidad** — no hay bloques fijos. Un bug fix simple necesita 1 follow-up; un cambio de schema necesita 3-4. Adaptar al contenido.
3. **Adaptar al usuario** — Pablo y Fabrizio tienen dominios diferentes. No preguntar fuera del dominio de cada persona.
4. **Palabra del usuario > repos** — los repos pueden no estar pusheados. Si el usuario dice que trabajó en algo pero git log no lo muestra, confiar y notar "(no pusheado aún)".
5. **Aprobación batch** — presentar TODAS las propuestas agrupadas en FASE 5. No pedir aprobación paso a paso (diferencia vs sprint-retro).
6. **ADRs completos** — si se crea un ADR, debe ser COMPLETO con todas las secciones del formato MADR. No crear stubs ni placeholders.
7. **Lecciones obligatorias** — SIEMPRE preguntar por lecciones aprendidas como última pregunta de la entrevista.
8. **Propagación obligatoria** — verificar `tasks/propagation-rules.md` para cada archivo modificado. No actualizar un archivo de forma aislada.
9. **No inventar progreso** — solo actualizar lo que el usuario confirma o los repos demuestran. Nunca asumir que algo se completó.
10. **Un commit al final** — agrupar todos los cambios de sync en un solo commit: `docs(sync): sincronizar estado — {resumen}`.
11. **Labels verificados** — usar solo labels que existen: `documentation`, `enhancement`, `bug`, `data`, `scraper`, `testing`, `golden-record`, `ai-brain`, `mastra`, `triggerdev`, `devops`, `frontend`, `geospatial`, `P0`, `P1`, `P2`. **NO usar `docs`**.
12. **Resumen como tabla** — el output final es una tabla de cambios (Archivo | Cambio | Razón), no bullet points.
13. **Profundidad sin límite** — la entrevista va tan profundo como sea necesario para capturar todo el trabajo realizado. Si el usuario empieza a planificar un sprint nuevo completo (OKRs, deliverables futuros), sugerir `/sprint-planning`, pero no cortar la conversación prematuramente.
14. **Repos locales primero** — intentar acceso local a directorios hermanos antes de usar GitHub API. Siempre usar comillas en paths con espacios.
15. **NUNCA modificar CLAUDE.md** — este archivo se mantiene manualmente. Sync-repo puede actualizar Executive-Summary, README, Roadmap, módulos, Stack-Decidido, ADRs, pero NUNCA CLAUDE.md.
16. **SIEMPRE AskUserQuestion en la entrevista** — ninguna pregunta al usuario se hace como texto libre. Toda pregunta usa AskUserQuestion con opciones relevantes al contexto. Máximo 4 preguntas por call.
