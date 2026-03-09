# Scraper — Log de Ejecuciones

> ⚠️ **Nota**: La trazabilidad de ejecuciones vive en el **dashboard de Trigger.dev** (runs, logs, duración, errores). Este archivo es un complemento manual para registrar métricas agregadas o incidentes notables.
>
> FINSA envía notificaciones Slack post-scrape con conteos de nuevas/removidas/desactivadas. Los demás scrapers tendrán Slack cuando se implemente el módulo compartido (Issue #18).

---

## Template

```
## [YYYY-MM-DD] — [Portal] — [Resultado: OK / ERROR / PARCIAL]

- **Propiedades encontradas**: X
- **Nuevas (propiedades)**: X
- **Nuevas (brokers/inmobiliarias)**: X
- **Actualizadas**: X
- **Sin cambios**: X
- **No encontradas (baja)**: X
- **Errores**: X
- **Anomalias detectadas**: X
- **Imagenes descargadas**: X
- **Duracion**: X min
- **Herramienta**: Apify / TriggerDev
- **Notas**: ...
```

---

## Ejecuciones

<!-- Nuevas entradas se agregan al inicio -->
