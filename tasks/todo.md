# Tareas en Curso

> Este archivo es el **scratchpad de sesión** de Claude, sincronizado con **GitHub Issues**.
> Antes de ejecutar cualquier tarea de 3+ pasos, Claude escribe el plan aquí.
>
> **Estructura de cada tarea**: Plan → Ejecución → Verificación
> **Fuente de verdad del backlog**: [GitHub Issues en beiqa-real-estate-platform](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues)

---

## Integración con GitHub Issues

- **Todos los issues** del proyecto viven centralizados en `pablo-beiqa/beiqa-real-estate-platform`
- Labels diferencian el destino: `scraper`, `data`, `triggerdev`, `frontend`, `docs`
- Cada tarea aquí referencia su issue correspondiente (`#número`)
- Al completar, Claude comenta y/o cierra el issue vía `gh` CLI
- **Multi-repo**: Si la tarea se ejecuta en `beiqa-product` o `beiqa-frontend`, Claude usa `gh --repo pablo-beiqa/beiqa-real-estate-platform` para sincronizar

### Comandos útiles

```bash
# Ver backlog completo
gh issue list --repo pablo-beiqa/beiqa-real-estate-platform

# Ver issues por label
gh issue list --repo pablo-beiqa/beiqa-real-estate-platform --label scraper

# Comentar progreso en un issue
gh issue comment 33 --repo pablo-beiqa/beiqa-real-estate-platform --body "Progreso: ..."

# Cerrar issue al completar
gh issue close 33 --repo pablo-beiqa/beiqa-real-estate-platform --comment "Completado: ..."
```

---

## Checklist de cierre de sesión

> Claude ejecuta automáticamente el protocolo de cierre via el plugin `beiqa-session`
> (skill auto-trigger o comando `/cierre-sesion`). La instrucción viene de MEMORY.md.

---

## Tarea actual

_No hay tarea activa. Cuando Claude reciba una tarea de 3+ pasos, escribirá el plan aquí antes de ejecutar._

---

## Plantilla de tarea

> Copiar esta plantilla al iniciar una nueva tarea. No borrar la plantilla original.

```markdown
### [Nombre descriptivo de la tarea]

**Fecha**: YYYY-MM-DD
**Solicitado por**: [nombre]
**GitHub Issue**: #XX
**Repo destino**: platform | product | frontend
**Contexto**: [por qué se necesita esta tarea]

#### Plan (Parte 1)
- [ ] Paso 1: [descripción]
- [ ] Paso 2: [descripción]
- [ ] Paso 3: [descripción]

#### Ejecución (Parte 2)
_Resumen de alto nivel de cada paso completado:_

1. **Paso 1**: [qué se hizo y resultado]
2. **Paso 2**: [qué se hizo y resultado]

#### Verificación (Parte 3)
- [ ] El resultado cumple con lo solicitado
- [ ] No se introdujeron cambios fuera de scope
- [ ] Se verificó que funciona (no solo que "se ve bien")
- [ ] Se actualizó tasks/lessons.md si hubo correcciones
- [ ] Se comentó/cerró el GitHub Issue correspondiente

#### Revisión post-tarea
_[Reflexión breve: qué salió bien, qué se podría mejorar]_
```

---

## Historial de tareas completadas

### 2026-03-05 — Integración Mastra en documentación + limpieza CONFLICT + merge a main

**Solicitado por**: Pablo
**Repo destino**: platform
**Contexto**: Incorporar Mastra como capa transversal de AI agents en toda la documentación del proyecto, limpiar archivos CONFLICT generados por herramienta de sync, y unificar todo en main.

**Resumen**:
1. Limpieza de 10 archivos CONFLICT (3 workspace + 7 .git/) — artefactos de sync, no git
2. Comparativa main vs mastra-orchestration: 1 commit, 14 archivos, +1,124 líneas
3. Commit de 9 archivos adicionales (módulos, roadmap, CLAUDE.md)
4. Merge fast-forward mastra-orchestration → main, push exitoso
5. Creación de CHANGELOG.md con historial completo (19 entradas)
6. README.md actualizado: links corregidos, metadata, changelog resumido
7. 9 lecciones documentadas en lessons.md

**Verificación**: ✅ git status limpio, sin archivos CONFLICT, main actualizado en GitHub

---

*Última actualización: 2026-03-05*
