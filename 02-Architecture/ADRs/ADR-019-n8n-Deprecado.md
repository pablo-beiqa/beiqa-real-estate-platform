---
status: "deprecado — supersedido por ADR-003-Trigger-dev.md"
date: 2026-02-27
decision-makers: "Pablo (CEO), Fabrizio (Tech Lead)"
costo-mensual: "$0 (eliminado)"
---

# n8n Cloud — DEPRECADO

## Contexto y Problema

n8n Cloud fue originalmente seleccionado como plataforma de orquestación para flujos de integración (scraping, sync HubSpot, automatizaciones). Sin embargo, al evaluar las necesidades reales del proyecto, se determinó que Trigger.dev cubre todos los casos de uso con ventajas significativas.

## Decisión Original

**n8n Cloud** como plataforma de orquestación visual para flujos de integración entre servicios.

## Por qué se Deprecó

### Razones de la migración a Trigger.dev

1. **Todo es código TypeScript**: Los scrapers, la limpieza de datos, y las automatizaciones requieren lógica compleja que es más natural en código que en flujos visuales.

2. **Un solo sistema**: En lugar de dividir entre "flujos visuales" (n8n) y "código" (Trigger.dev), todo vive en Trigger.dev. Menos complejidad operacional.

3. **Mejor debugging**: TypeScript con tipos, logs estructurados, y debugging local es superior a debuggear nodos en una UI visual.

4. **Costo**: Eliminar n8n = un servicio menos que pagar y mantener.

5. **Consistencia del stack**: Todo el código de automatización en un solo lugar (repo beiqa-scraper).

### Qué se migró

| Flujo en n8n | Destino en Trigger.dev | Estado |
|-------------|----------------------|--------|
| Scraping orchestration | Trigger.dev scheduled tasks | ✅ Migrado |
| HubSpot sync | Trigger.dev task | 🟡 En migración |
| Data cleaning | Trigger.dev task | ✅ Migrado |
| Error notifications | Slack webhooks directos | ✅ Migrado |

### Qué NO se migró (no aplica)

- n8n no tenía flujos de frontend/UI
- n8n no manejaba autenticación ni storage

## Consecuencias

* Bien, porque se elimina un servicio del stack (menos costo, menos complejidad)
* Bien, porque todo el código de automatización vive en un solo repo (beiqa-scraper)
* Bien, porque TypeScript es más testeable y mantenible que flujos visuales
* Neutral, porque se pierde la interfaz visual de n8n (pero no era necesaria para el equipo técnico)

## Información Adicional

- Supersedido por: [ADR-003](ADR-003-Trigger-dev.md) (Trigger.dev como plataforma de automatización)
- Stack sin n8n: [Stack-Decidido.md](../Stack-Decidido.md) (a actualizar)
- Repo con todo el código migrado: `github.com/pablo-beiqa/beiqa-scraper`
