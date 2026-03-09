# Protocolo de Cierre de Sesion

> Claude ejecuta este protocolo **ANTES** de cerrar cualquier sesion.
> La instruccion de ejecutarlo viene de MEMORY.md (auto-cargado).

---

## Paso 1: Resumen de sesion

- Listar que se hizo (cambios, decisiones, investigacion)
- Listar que NO se termino y por que
- Identificar bloqueantes descubiertos

## Paso 2: Commits y codigo

- [ ] Cambios committeados con convencion `tipo(scope): descripcion`
- [ ] No hay archivos sin trackear que deberian estar committeados
- [ ] Si se modifico codigo: verificar que funciona

## Paso 3: Propagacion cross-archivo

- [ ] Si se modifico un archivo clave (Roadmap, Budget, Stack, Agent-Architecture, estado de modulo): verificar dependientes segun `tasks/propagation-rules.md`
- [ ] Si cambio algo que afecta otro repo: documentar la dependencia

## Paso 4: Lecciones aprendidas

- [ ] Si el usuario corrigio un error → proponer leccion para `tasks/lessons.md`
- [ ] Si se descubrio un patron o decision nueva → documentarla

## Paso 5: GitHub Issues

- [ ] Issues trabajados: comentar progreso o cerrar si completados
- [ ] Tareas nuevas descubiertas: **PROPONER** al usuario (nunca crear sin aprobacion)
- [ ] Comando: `gh issue` con `--repo pablo-beiqa/beiqa-real-estate-platform`

## Paso 6: Presupuesto (condicional)

- [ ] Si se agrego/removio servicio del stack → actualizar Total-Budget.md
- [ ] Si cambio pricing de servicio existente → actualizar costos

## Paso 7: Actualizar MEMORY.md

Actualizar el MEMORY.md del proyecto con:
- Fecha de la sesion
- Resumen de lo que se hizo
- Estado actual (que queda pendiente)
- Decisiones tomadas
- Contexto para la proxima sesion

## Paso 8: Proximos pasos

- Proponer 2-3 tareas concretas para la proxima sesion
- Indicar prioridad y repo donde se ejecutarian
- Preguntar al usuario si esta de acuerdo

---

## Formato de resumen final

```
=== CIERRE DE SESION ===
Repo: [nombre]
Fecha: [fecha]
Commits: [N] nuevos
Issues: [cerrados/comentados/propuestos]
Lessons: [N] propuestas
MEMORY.md: [actualizado/sin cambios]
Proximos pasos: [lista]
========================
```
