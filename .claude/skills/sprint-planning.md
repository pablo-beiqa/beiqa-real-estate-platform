# sprint-planning

Planifica un sprint completo para Beiqa: entrevista al usuario, define OKRs y deliverables, crea/mueve issues en GitHub, y actualiza toda la documentación del repositorio.

## Cómo invocar

`/sprint-planning`

---

## Contexto

Este skill implementa el proceso de planificación de sprint validado en sesiones anteriores. El sprint planning es un proceso **colaborativo** — Claude organiza y estructura, pero Pablo dicta la visión y prioridades.

---

## Proceso completo

### FASE 1 — Recolectar contexto (automático)

Leer estos archivos en paralelo antes de hacer cualquier pregunta:

1. `03-Roadmap/Roadmap.md` — milestones, estructura general
2. Sprint anterior completo: `03-Roadmap/Q{X}-2026/Sprint-{N-1}.md`
3. `03-Roadmap/Backlog.md` — todos los issues con sprint asignado
4. Ejecutar `gh issue list --repo pablo-beiqa/beiqa-real-estate-platform --limit 200 --state open` — estado real de issues
5. `tasks/lessons.md` — para no repetir errores
6. `02-Architecture/ADRs/README.md` — ADRs pendientes que puedan bloquear

Con esta información, tener claro:
- Qué sprint se está planeando (N) y sus fechas
- Qué quedó pendiente del sprint anterior
- Qué issues ya están asignados al sprint N
- Qué milestones están activos

### FASE 2 — Entrevista (preguntas a Pablo)

**REGLA CRÍTICA**: No proponer un plan completo sin antes entrevistar. Pablo dicta, Claude organiza.

Preguntar en bloques temáticos usando AskUserQuestion. Esperar respuesta de cada bloque antes de continuar.

**Bloque 1 — Estado actual:**
- ¿Cuál es el estado REAL del sprint que termina? ¿Qué se completó, qué quedó pendiente?
- ¿Hay cambios de contexto de negocio? (clientes, demos, urgencias)

**Bloque 2 — Visión del sprint:**
- "En tus propias palabras, ¿de qué va a tratar el próximo sprint?"
- Dejar que Pablo dicte libremente. NO interrumpir con sugerencias.

**Bloque 3 — Detalle por área** (para CADA área que Pablo mencionó):
- ¿Qué específicamente se va a hacer?
- ¿Quién lo hace? (Pablo, Fabrizio, Pamela)
- ¿Cuál es el resultado esperado? (schema vacío, datos ingested, prototipo funcional, etc.)
- ¿Hay dependencias o bloqueos?

**Bloque 4 — Tradeoffs de capacidad:**
- Mostrar conflictos de capacidad explícitamente
- "Fabrizio no puede hacer X + Y + Z en 2 semanas. ¿Cuál es prioridad?"
- Dejar que Pablo decida, no asumir

**Bloque 5 — Issues existentes:**
- Revisar issues asignados al sprint en Backlog.md
- Presentar issues "huérfanos" (asignados pero no mencionados por Pablo)
- Preguntar: ¿se mantienen, se mueven, se eliminan?

**Bloque 6 — OKRs:**
- Claude propone 2-3 OKRs basados en la entrevista
- Pablo aprueba, ajusta, o rechaza
- Iterar hasta tener OKRs aprobados

**Bloque 7 — Actividades continuas:**
- ¿Hay learning, educación, repo updates?
- ¿Son deliverables formales o background?

### FASE 3 — Ejecución

Una vez aprobados OKRs y deliverables:

1. **Reescribir `Sprint-{N}.md`** con formato estándar (ver template abajo)
2. **Crear issues nuevos** en GitHub con `gh issue create --repo pablo-beiqa/beiqa-real-estate-platform`
   - Labels disponibles: `documentation`, `enhancement`, `bug`, `data`, `scraper`, `testing`, `infra`, `triggerdev`, `frontend`, `ai-brain`, `geospatial`
   - NO usar label `docs` (no existe) — usar `documentation`
3. **Mover/actualizar issues existentes**: comentar cambios de sprint con `gh issue comment`
4. **Actualizar `Backlog.md`**: reflejar issues nuevos, movidos, y cambios de sprint
5. **Actualizar `ADRs/README.md`** si hay nuevos ADRs propuestos
6. **Verificar propagación** según `tasks/propagation-rules.md`:
   - Q{X}-2026/README.md (capacidad, descripción sprint)
   - Roadmap.md (si cambió resumen del sprint)
7. **Commit** con convención: `docs(roadmap): reestructurar Sprint {N} — {tema}`

### FASE 4 — Resumen

Presentar al usuario:
- OKRs aprobados con owners
- Deliverables por persona (tabla)
- Issues creados (con números) y movidos
- Dependencias críticas y riesgos
- Confirmar que todo fue committeado

---

## Template de Sprint-{N}.md

```markdown
# Sprint {N} ({fecha inicio}-{fecha fin}) — {Tema}

> **Estado**: Planificado
> **Quarter**: Q{X}-2026
> **Tema**: {descripción corta}
> **Milestones**: {milestones activos}

---

## OKRs

| Objetivo | Key Result | Owner |
|----------|------------|-------|
| **O1: {nombre}** | KR1: {descripción} | {owner} |
| O1 | KR2: {descripción} | {owner} |
| **O2: {nombre}** | KR3: {descripción} | {owner} |

### Actividades continuas (se evalúan al cierre, no son deliverables formales)

- **{actividad}** — {descripción}

---

## Definition of Done

- [ ] {criterio 1}
- [ ] {criterio 2}

---

## Deliverables por Track

### Track: {nombre} ({owner})

| Deliverable | Issue | AC | Estado |
|-------------|-------|----|--------|
| {nombre} | [#{N}](url) | {acceptance criteria} | Pendiente |

---

## Dependencias y Orden (Critical Path)

```
Semana 1 ({fechas}):
  {Owner}: {tarea} → {tarea}
Semana 2 ({fechas}):
  {Owner}: {tarea} → {tarea}
```

---

## Riesgos y Dependencias

| Riesgo / Dependencia | Impacto | Mitigación |
|---------------------|---------|------------|
| {riesgo} | Alto/Medio/Bajo | {mitigación} |

---

## Issues movidos fuera de Sprint {N}

| Issue | Titulo | Destino | Razón |
|-------|--------|---------|-------|

---

## Issues nuevos a crear

| Titulo | Labels | Sprint | Owner | Descripción |
|--------|--------|--------|-------|-------------|

---

## Review ({fecha cierre})

> Completar al cierre del sprint.

### Métricas de Cierre

| Métrica | Target | Resultado |
|---------|--------|-----------|

### Retrospectiva

- **Bien**: ...
- **Mejorar**: ...
- **Acción**: ...
```

---

## Reglas de ejecución

1. **NUNCA proponer plan sin entrevistar** — la entrevista es obligatoria. Pablo dicta, Claude organiza.
2. **NUNCA asumir prioridades** — si hay conflicto de capacidad, presentar el tradeoff y dejar que Pablo decida.
3. **Iterar OKRs** — proponer, recibir feedback, ajustar. No aceptar la primera versión sin validación.
4. **Labels de GitHub**: verificar que existen antes de usarlos. Labels conocidos: `documentation`, `enhancement`, `bug`, `data`, `scraper`, `testing`, `infra`, `triggerdev`, `frontend`, `ai-brain`, `geospatial`.
5. **Propagación obligatoria** — siempre verificar Q{X} README, Roadmap.md, Backlog.md después de cambios.
6. **Un commit al final** — no hacer commits intermedios durante el planning.
