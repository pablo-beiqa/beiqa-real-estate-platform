---
status: "aceptado"
date: 2026-02-25
decision-makers: "Pablo (CEO)"
consulted: "Fabrizio (Tech Lead)"
informed: "Jerónimo, Pamela"
costo-mensual: "incluido en plan Rube"
---

# Rube — MCP Bridge como Interfaz Humana

## Contexto y Problema

BEIQA necesita que el equipo (Pablo, Jerónimo, Fabrizio) pueda interactuar con los sistemas (Supabase, HubSpot, Slack) sin esperar a que exista un frontend. La Internal App (Next.js) se difirió a Fase 2-3. ¿Cómo interactúa el equipo con la plataforma en Fase 1?

## Factores de Decisión

* Internal App no estará lista en Fase 1
* El equipo necesita acceder a datos y ejecutar acciones ahora
* Rube.app (by Composio) ya está en uso por el equipo
* Cero desarrollo requerido — funciona como MCP bridge inmediato

## Decisión

**Rube.app** como MCP bridge que conecta Claude Desktop con Supabase, HubSpot y Slack. El equipo habla con Claude, y Claude actúa sobre los sistemas vía MCP.

**Scope de Rube (ES):**
- Interfaz humana: equipo ↔ Claude ↔ sistemas
- Consultas ad-hoc a Supabase y HubSpot
- Acciones manuales sobre datos (actualizar, crear, buscar)

**Scope de Rube (NO ES):**
- No participa en pipelines automáticos (eso es Trigger.dev)
- No es la solución permanente — será reemplazado por frontend propio

### Consecuencias

* Bien, porque cero tiempo de desarrollo — funciona inmediatamente
* Bien, porque Claude entiende contexto y puede hacer operaciones complejas
* Mal, porque depende de Claude Desktop (no es una app web accesible para todos)
* Neutral, porque es transitorio — será reemplazado cuando exista el frontend

## Plan de Implementación

* **Sistemas afectados**: Claude Desktop (configuración MCP), Rube.app (dashboard)
* **Dependencias**: Cuenta Rube.app con MCPs configurados para Supabase, HubSpot, Slack
* **Patrones a seguir**: Usar Rube solo para interacción humana. Pipelines automáticos van por Trigger.dev.
* **Patrones a evitar**: No usar Rube para automatizaciones batch. No depender de Rube como solución permanente.

### Verificación

- [x] Rube conectado a Supabase vía MCP
- [x] Rube conectado a HubSpot vía MCP
- [x] Rube conectado a Slack vía MCP
- [x] Equipo usando Claude Desktop + Rube para consultas diarias

## Alternativas Consideradas

* **Retool/NocoDB**: Interface rápida sin código. Rechazado porque requiere setup y Rube ya funciona.
* **Desarrollar Internal App ahora**: Rechazado porque recursos limitados y Fase 1 se enfoca en datos.
* **Consultas directas a Supabase**: Posible pero no amigable para usuarios no técnicos (Jerónimo).

## Información Adicional

- Rube.app: [https://rube.app](https://rube.app) (by Composio)
- Será supersedido cuando se implemente [ADR-015](ADR-015-Frontend-Next-js.md) (Internal App con Next.js)
- Condición para revisitar: cuando la Internal App esté lista para producción
