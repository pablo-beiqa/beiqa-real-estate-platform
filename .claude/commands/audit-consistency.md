---
description: "Auditoría completa de consistencia del repo — detecta links rotos, status mismatches, ADR count drift, stack sync, y convenciones de emojis. Solo lee, nunca modifica."
argument-hint: "[links|sprints|status|stack]"
allowed-tools: ["Read", "Glob", "Grep"]
model: "haiku"
---

# audit-consistency

Ejecuta una auditoría completa de consistencia del repositorio de documentación de Beiqa. Detecta inconsistencias entre archivos — sprint conflicts, status mismatches, broken links, ADR count drift, stack sync, y convenciones de emojis. No modifica ningún archivo; solo reporta hallazgos.

## Cómo invocar

`/audit-consistency`

Opcionalmente con scope: `/audit-consistency links` | `/audit-consistency sprints` | `/audit-consistency status` | `/audit-consistency stack`

---

## Proceso de auditoría

Ejecutar los siguientes checks en orden. Para cada check, leer los archivos necesarios y comparar. Reportar hallazgos en la sección "Reporte" al final.

### CHECK 1 — Links rotos (Broken Links)

**Sources of truth**: paths reales en el filesystem.

Usar Grep para buscar links markdown `](` en archivos `.md` activos (excluir `archive/`). Para cada link relativo encontrado, verificar con Glob que el archivo destino existe.

Archivos con historial de links rotos (verificar primero):
- `01-Modules/Data/Normalization/README.md` → links a `02-Architecture/`
- `01-Modules/AI-Brain/Research/AI-Strategy.md` → links a `02-Architecture/`
- `01-Modules/Scraper/Research/Acquisition-Strategy.md` → links a `02-Architecture/`
- `00-Project/Vision-and-Goals.md` → links a `03-Roadmap/`
- `tasks/session-protocol.md` → referencia a `MEMORY.md`

Para cada link roto, reportar:
- Archivo que contiene el link roto
- Path incorrecto referenciado
- Path correcto sugerido (si existe en el filesystem)

### CHECK 2 — Archivos estándar por módulo

**Convención**: cada módulo en `01-Modules/` debe tener `README.md`, `Product-Questions.md`, `Requirements.md`.

Usar Glob para listar archivos en cada directorio de módulo. Módulos a verificar: `Scraper/`, `Data/`, `AI-Brain/`, `Geospatial/`, `Tenant-Portal/`, `Internal-App/`, `Market-Intelligence/`

Reportar módulos con archivos faltantes.

### CHECK 3 — Sprint del módulo vs Roadmap.md

**Source of truth**: `03-Roadmap/Roadmap.md`
**Derivados**: `01-Modules/README.md`, `CLAUDE.md`, cada módulo `README.md`

Leer Roadmap.md y extraer cuándo inicia cada módulo. Luego leer el campo `Sprint del proyecto` en el README de cada módulo y comparar. Verificar también las tablas en `01-Modules/README.md` y `CLAUDE.md`.

### CHECK 4 — Estado del módulo cross-file

**Source of truth por módulo**: `01-Modules/[Módulo]/README.md` campo `Estado`
**Derivados**: `01-Modules/README.md` tabla, `CLAUDE.md` tabla de módulos, `README.md` tabla de módulos

Leer el estado de cada módulo en su propio README. Luego leer las tres tablas derivadas. Reportar cualquier módulo donde el estado difiere.

Convención de emojis a verificar:
- 🟢 = "En desarrollo" o "Activo" o "En implementación"
- 🟡 = "En diseño" o "En pruebas" o "Draft"
- 🔴 = "Por iniciar"
- ✅ = "Completado" o "Producción"

Reportar cualquier emoji + texto que no sea coherente (ej: 🟢 con "En diseño").

### CHECK 5 — Conteo de ADRs

**Source of truth**: `02-Architecture/ADRs/README.md`
**Derivados**: `05-Communication/Executive-Summary.md`, `README.md`, `CLAUDE.md`

Contar ADRs en las tablas de ADRs/README.md (Aceptados, Propuestos, Deprecados). Buscar en derivados strings como "X ADRs" o "XX Architecture Decision Records". Reportar números que no coincidan.

Verificar también que no haya archivos ADR huérfanos (en el directorio pero no en el índice).

### CHECK 6 — Stack table sync

**Source of truth**: `02-Architecture/Stack-Decidido.md`
**Derivados**: `CLAUDE.md`, `README.md`, `05-Communication/Executive-Summary.md`

Tecnologías clave a verificar en TODOS los derivados:

| Tecnología | Verificar |
|-----------|-----------|
| Atlas.co | Debe aparecer en CLAUDE.md y Executive-Summary |
| Trigger.dev | NO debe describirse como "batch AI" — es "scrapers, cron, sync (sin AI)" per ADR-021 |
| Mastra | Debe aparecer como "En implementación" |

Buscar "batch AI" con Grep en archivos activos (excluir archive/, ADRs/, lessons.md). Reportar cualquier referencia activa.

### CHECK 7 — Datos numéricos críticos

Números a verificar con Grep:
1. Volumen I24: debe ser "~30K" o "~30,000". Buscar "60K", "60,000" — son incorrectos
2. Count de migrations: debe ser "14+" en todos los archivos
3. Presupuesto: debe ser "$747–$896 USD/mes" como rango base

Reportar archivos con números incorrectos.

### CHECK 8 — Stack descartado en contexto activo

**Source of truth**: `02-Architecture/Stack-Decidido.md` sección "Tecnologías Deprecadas"

Tecnologías que NO deben aparecer como activas:
- EasyBroker (descartado — ADR-002)
- n8n (deprecado — ADR-019)
- Backboard.io (supersedido — ADR-014)

Buscar con Grep en archivos activos (excluir archive/, ADRs/, CHANGELOG.md, lessons.md). Para cada mención, verificar si el contexto es histórico (correcto) o activo/futuro (incorrecto).

### CHECK 9 — Convenciones de emojis

Buscar con Grep patrones incoherentes:
- `🟢` seguido de "En diseño" o "Draft" (incorrecto — debería ser 🟡)
- `🟡` seguido de "Activo" o "Producción" (incorrecto)
- `🔴` seguido de "En desarrollo" o "Implementado" (incorrecto)

### CHECK 10 — Roles del equipo

**Source of truth**: `CLAUDE.md` tabla de equipo
**Derivados**: `README.md`, `05-Communication/Executive-Summary.md`

Verificar que Pamela aparece como "diseño" / "Figma", nunca como desarrolladora.

### CHECK 11 — Propagation rules coverage

Leer `tasks/propagation-rules.md`. Para cada archivo listado como source of truth, verificar que existe. Para cada archivo derivado listado, verificar que existe.

También verificar que `tasks/session-protocol.md` y `tasks/lessons.md` están referenciados en `CLAUDE.md` sección "Archivos de referencia clave".

---

## Formato del Reporte

Al finalizar todos los checks, producir el siguiente reporte:

```
=== AUDITORÍA DE CONSISTENCIA — [FECHA] ===

## RESUMEN
- Total inconsistencias: N
- Críticas (links rotos, datos incorrectos): N
- Medias (sprint/status conflicts): N
- Bajas (cosméticas, convenciones): N

## CRÍTICAS

### [CHECK N] — [Título]
- **Archivo**: path/al/archivo.md
- **Problema**: descripción concreta
- **Valor actual**: "texto exacto"
- **Valor correcto**: "texto correcto"
- **Source of truth**: path/al/source.md

## MEDIAS
[mismo formato]

## BAJAS
[mismo formato]

## SIN PROBLEMAS
Checks sin inconsistencias:
- CHECK N — Título

## SIGUIENTE PASO
[2-3 fixes más urgentes]
```

## Reglas de ejecución

1. **NO modificar ningún archivo** — solo leer y reportar
2. Distinguir entre "referencia histórica correcta" y "dato activo incorrecto"
3. Archivos en `archive/`, `ADRs/` (contexto histórico), `lessons.md`, y `CHANGELOG.md` se excluyen de checks de stack descartado
4. Cuando el scope es parcial, ejecutar solo checks relevantes:
   - `links` = CHECK 1, 2
   - `sprints` = CHECK 3, 4
   - `status` = CHECK 4, 5, 6
   - `stack` = CHECK 6, 7, 8
