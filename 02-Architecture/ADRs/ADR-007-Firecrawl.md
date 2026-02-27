---
status: "aceptado"
date: 2026-02-25
decision-makers: "Fabrizio (Tech Lead)"
consulted: "Pablo (CEO)"
informed: "Jerónimo"
costo-mensual: "$99"
---

# Firecrawl — Motor de Web Scraping

## Contexto y Problema

Para los 3 nuevos portales (Pincali, CBRE, Colliers), se necesita un motor de scraping que maneje JavaScript rendering, anti-bot measures, y extracción estructurada de datos. ¿Qué motor usamos para los scrapers custom en Trigger.dev?

## Factores de Decisión

* Los 3 portales usan JavaScript heavy rendering (Next.js en CBRE, EasyBroker en Pincali)
* Se necesita extracción LLM para datos no estructurados (precios, áreas, coordenadas)
* Anti-bot measures en Pincali requieren stealth proxy
* El equipo usa TypeScript — el motor debe tener API HTTP simple
* Budget: ~$100/mo para 100K páginas es razonable

## Opciones Consideradas

* Firecrawl API v2
* Playwright/Puppeteer self-managed
* Apify para todos los portales
* fetch() simple con cheerio

## Decisión

Opción elegida: "**Firecrawl API v2**", porque ofrece JS rendering, LLM extraction nativa, stealth proxy, y API HTTP simple. $99/mo para 100K páginas cubre el volumen de los 3 portales.

### Consecuencias

* Bien, porque API HTTP simple — una llamada para scrape + extract
* Bien, porque LLM extraction integrada (sin llamar a Claude/GPT por separado para parsing)
* Bien, porque stealth proxy incluido (necesario para Pincali)
* Bien, porque soporte de sitemap crawling y URL discovery
* Mal, porque dependencia de un servicio externo (si Firecrawl cae, los scrapers paran)
* Mal, porque $99/mo es un costo significativo vs herramientas gratuitas

## Plan de Implementación

* **Sistemas afectados**: `beiqa-scraper` (repo), Firecrawl API
* **Dependencias**: `FIRECRAWL_API_KEY` en environment de Trigger.dev
* **Patrones a seguir**: Usar `/scrape` para páginas individuales, `/map` para discovery de URLs, `/crawl` para sitios completos. Firecrawl devuelve `rawHtml` + `markdown` + JSON extraction.
* **Patrones a evitar**: No usar Firecrawl para I24 (usa Apify). No hacer scraping directo con fetch() en portales con JS rendering.
* **Configuración**: URL base `https://api.firecrawl.dev/v2`

### Verificación

- [x] Pincali: Firecrawl con proxy `stealth` (9 créditos/propiedad)
- [x] CBRE: Firecrawl sin LLM extraction (RSC parsing manual ahorra créditos)
- [x] Colliers: Firecrawl con LLM JSON extraction
- [x] Todos los scrapers usan Firecrawl como motor en `scraper-engine.ts`

## Información Adicional

- Firecrawl API docs: `https://docs.firecrawl.dev`
- Los 3 scrapers están implementados en `beiqa-scraper` repo
- CBRE usa RSC payload parsing en vez de LLM extraction (ahorro de créditos)
- Relacionado con: [ADR-002](ADR-002-Estrategia-Scraping.md), [ADR-008](ADR-008-Browserbase.md) (fallback para casos complejos)
