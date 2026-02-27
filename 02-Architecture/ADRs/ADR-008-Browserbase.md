---
status: "aceptado"
date: 2026-02-25
decision-makers: "Fabrizio (Tech Lead)"
informed: "Pablo"
costo-mensual: "$20"
---

# Browserbase — Cloud Browser para Scraping Complejo

## Contexto y Problema

Algunos escenarios de scraping requieren interacción con el browser (clicks, scrolls, sesiones autenticadas) que Firecrawl no maneja. ¿Cómo resolvemos estos casos sin montar infraestructura de Playwright/Chromium propia?

## Decisión

**Browserbase** como servicio de cloud browser sessions. Complementa a Firecrawl para casos donde se necesita interacción real con el browser.

**Casos de uso actuales:**
- Colliers: descarga de archivos (imágenes + PDFs) que requieren cookies de sesión
- Pincali: extracción de teléfonos de broker (click en botón de contacto que revela el dato)

### Consecuencias

* Bien, porque sesiones de browser en la nube sin infraestructura propia
* Bien, porque complementa Firecrawl para casos edge
* Mal, porque $20/mo adicionales por un servicio que se usa poco
* Neutral, porque el uso es limitado a 2-3 escenarios específicos

## Plan de Implementación

* **Sistemas afectados**: `beiqa-scraper` (repo), Browserbase (dashboard)
* **Dependencias**: `browserbase@^2.6.0`, `playwright-core@^1.58.2`, `chromium-bidi@^15.0.0`, `BROWSERBASE_API_KEY`, `BROWSERBASE_PROJECT_ID`
* **Patrones a seguir**: Browserbase como **fallback** cuando Firecrawl no puede. Usar `shared/browserbase.ts` para session management.
* **Patrones a evitar**: No usar Browserbase para scraping simple (Firecrawl es más eficiente). No escalar sesiones sin monitorear costos.

### Verificación

- [x] Colliers `download-files.ts`: descarga directa + Browserbase fallback
- [x] Pincali `test-browserbase.ts`: extracción de teléfonos de broker via click
- [x] Shared session manager en `shared/browserbase.ts`

## Alternativas Consideradas

* **Playwright self-hosted**: Requiere infraestructura propia (Docker, servers). Overkill para 2-3 casos.
* **No usar browser automation**: Algunos datos (teléfonos de broker, archivos protegidos) no son accesibles sin interacción.

## Información Adicional

- Browserbase docs: `https://docs.browserbase.com`
- Relacionado con: [ADR-007](ADR-007-Firecrawl.md) (motor principal), [ADR-002](ADR-002-Estrategia-Scraping.md) (estrategia general)
