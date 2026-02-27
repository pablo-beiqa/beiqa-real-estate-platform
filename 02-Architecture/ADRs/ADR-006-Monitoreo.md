---
status: "aceptado"
date: 2026-02-24
decision-makers: "Fabrizio (Tech Lead)"
informed: "Pablo, Jerónimo"
costo-mensual: "$0"
---

# Monitoreo — Slack + tabla error_logs

## Contexto y Problema

¿Cómo monitoreamos errores y estado de los pipelines de scraping y automatización? Con un equipo de 5 personas y ~60K propiedades, ¿necesitamos herramientas enterprise de monitoreo?

## Decisión

**Slack + tabla `error_logs` en Supabase**. Sin Sentry, sin Datadog, sin Grafana. El volumen y tamaño del equipo no justifican herramientas enterprise.

### Consecuencias

* Bien, porque costo $0
* Bien, porque Slack ya lo usa todo el equipo — las alertas llegan donde están
* Mal, porque no hay alerting automatizado sofisticado (thresholds, anomaly detection)

## Plan de Implementación

* **Sistemas afectados**: Supabase (tabla `error_logs`), Slack (canal de alertas), Trigger.dev (logging)
* **Patrones a seguir**: Errores → tabla `error_logs` en Supabase + notificación Slack
* **Patrones a evitar**: No instalar Sentry, Datadog ni Grafana por ahora

### Verificación

- [x] Tabla `error_logs` en Supabase
- [x] Notificaciones a Slack para errores críticos
- [x] Trigger.dev dashboard con logs de ejecuciones

## Alternativas Consideradas

* **Sentry**: Overkill para el volumen actual. Reconsiderar cuando haya frontend activo.
* **Datadog/Grafana**: Enterprise. No justificado para 5 personas y 4 usuarios concurrentes.

## Información Adicional

- Condición para revisitar: cuando la Internal App esté en producción y haya usuarios externos
- Relacionado con: [ADR-001](ADR-001-Supabase-Plataforma.md) (tabla error_logs vive en Supabase)
