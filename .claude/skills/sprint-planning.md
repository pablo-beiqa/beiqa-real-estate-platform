# sprint-planning

Facilita el proceso completo de planificación de un sprint de Beiqa. Absorbe todo el contexto disponible del repo (silenciosamente), entrevista a Pablo con profundidad usando AskUserQuestion, genera el sprint file completo como preview, y ejecuta todos los cambios tras una sola aprobación: reescribir el sprint file, crear/mover issues en GitHub + Project Board, actualizar Backlog.md, propagación y commit.

## Cómo invocar

`/sprint-planning`

---

## Contexto

Este skill implementa el proceso validado en la sesión del 2026-03-11, documentado en `memory/skills-context.md`. La entrevista usa AskUserQuestion (nunca texto libre) y es **adaptativa**: ~8 rondas para sprints simples, hasta 14-16 para sprints complejos. Pablo dicta; el skill organiza.

---

## FASE 0 — Contexto automático (silenciosa)

Ejecutar antes de hacer cualquier pregunta. No reportar progreso al usuario durante esta fase.

### 0.1 — Detectar sprint siguiente

1. Usar Glob para listar archivos `03-Roadmap/Q*-2026/Sprint-*.md`
2. Identificar el número más alto (N) → el siguiente sprint es N+1
3. Identificar el Quarter (Q{X}) del sprint siguiente leyendo `03-Roadmap/Roadmap.md`

### 0.2 — Leer estado actual

1. Leer `03-Roadmap/Roadmap.md` completo — milestones activos, estructura de sprints, fechas
2. Leer `03-Roadmap/Q{anterior}-2026/Sprint-{N}.md` (sprint anterior completo)
3. **Detección de retro**: Si `## Review` del sprint anterior contiene solo el placeholder "Completar al cierre..." → flag `retro_pendiente = true`. Si está llena → `retro_pendiente = false`.
4. Leer `03-Roadmap/Q{X}-2026/Sprint-{N+1}.md` si existe (puede ser un placeholder — no importa, se sobreescribirá)
5. Leer `03-Roadmap/Backlog.md` completo — todos los issues y sus asignaciones de sprint actuales
6. Leer `tasks/lessons.md` — no repetir errores pasados
7. Leer `02-Architecture/ADRs/README.md` — verificar si hay ADRs propuestos que bloqueen deliverables

### 0.3 — Consultar GitHub

Ejecutar en paralelo:
```bash
gh issue list --repo pablo-beiqa/beiqa-real-estate-platform --limit 200 --state open
gh project list --owner pablo-beiqa
```

Con el número de proyecto obtenido:
```bash
gh project field-list {PROJECT_NUMBER} --owner pablo-beiqa --format json
```

Guardar internamente: project number, Sprint field ID, iteration IDs disponibles (para ejecutar `gh project item-edit` en Fase 5).

### 0.4 — Calcular capacidad base

- Pablo: ~10 días hábiles (2 semanas)
- Fabrizio: ~10 días hábiles (2 semanas)
- Total disponible: ~4 person-weeks
- Pamela: diseño paralelo — no computa como capacidad de desarrollo, no bloquea
- Jerónimo: ops — no computa como capacidad de desarrollo

---

## FASE 1 — Snapshot (1 ronda con AskUserQuestion)

### Si `retro_pendiente = true` — incluir mini-retro

Lanzar AskUserQuestion con estas preguntas:
1. ¿Qué se completó del sprint anterior? ¿Qué quedó pendiente y por qué?
2. ¿Hay cambios de contexto de negocio que afecten prioridades? (demos, urgencias, cambios con clientes)
3. En tus propias palabras, ¿de qué va a tratar el próximo sprint?

### Si `retro_pendiente = false` — solo visión

Lanzar AskUserQuestion con:
1. ¿Hay cambios de contexto de negocio relevantes para este sprint?
2. En tus propias palabras, ¿de qué va a tratar el próximo sprint?

**Escuchar atentamente la respuesta**: identificar los tracks/áreas que Pablo menciona. Estos son los únicos tracks del sprint. No agregar tracks que Pablo no mencionó.

---

## FASE 2 — Deep Dive (adaptativo: ~5-10 rondas)

### Calibración de complejidad

Antes de comenzar, estimar la complejidad basada en los tracks mencionados en Fase 1:

| Complejidad | Tracks | Deliverables esperados | Rondas Fase 2 | Total sprint |
|-------------|--------|----------------------|---------------|-------------|
| Simple | 2-3 | ≤8 | ~4-5 | ~8 rondas |
| Media | 3-4 | 8-12 | ~6-8 | ~10-12 rondas |
| Compleja | 4-5 | >12 | ~9-11 | ~14-16 rondas |

### 2a — Listado de deliverables por track

Para **cada track que Pablo mencionó** en Fase 1, usar AskUserQuestion para preguntar:
- ¿Qué deliverables concretos incluye este track en el sprint?
- ¿Quién los ejecuta? (Pablo / Fabrizio / Pamela / Jerónimo)
- ¿Hay dependencias o bloqueos entre ellos, o con otros tracks?

**Batching**: Hasta 4 tracks por ronda (límite de AskUserQuestion). Si hay 4 tracks, preguntar los 4 en paralelo en una sola ronda.

**Regla crítica**: No preguntar sobre tracks que Pablo NO mencionó en Fase 1. Si no habló de frontend → no preguntar sobre frontend. Respetar sus exclusiones activas.

### 2b — Acceptance Criteria por deliverable

Una vez conocida la lista completa de deliverables, preguntar ACs específicos para cada uno.

Para cada deliverable, preguntar:
- ¿Cuál es el resultado esperado concreto? (ej: "schema con ≥5 tablas + migrations aplicadas + primeros datos ingested de ≥1 portal")
- ¿Qué tiene que ser verdad para que este deliverable esté "listo"? (Acceptance Criteria)

**Batching**: Agrupar 3-4 deliverables por ronda usando AskUserQuestion. Si un deliverable es ambiguo, de alto riesgo o crítico → dedicarle su propia pregunta.

**Criterio de detención**: Cuando todos los deliverables tienen ACs claros y específicos, cerrar la sub-fase y pasar a Fase 3.

---

## FASE 3 — Síntesis (1-2 rondas con AskUserQuestion)

### 3.1 — Issues huérfanos

Comparar los issues marcados para Sprint {N+1} en Backlog.md vs los deliverables que Pablo mencionó en la entrevista. Identificar "huérfanos" (asignados al sprint pero no mencionados).

Si hay huérfanos, preguntar en bloque (una sola AskUserQuestion):
> "Estos issues estaban asignados a Sprint {N+1} pero no los mencionaste: [lista con #N: título]. ¿Qué hacemos con cada uno? (se mantienen / se mueven a Sprint {N+2} / se eliminan del backlog)"

### 3.2 — Conflictos de capacidad

Calcular la carga por persona sumando estimados rough por deliverable:
- Deliverable pequeño (copy/paste, config): ~0.5-1d
- Deliverable mediano (feature nueva, schema): ~1-3d
- Deliverable grande (arquitectura, pipeline completo): ~3-5d

Si la suma supera la capacidad base (10d por persona):
- Mostrar exactamente qué no cabe: "Pablo tiene ~10d pero los deliverables suman ~{N}d: [lista con estimado]"
- Preguntar: "¿Qué cede? ¿Cuál es la prioridad?"
- Proponer activamente qué cortar con justificación de negocio (no solo alertar)

### 3.3 — Milestones

Leer milestones activos de Roadmap.md. Construir tabla de mapeo: cada deliverable → milestone que avanza.

Si algún deliverable no mapea a ningún milestone existente:
1. Proponer nuevo milestone (si tiene peso suficiente para justificarlo)
2. O asignarlo al milestone existente más cercano en tiempo/tema

Presentar tabla y preguntar: "¿Algún ajuste en el mapeo de deliverables a milestones?"

### 3.4 — Proponer OKRs

Basado en todo lo discutido, proponer 2-3 OKRs. Máximo 3-4 KRs por OKR. Formato:

```
O1: [Nombre del objetivo]
  KR1: [Key Result medible con criterio de éxito específico]
  KR2: [Key Result medible]
  Owner: [persona]
```

Preguntar: "¿Apruebas estos OKRs o quieres ajustar alguno?"
Iterar hasta aprobación. No pasar a siguiente paso sin OKRs aprobados.

### 3.5 — Actividades continuas

Preguntar:
> "¿Hay actividades de fondo este sprint? (learning, educación técnica, actualizaciones de repo, etc.) ¿Son deliverables formales o trabajo de background que se evalúa al cierre?"

---

## FASE 4 — Preview + Aprobación (1 ronda)

Generar el sprint file completo en markdown y renderizarlo directamente en la conversación. Seguir exactamente el formato de `03-Roadmap/Q1-2026/Sprint-02.md`.

### Estructura del preview

```markdown
# Sprint {N+1} ({fecha inicio}-{fecha fin}) — {Tema}

> **Estado**: Planificado
> **Quarter**: Q{X}-2026
> **Tema**: {descripción corta del tema central}
> **Milestones**: {lista de milestones activos}

---

## OKRs

| Objetivo | Key Result | Owner |
|----------|------------|-------|
| **O1: {nombre}** | KR1: {descripción} | {owner} |
| O1 | KR2: {descripción} | {owner} |
| **O2: {nombre}** | KR3: {descripción} | {owner} |
...

### Actividades continuas (se evalúan al cierre, no son deliverables formales)

- **{actividad}** — {personas} — {descripción breve}

---

## Definition of Done

(Auto-generado a partir de los ACs de cada deliverable — un checkbox por AC crítico)

- [ ] {AC del deliverable 1}
- [ ] {AC del deliverable 2}
- [ ] ...
- [ ] Todo commiteado y pusheado

---

## Deliverables por Track

### Track: {Nombre} ({Owner})

| Deliverable | Issue | AC | Estado |
|-------------|-------|----|--------|
| {nombre} | [#{N}](url) o NUEVO | {AC específico de la entrevista} | Pendiente |
...

(repetir por cada track)

---

## Dependencias y Orden (Critical Path)

```
Semana 1 ({fechas}):
  Pablo: {tarea} (~{N}d) → {tarea} (~{N}d)
  Fabrizio: {tarea} (~{N}d) → {tarea} (~{N}d)

Semana 2 ({fechas}):
  Pablo: {tarea} → {tarea}
  Fabrizio: {tarea} → {tarea}
```

**Dependencia crítica**: {si existe, describir la dependencia más importante y su fecha límite}

---

## Riesgos y Dependencias

| Riesgo / Dependencia | Impacto | Mitigación |
|---------------------|---------|------------|
| {riesgo} | Alto/Medio/Bajo | {mitigación concreta} |

---

## Issues movidos fuera de Sprint {N+1}

| Issue | Titulo | Destino | Razón |
|-------|--------|---------|-------|
| [#{N}](url) | {título} | S{N+2} / Backlog | {razón} |

---

## Issues nuevos a crear

| Titulo | Labels | Sprint | Owner | Descripción |
|--------|--------|--------|-------|-------------|
| {título} | `{label}` | S{N+1} | {owner} | {descripción} |

---

## Review ({fecha cierre})

> Completar al cierre del sprint.

### Métricas de Cierre

| Métrica | Target | Resultado |
|---------|--------|-----------|
| Issues cerrados | {N} | ... |
| KRs completados | {N}/{N} | ... |
{métricas específicas del sprint}

### Retrospectiva

- **Bien**: ...
- **Mejorar**: ...
- **Acción**: ...
```

### Solicitar aprobación

Después de renderizar el markdown completo:
> "¿Procedo con la ejecución? Si quieres ajustar algo, dime qué cambiar y regenero el draft."

Si Pablo hace correcciones → ajustar el draft, mostrarlo completo nuevamente, y volver a preguntar.
**No ejecutar nada** hasta recibir aprobación explícita.

---

## FASE 5 — Ejecución (automática tras aprobación)

Ejecutar en este orden exacto:

### 5.1 — Reescribir Sprint-{N+1}.md

Si el archivo ya existe (placeholder): sobreescribir completamente.
Si no existe: crear en `03-Roadmap/Q{X}-2026/Sprint-{N+1}.md`.
Contenido = exactamente el markdown aprobado en Fase 4.

### 5.2 — Crear issues nuevos en GitHub

Para cada issue de la tabla "Issues nuevos a crear":

```bash
gh issue create \
  --repo pablo-beiqa/beiqa-real-estate-platform \
  --title "{título}" \
  --body "{descripción}" \
  --label "{label1},{label2}"
```

Guardar el número de issue creado (#N) para actualizar la tabla de deliverables en Sprint-{N+1}.md (reemplazar "NUEVO" por "[#{N}](url)").

Labels válidos: `scraper`, `data`, `golden-record`, `ai-brain`, `mastra`, `triggerdev`, `devops`, `frontend`, `geospatial`, `documentation`, `P0`, `P1`, `P2`. **NO usar `docs`** (no existe).

### 5.3 — Mover issues de sprint

Para cada issue que cambia de sprint (huérfanos aceptados para mover, issues traídos de sprints anteriores):

**a) Actualizar campo Sprint en GitHub Project Board:**

```bash
# 1. Obtener el item ID del issue en el proyecto
gh project item-list {PROJECT_NUMBER} --owner pablo-beiqa --format json \
  | jq '.items[] | select(.content.number == {ISSUE_NUMBER}) | .id'

# 2. Actualizar el campo Sprint/Iteration
gh project item-edit \
  --project-id {PROJECT_ID} \
  --id {ITEM_ID} \
  --field-id {SPRINT_FIELD_ID} \
  --iteration-id {ITERATION_ID_DEL_SPRINT}
```

Nota: Los IDs de proyecto, field y iteration se obtuvieron en Fase 0 paso 0.3.

**b) Comentar en el issue (trazabilidad obligatoria):**

```bash
gh issue comment {N} \
  --repo pablo-beiqa/beiqa-real-estate-platform \
  --body "Movido de Sprint {X} a Sprint {Y} durante sprint planning {FECHA}: {razón breve}"
```

Siempre ejecutar ambos pasos juntos. Nunca uno sin el otro.

### 5.4 — Actualizar Backlog.md

Editar `03-Roadmap/Backlog.md`:
- Cambiar la columna de sprint de los issues movidos (ej: `S2` → `S3`, `S5+` → `S3`)
- Agregar filas para los issues nuevos creados en 5.2 (con su # y título)
- Actualizar issues movidos fuera del sprint (→ Backlog, S{N+2}, etc.)

### 5.5 — Actualizar Q{X}/README.md

Marcar Sprint {N+1} como activo en el README del quarter correspondiente.

**Si el sprint nuevo es el primero de un nuevo Quarter:**
- `Q{X-1}/README.md`: marcar Sprint {N} como cerrado/completado
- `Q{X}/README.md`: marcar Sprint {N+1} como activo y actualizar resumen

### 5.6 — Verificar propagación

Leer `tasks/propagation-rules.md`. Para cada archivo modificado, verificar sus dependientes:

| Si se modificó... | Verificar que se actualizó... |
|-------------------|-------------------------------|
| `Roadmap.md` | Executive-Summary, README.md, 01-Modules/README.md |
| `Backlog.md` | No tiene dependientes directos |
| Sprint file | Q{X}/README.md |

Aplicar correcciones menores autónomamente. Para cambios de significado/estructura: preguntar.

### 5.7 — Commit

```bash
git add 03-Roadmap/Q{X}-2026/Sprint-{N+1}.md
git add 03-Roadmap/Backlog.md
git add 03-Roadmap/Q{X}-2026/README.md
# + cualquier otro archivo modificado en 5.5 y 5.6

git commit -m "$(cat <<'EOF'
docs(roadmap): planificar Sprint {N+1} — {tema en 3-4 palabras}

Co-Authored-By: Claude Sonnet 4.6 <noreply@anthropic.com>
EOF
)"
```

### 5.8 — Resumen final

```
=== SPRINT {N+1} PLANIFICADO ===
Periodo: {fecha inicio} - {fecha fin}
Quarter: Q{X}-2026

OKRs ({N}):
  O1: {nombre}
  O2: {nombre}
  O3: {nombre (si aplica)}

Deliverables: {N} total
  Track AI/Mastra: {N} ({owner})
  Track Scraper: {N} ({owner})
  Track Data/Infra: {N} ({owner})
  Track Frontend: {N} ({owner})
  Track Design: {N} ({owner})

Issues creados: {N}
  [lista: #número — título]

Issues movidos: {N}
  [lista: #número Sprint X → Sprint Y]

Milestones cubiertos: {lista}
Commit: 1
================================
```

---

## Reglas de ejecución

1. **NUNCA proponer el plan antes de terminar la entrevista** — Pablo dicta, el skill organiza. No inventar deliverables.
2. **Usar SIEMPRE AskUserQuestion** para la entrevista. Nunca preguntas en texto libre. Máximo 4 preguntas por call.
3. **Milestones son no-negociables** — ningún deliverable puede quedar sin milestone asignado.
4. **Una sola aprobación antes de ejecutar** — después del preview completo en markdown. No pedir confirmación paso a paso.
5. **Mover issues = board + comentar** — siempre ambos pasos. Nunca uno sin el otro.
6. **gh project IDs**: descubrir dinámicamente en cada invocación (`gh project list`, `gh project field-list`). No hardcodear IDs.
7. **Cross-quarter**: Si Sprint {N+1} es el primero de un nuevo Q, actualizar ambos README de quarter (anterior + nuevo).
8. **No preguntar sobre tracks no mencionados** — si Pablo no habló de frontend, no preguntar sobre frontend. Respetar exclusiones.
9. **Labels**: verificar que son válidos antes de usar. NO usar `docs` (usar `documentation`). Labels conocidos: `scraper`, `data`, `golden-record`, `ai-brain`, `mastra`, `triggerdev`, `devops`, `frontend`, `geospatial`, `documentation`, `P0`, `P1`, `P2`.
10. **DoD auto-generado**: construir el checklist del DoD a partir de los ACs de cada deliverable. Pablo puede editarlo en el preview antes de aprobar.
