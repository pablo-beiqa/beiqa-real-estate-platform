# sprint-retro

Ejecuta el cierre completo de un sprint: recolecta datos reales de forma silenciosa, entrevista a Pablo de forma adaptativa con AskUserQuestion hasta alcanzar 95/100 de confianza, cruza perspectiva subjetiva con datos objetivos, propone carryover y lecciones, actualiza toda la documentación, y hace handoff formal al sprint planning.

## Cómo invocar

`/sprint-retro`

---

## Contexto

Este skill se ejecuta al **final de un sprint** (o al inicio del siguiente, antes del planning). Combina datos objetivos (issues cerrados, commits) con la perspectiva subjetiva de Pablo para generar una retrospectiva útil y honesta. **El retro va siempre antes del planning** — garantiza que el agente de planning tiene acceso a lecciones frescas, carryover documentado, y el estado real del equipo.

---

## Proceso completo

### FASE 1 — Recolectar datos (automático, silencioso)

Ejecutar **en paralelo** antes de hacer cualquier pregunta a Pablo:

1. Leer `03-Roadmap/Roadmap.md` para identificar el sprint activo y sus fechas
2. Leer `03-Roadmap/Q{X}-2026/Sprint-{N}.md` — OKRs, deliverables planeados, fechas
3. Ejecutar `gh issue list --repo pablo-beiqa/beiqa-real-estate-platform --limit 200 --state all` — issues cerrados durante el sprint
4. Ejecutar `git log --after="{fecha_inicio_sprint}" --before="{fecha_fin_sprint}" --oneline` — actividad real del periodo
5. Leer `tasks/lessons.md` — para no duplicar lecciones existentes
6. Leer `01-Modules/README.md` — estado actual de módulos (para detectar cambios después)

**CRÍTICO**: NO mostrar estos datos a Pablo antes de la entrevista. Dejar que su perspectiva sea completamente libre y sin sesgo de métricas.

---

### FASE 2 — Entrevista adaptativa (hasta 95/100 de confianza)

**Instrucción base**: Leer el sprint file (ya cargado en FASE 1) y entrevistar a Pablo en detalle sobre **todo lo relevante**: implementación técnica, proceso, trade-offs, decisiones, sorpresas, aprendizajes. Usar **siempre** AskUserQuestion — nunca preguntas en texto libre. Cada pregunta debe tener 2-4 opciones + Other (que el tool provee automáticamente). Después de cada ronda, reportar confianza explícitamente. No pasar a FASE 3 hasta alcanzar **95/100**.

---

#### Dominios obligatorios (cubrir todos, en cualquier orden)

| # | Dominio | Preguntas clave |
|---|---------|-----------------|
| 1 | **Completado** | ¿Qué se completó? ¿Cómo se logró? ¿Qué facilitó el trabajo? |
| 2 | **No completado + causa raíz** | ¿Qué no se completó? ¿Por qué exactamente? |
| 3 | **Proceso y comunicación** | ¿Qué funcionó bien? ¿Qué falló? ¿Hubo fricción con Fabrizio/Pamela? |
| 4 | **Sorpresas y aprendizajes** | ¿Qué sorprendió? ¿Qué cambia la estrategia técnica? |
| 5 | **Evaluación de OKRs** | ¿Se cumplieron? ¿Eran realistas? ¿KRs necesitan reformularse? |
| 6 | **Intención de carryover** | Para cada incompleto: ¿siguiente sprint, backlog, o descartar? |

#### Dominios adaptativos (agregar si los datos de FASE 1 lo sugieren)

- Si hay >3 deliverables incompletos → profundizar en bloqueos técnicos específicos
- Si hay commits en semana 2 pero no en semana 1 → preguntar sobre arranque lento
- Si hay issues cerrados que no estaban en el sprint original → preguntar sobre scope creep
- Si `tasks/lessons.md` tiene lecciones repetidas del sprint anterior → preguntar si se aplicaron
- Si detecta cambios de arquitectura en commits → preguntar qué decisiones se tomaron y por qué

---

#### Formato de cada ronda

**Máximo 4 preguntas por llamada AskUserQuestion.** Batching: agrupar preguntas del mismo dominio. Si un dominio requiere más de 4 preguntas, dividirlo en sub-rondas.

Ejemplo de pregunta con opciones (para dominio 2):
```
question: "¿Cuál fue la causa principal de que [deliverable X] no se completara?"
header: "Causa raíz — [deliverable X]"
options: [
  { label: "Subestimación del esfuerzo", description: "Más complejo de lo estimado al planear" },
  { label: "Cambio de prioridad", description: "Surgió algo más urgente durante el sprint" },
  { label: "Bloqueo técnico", description: "Dependencia, error, o problema técnico sin resolver" },
  { label: "Falta de claridad", description: "El deliverable no estaba bien definido al inicio" }
]
// Other siempre disponible automáticamente
```

---

#### Reporte de confianza (después de CADA ronda, visible para Pablo)

```
**Confianza actual: X/100**
Tengo claro: [lista de dominios con suficiente detalle]
Necesito clarificar: [lista de puntos ambiguos o sin responder]
→ [Si <95%: "Lanzo ronda {N+1} sobre: [temas específicos]"]
→ [Si ≥95%: "Entendimiento suficiente. Paso al análisis cruzado."]
```

#### Criterio para alcanzar 95/100

- ✅ Todos los deliverables incompletos tienen causa raíz específica (no "falta de tiempo" genérico)
- ✅ Todos los OKRs evaluados con semáforo + justificación
- ✅ Al menos una lección potencial identificada
- ✅ Intención de carryover clara para cada incompleto
- ✅ Sin respuestas ambiguas sin profundizar ("no sé", "más o menos", "algo así")
- ✅ Dominios 1-6 cubiertos con respuestas concretas y accionables

---

### FASE 3 — Análisis cruzado (automático, interno)

Cruzar datos de FASE 1 con respuestas de FASE 2. No mostrar este proceso al usuario — usarlo internamente para preparar las propuestas de FASE 4.

- Verificar cada deliverable del sprint: ✅ Completo / ⚠️ Parcial / ❌ No iniciado
- Detectar discrepancias entre lo que Pablo dice y los issues realmente cerrados en GitHub
- Comparar deliverables completados con `01-Modules/README.md` para detectar módulos cuyo estado debería cambiar
- Proponer lecciones basadas en patrones (ej: "Fabrizio tuvo más carga de lo estimado en 2 sprints consecutivos", "Issues de infra suelen subestimarse")

---

### FASE 4 — Propuestas para aprobación (secuencial)

Presentar cada propuesta y esperar aprobación antes de ejecutar.

**4a. Carryover items** — Para cada deliverable incompleto, Claude propone destino:

- Opción A: Sprint {N+1} (alta prioridad, se retoma inmediatamente)
- Opción B: Backlog con sprint=TBD (importante pero no urgente)
- Opción C: Descartar (cerrar el issue, ya no aplica)

Presentar propuestas en tabla y pedir aprobación en bloque.

**Si Sprint-{N+1}.md no existe aún**: documentar carryover en `Backlog.md` con `sprint='S{N+1}'` y nota `[CARRYOVER de S{N}: {razón}]`. El skill de planning lo recogerá desde ahí.

**4b. Lecciones** — Claude propone lecciones concretas basadas en el análisis:

Proponer con el mismo formato de `tasks/lessons.md`. Pablo aprueba, rechaza, o modifica cada una antes de escribir. No agregar lecciones que ya existan.

**4c. Module status** — Si detectó cambios de estado:

- "Propongo actualizar módulo X de 'En desarrollo' a 'Funcional' dado que el deliverable Y se completó. ¿De acuerdo?"
- Esperar aprobación explícita antes de actualizar `01-Modules/README.md`

**4d. GitHub issues** — Preguntar issue por issue antes de cerrar:

- "¿Cerrar #133 — ADR-023 (completado en Sprint {N})?"
- Esperar respuesta. Pasar al siguiente solo después de confirmar.

---

### FASE 5 — Actualizar documentación

Ejecutar solo después de tener todas las aprobaciones de FASE 4.

1. **Sprint-{N}.md** — Llenar sección `## Review` completa (ver template abajo)
   - Cambiar `**Estado**: Activo` a `**Estado**: Cerrado`
2. **Backlog.md** — Marcar issues cerrados + agregar carryover items con sprint='S{N+1}'
3. **tasks/lessons.md** — Agregar lecciones aprobadas
4. **01-Modules/README.md** — Aplicar cambios de estado aprobados
5. **Propagación obligatoria**:
   - `03-Roadmap/Q{X}-2026/README.md` — actualizar estado del sprint a "Cerrado"
   - `03-Roadmap/Roadmap.md` — actualizar resumen del sprint si cambió

---

### FASE 6 — Commit + Handoff formal

**Commit**:
```
docs(roadmap): cerrar Sprint {N} — retrospectiva y métricas de cierre
```

**Resumen final**:
```
=== CIERRE SPRINT {N} ===
Periodo: {fechas}
OKRs: X🟢 Y🟡 Z🔴
Issues cerrados: N
Deliverables completados: X/Y
Carryover a Sprint {N+1}: N items
Lecciones documentadas: N
========================
```

**Handoff explícito** (siempre al final):
> Sprint {N} cerrado. Para planificar el siguiente, ejecuta `/sprint-planning`.
>
> Contexto clave para el planning:
> - Carryover: [lista de items con sus razones]
> - Lecciones frescas: [lecciones documentadas en este cierre]
> - Estado del equipo: [observaciones relevantes sobre capacidad/fricción]

---

## Template expandido — Sección Review en Sprint-{N}.md

```markdown
## Review ({fecha cierre})

### Métricas de Cierre

| Métrica | Target | Resultado |
|---------|--------|-----------|
| Issues cerrados | {N} planeados | {N} cerrados |
| Deliverables completos | {N} | {N} |
| OKRs completados | {N} | {N} |

### OKRs — Evaluación

| Objetivo | Key Result | Estado | Notas |
|----------|------------|--------|-------|
| **O1: {nombre}** | KR1: {descripción} | 🟢 | {completado} |
| O1 | KR2: {descripción} | 🟡 | Parcial: {razón} |
| **O2: {nombre}** | KR3: {descripción} | 🔴 | No iniciado: {razón} |

> 🟢 ≥80% completado / 🟡 40-79% / 🔴 <40% o no iniciado

### Deliverables — Estado Final

| Deliverable | Issue | Estado | Causa (si incompleto) |
|-------------|-------|--------|----------------------|
| {nombre} | #{N} | ✅ Completo | - |
| {nombre} | #{N} | ⚠️ Parcial | {causa raíz} |
| {nombre} | #{N} | ❌ No iniciado | {bloqueado por X / cambio de prioridad} |

### Retrospectiva

- **Bien**: {qué funcionó — proceso, herramientas, comunicación}
- **Mejorar**: {qué falló o puede mejorar}
- **Acción**: {qué se va a cambiar en el siguiente sprint}

### Carryover a Sprint {N+1}

| Issue | Deliverable | Razón del Carryover | Destino |
|-------|-------------|---------------------|---------|
| #{N} | {nombre} | {razón} | S{N+1} / Backlog |

### Lecciones Documentadas en este Cierre

- {lección 1}
- {lección 2}
```

---

## Reglas de ejecución

1. **Datos silenciosos primero** — recolectar todo antes de preguntar. NO mostrar métricas a Pablo antes de la entrevista para no sesgar respuestas.
2. **Entrevista adaptativa hasta 95/100** — usar AskUserQuestion con opciones en todas las preguntas. Reportar confianza después de cada ronda. Sin límite de rondas — continuar hasta alcanzar el umbral.
3. **Semáforo OKRs**: 🟢 ≥80% / 🟡 40-79% / 🔴 <40% o no iniciado.
4. **Carryover explícito** — cada incompleto tiene destino documentado. Nunca dejar incompletos "flotando".
5. **Lecciones proactivas** — Claude propone basado en patrones; Pablo aprueba antes de escribir.
6. **Issues GitHub** — preguntar uno por uno antes de cerrar.
7. **Propagación obligatoria** — Q{X} README y Roadmap.md siempre.
8. **Handoff formal** — el retro siempre termina con contexto concreto para el planning.
9. **No juzgar** — documentar causas sin crítica. Si algo no se completó, registrar la razón con neutralidad.
10. **Métricas honestas** — si un KR se completó parcialmente, es 🟡 o 🔴, no 🟢.
