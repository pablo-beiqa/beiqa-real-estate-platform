---
name: "issue-creator"
description: "Schema y proceso para crear, editar, y mover issues en GitHub para el repo beiqa-real-estate-platform"
user-invocable: false
---

# issue-creator

Conocimiento de dominio para operaciones de GitHub Issues en `pablo-beiqa/beiqa-real-estate-platform`.

## Repo base

Siempre usar `--repo pablo-beiqa/beiqa-real-estate-platform` en todos los comandos.

## Crear issue nuevo

```bash
gh issue create \
  --repo pablo-beiqa/beiqa-real-estate-platform \
  --title "{título}" \
  --body "{body}" \
  --label "{label1},{label2}" \
  --milestone "{milestone}" \
  --assignee "{user}"
```

Guardar el número de issue retornado para referencia inmediata.

## Labels válidos

ÚNICAMENTE usar labels de esta lista. NO inventar labels nuevos:

| Label | Área |
|-------|------|
| `scraper` | Scrapers de portales |
| `data` | Pipeline de datos |
| `golden-record` | Golden record / normalización |
| `ai-brain` | AI agents / Mastra |
| `mastra` | Framework Mastra específicamente |
| `triggerdev` | Trigger.dev jobs / infra |
| `devops` | Infraestructura / deploy |
| `frontend` | Tenant Portal / Internal App |
| `geospatial` | H3, PostGIS, mapas |
| `documentation` | Docs del repo |
| `testing` | Tests y QA |
| `enhancement` | Mejora de feature existente |
| `bug` | Bug fix |
| `P0` | Prioridad crítica (bloqueante) |
| `P1` | Prioridad alta |
| `P2` | Prioridad media |

**NUNCA usar `docs`** — no existe. Usar `documentation`.

## Editar issue existente

```bash
# Cambiar título, body, milestone
gh issue edit {N} \
  --repo pablo-beiqa/beiqa-real-estate-platform \
  --title "{nuevo título}" \
  --body "{nuevo body}"

# Agregar/remover labels
gh issue edit {N} \
  --repo pablo-beiqa/beiqa-real-estate-platform \
  --add-label "{label}" \
  --remove-label "{label}"

# Cambiar milestone
gh issue edit {N} \
  --repo pablo-beiqa/beiqa-real-estate-platform \
  --milestone "{milestone}"

# Agregar assignee
gh issue edit {N} \
  --repo pablo-beiqa/beiqa-real-estate-platform \
  --add-assignee "{user}"
```

## Cerrar / Reabrir issue

```bash
# Cerrar con comentario (siempre incluir contexto)
gh issue close {N} \
  --repo pablo-beiqa/beiqa-real-estate-platform \
  --comment "Completado en {FECHA}: {descripción breve del resultado}"

# Reabrir
gh issue reopen {N} --repo pablo-beiqa/beiqa-real-estate-platform
```

## Comentar en issue

```bash
gh issue comment {N} \
  --repo pablo-beiqa/beiqa-real-estate-platform \
  --body "Progreso {FECHA}: {avance detallado}"
```

## Mover issue en Project Board (cambiar sprint)

```bash
# 1. Descubrir project number
gh project list --owner pablo-beiqa

# 2. Descubrir field IDs (Sprint field)
gh project field-list {PROJECT_NUMBER} --owner pablo-beiqa --format json

# 3. Obtener item ID del issue en el proyecto
gh project item-list {PROJECT_NUMBER} --owner pablo-beiqa --format json \
  | jq '.items[] | select(.content.number == {ISSUE_NUMBER}) | .id'

# 4. Cambiar sprint
gh project item-edit \
  --project-id {PROJECT_ID} \
  --id {ITEM_ID} \
  --field-id {SPRINT_FIELD_ID} \
  --iteration-id {ITERATION_ID}
```

Siempre descubrir IDs dinámicamente. NUNCA hardcodear project IDs, field IDs, ni iteration IDs.

## Regla crítica: mover issues

Al mover un issue de sprint, siempre ejecutar AMBOS pasos:
1. `gh project item-edit` — actualizar el board
2. `gh issue comment` — dejar trazabilidad con razón del movimiento

Nunca hacer uno sin el otro.

## Listar issues

```bash
# Issues abiertos (máx 200)
gh issue list --repo pablo-beiqa/beiqa-real-estate-platform --limit 200 --state open

# Issues por label
gh issue list --repo pablo-beiqa/beiqa-real-estate-platform --label "scraper" --state open

# Issues cerrados recientemente
gh issue list --repo pablo-beiqa/beiqa-real-estate-platform --limit 50 --state closed
```
