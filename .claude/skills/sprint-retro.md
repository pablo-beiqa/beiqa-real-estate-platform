# sprint-retro

Ejecuta el cierre completo de un sprint: recolecta datos reales, entrevista al usuario sobre lo completado y aprendizajes, actualiza documentación, mueve deliverables incompletos, y documenta la retrospectiva.

## Cómo invocar

`/sprint-retro`

---

## Contexto

Este skill se ejecuta al **final de un sprint** (o al inicio del siguiente, antes del planning). Combina datos objetivos (issues cerrados, commits) con la perspectiva subjetiva de Pablo para generar una retrospectiva útil.

---

## Proceso completo

### FASE 1 — Recolectar datos (automático)

Ejecutar en paralelo:

1. Leer sprint actual: `03-Roadmap/Q{X}-2026/Sprint-{N}.md`
2. Ejecutar `gh issue list --repo pablo-beiqa/beiqa-real-estate-platform --limit 200 --state all` — ver qué issues se cerraron durante el sprint
3. Para cada deliverable del sprint, verificar: ¿se completó? (basándose en issues cerrados y estado documentado)
4. Ejecutar `git log --after={fecha_inicio_sprint} --before={fecha_fin_sprint} --oneline` para ver commits del periodo
5. Leer `tasks/lessons.md` — lecciones existentes para no duplicar

### FASE 2 — Entrevista de cierre

Preguntar a Pablo usando AskUserQuestion:

**Bloque 1 — Completado vs planeado:**
- ¿Qué se completó de lo planeado?
- ¿Qué NO se completó y por qué?

**Bloque 2 — Retrospectiva:**
- ¿Qué salió bien? (proceso, comunicación, herramientas)
- ¿Qué salió mal o puede mejorar?
- ¿Hubo sorpresas o aprendizajes inesperados?

**Bloque 3 — Lecciones:**
- ¿Hay algo que agregar a `tasks/lessons.md`?
- Proponer lecciones basadas en lo que Pablo describió

**Bloque 4 — Carryover:**
- Para cada deliverable incompleto: ¿se mueve al siguiente sprint o se descarta?
- ¿Cambió la prioridad de algo?

### FASE 3 — Actualizar documentación

1. **Llenar sección "Review" del Sprint-{N}.md**:
   - Métricas de cierre (issues cerrados, KRs completados, deliverables entregados)
   - Retrospectiva (Bien / Mejorar / Acción)
   - Cambiar estado del sprint de "Activo" a "Cerrado"

2. **Mover deliverables incompletos**:
   - Agregar a Sprint-{N+1}.md como carryover (si existe)
   - O documentar en Backlog.md con sprint actualizado

3. **Actualizar Backlog.md**:
   - Issues completados: marcar como cerrados
   - Issues movidos: actualizar sprint asignado

4. **Cerrar/comentar issues en GitHub**:
   - `gh issue close <N> --repo pablo-beiqa/beiqa-real-estate-platform --comment "Completado en Sprint {N}"`
   - `gh issue comment <N> --repo pablo-beiqa/beiqa-real-estate-platform --body "Movido a Sprint {N+1}: {razón}"`

5. **Agregar lecciones a `tasks/lessons.md`** (si Pablo aprobó)

6. **Verificar propagación**:
   - Q{X}-2026/README.md (OKRs status)
   - Roadmap.md (si cambió estado del sprint)

### FASE 4 — Commit

`docs(roadmap): cerrar Sprint {N} — retrospectiva y métricas de cierre`

Presentar resumen final:
```
=== CIERRE SPRINT {N} ===
Periodo: {fechas}
KRs completados: X/Y
Issues cerrados: N
Deliverables completados: X/Y
Carryover a Sprint {N+1}: N items
Lecciones documentadas: N
========================
```

---

## Reglas de ejecución

1. **Datos objetivos primero** — recolectar issues cerrados y commits ANTES de entrevistar
2. **No juzgar** — si algo no se completó, documentar la razón sin criticar
3. **Pedir aprobación** para cerrar issues en GitHub y para agregar lecciones
4. **Carryover explícito** — cada deliverable incompleto debe tener destino: siguiente sprint, backlog, o descartado
5. **Métricas honestas** — no inflar KRs completados. Si un KR se completó parcialmente, documentar el porcentaje
6. **Propagación obligatoria** — verificar Q{X} README y Roadmap.md
