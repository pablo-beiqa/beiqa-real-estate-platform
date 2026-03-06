# Lecciones Aprendidas

> Este archivo es el **loop de auto-mejora** del equipo con Claude. Después de cada corrección,
> Claude agrega la lección aquí para que el error no se repita.
>
> **Regla**: Al inicio de cada sesión, Claude lee este archivo completo.
> **Formato**: Cada lección tiene fecha, contexto y la regla concreta.

---

## Cómo funciona

1. El usuario corrige a Claude (error, ajuste de tono, decisión incorrecta, etc.)
2. Claude propone una lección con fecha y categoría
3. El usuario aprueba o ajusta la lección
4. Claude la agrega aquí bajo la categoría correspondiente
5. En sesiones futuras, Claude consulta estas lecciones antes de actuar

---

## Lecciones

### Documentación

#### 2026-03-02 — El scoring dashboard es Tenant Portal, no Internal App
**Contexto**: El módulo Tenant Portal estaba documentado como "Fase 4+ Post-MVP" y sin owner. En realidad, el scoring dashboard implementado en beiqa-frontend ES el Tenant Portal (client-facing), no la Internal App (team-facing). La Internal App (gestión de operaciones, data, GIS) aún no existe como repo.
**Regla**: Antes de documentar un módulo, verificar en qué repo vive la implementación y a quién está orientado (equipo interno vs cliente). La distinción Internal App vs Tenant Portal es fundamentalmente sobre la **audiencia**: equipo Beiqa vs clientes corporativos.

### Arquitectura y stack

#### 2026-03-05 — Mastra reemplaza uso directo de Claude API para orquestación de agentes
**Contexto**: Trigger.dev tenía batch AI extraction acoplado al scraper. No había orquestación entre tareas AI, ni memory, ni evals. Se evaluó Mastra como framework dedicado de AI agents.
**Regla**: La lógica de AI (agentes, tools, workflows, memory) vive en Mastra (repo separado `beiqa-agents`). Trigger.dev solo hace ejecución durable (scrapers, cron, sync). La separación es por naturaleza de la tarea: determinístico = Trigger.dev, inteligente = Mastra. Ver [ADR-020](../02-Architecture/ADRs/ADR-020-Mastra.md) y [ADR-021](../02-Architecture/ADRs/ADR-021-Separacion-Trigger-Mastra.md).

#### 2026-03-05 — Trigger.dev scope reducido: solo ejecución durable, no AI
**Contexto**: ADR-003 se actualizó formalmente. Trigger.dev ya no hace batch AI extraction ni limpieza con LLMs — eso migra a Mastra.
**Regla**: No agregar lógica de AI a Trigger.dev. Si una tarea requiere un LLM, pertenece a Mastra. Trigger.dev notifica a Mastra vía HTTP POST cuando hay datos listos para procesar.

#### 2026-03-05 — Backboard.io deprecado: Mastra memory lo reemplaza
**Contexto**: ADR-014 pasó a "supersedido". Mastra incluye persistent memory, RAG y shared memory built-in, eliminando la necesidad de un servicio externo.
**Regla**: No proponer servicios externos de memoria AI. Mastra memory cubre persistent context, RAG y shared memory entre agentes.

#### 2026-02-27 — n8n deprecado: todo migrado a Trigger.dev
**Contexto**: n8n Cloud tenía workflows visuales pero no soportaba parsing HTML complejo ni lógica condicional por portal. Se migró todo a Trigger.dev (TypeScript, durable execution).
**Regla**: No proponer n8n para ningún flujo nuevo. Ver [ADR-019](../02-Architecture/ADRs/ADR-019-n8n-Deprecado.md).

#### 2026-03-05 — OpenRouter es "legacy": Mastra maneja AI routing
**Contexto**: OpenRouter ($15–30/mo) se usaba para GPT-4o-mini extraction en el scraping pipeline. Con Mastra, cada agente selecciona su modelo directamente.
**Regla**: OpenRouter sigue activo para extraction legacy, pero no proponer OpenRouter para nuevos agentes. Los costos se absorben en el presupuesto de Mastra LLM (~$67–88/mo).

### Proceso y workflow

#### 2026-03-05 — Planificación sprint-based reemplaza planificación por fases
**Contexto**: El roadmap original tenía "Fase 1, 2, 3" sin fechas concretas ni deliverables específicos. Se reescribió con sprints de 2 semanas, OKRs, acceptance criteria y links a GitHub Issues.
**Regla**: No usar "Fase X" para planificación operativa. Usar sprints de 2 semanas con OKRs y deliverables concretos. Solo Sprint actual y siguiente se detallan; el resto es backlog priorizado.

#### 2026-03-02 — Presupuesto: de estimaciones a costos reales verificados
**Contexto**: Total-Budget.md tenía $30–50/mes estimados. La realidad era $747–896/mes — subestimación de ~15x. Los costos reales se verificaron contra billing de cada servicio.
**Regla**: Nunca documentar costos estimados sin verificar contra billing real. Incluir siempre: costo por servicio, costo por portal, costo por propiedad, proyecciones y puntos de inflexión.

#### 2026-02-28 — Workflow Boris Cherny adoptado para gestión de tareas
**Contexto**: Antes no había proceso estructurado para tareas. Se implementó: plan en todo.md, ejecución paso a paso, verificación obligatoria, sync con GitHub Issues.
**Regla**: Tareas de 3+ pasos requieren plan escrito en `tasks/todo.md` antes de ejecutar. Verificación es obligatoria antes de marcar como completado.

#### 2026-03-05 — Archivos CONFLICT de herramientas de sync no son conflictos de git
**Contexto**: La herramienta de sincronización de archivos (carpeta `Sync`) generó archivos `*-CONFLICT-1` cuando hubo cambios concurrentes. Parecían conflictos de git pero NO lo eran — no contenían marcadores `<<<<<<<`/`=======`/`>>>>>>>`.
**Regla**: Los archivos que contengan "CONFLICT" en el nombre deben eliminarse — son artefactos de sync, no documentos válidos. Verificar periódicamente con `find . -name "*CONFLICT*"`.

### Comunicación y tono

_Aún no hay lecciones en esta categoría._

---

*Última actualización: 2026-03-05*
