---
status: "aceptado"
date: 2026-02-25
decision-makers: "Fabrizio (Tech Lead), Pablo (CEO)"
consulted: "Alex (Advisor)"
informed: "Jerónimo"
costo-mensual: "incluido en Apify ($20) + Firecrawl ($99) + Browserbase ($20)"
---

# Estrategia de Scraping — 4 portales con herramientas diferenciadas

## Contexto y Problema

BEIQA necesita datos de propiedades comerciales/industriales de múltiples portales inmobiliarios en México. ¿Cuáles portales scrapeamos, con qué herramientas, y con qué frecuencia?

El mercado target de BEIQA (CDMX, EdoMex, Morelos, Puebla) está cubierto por 4 portales principales:
- **Inmuebles24**: portal multi-broker más grande de México (~30-40K props comerciales)
- **Pincali**: portal multi-broker (~30-40K props)
- **CBRE**: sitio corporativo de broker internacional (~cientos de props)
- **Colliers**: sitio corporativo de broker internacional (~cientos de props)

## Factores de Decisión

* Cobertura del mercado target — los 4 portales cubren la mayoría de propiedades relevantes
* Complejidad técnica variable — portales multi-broker vs sitios corporativos
* Volumen diferenciado — I24/Pincali (~30-40K) vs CBRE/Colliers (~cientos)
* Disponibilidad de herramientas pre-construidas — solo I24 tiene actor Apify
* Datos de broker diferenciados — multi-broker (solo empresa) vs corporativos (contactos individuales)

## Opciones Consideradas

* Apify para todos los portales
* Python/Scrapy custom para todos
* Apify (I24) + Trigger.dev/Firecrawl (nuevos portales)
* Solo Inmuebles24 (un portal)

## Decisión

Opción elegida: "**Apify (I24) + Trigger.dev/Firecrawl (Pincali, CBRE, Colliers)**", porque cada portal tiene características diferentes que requieren herramientas diferenciadas.

### Consecuencias

* Bien, porque Apify ya está validado en producción para I24
* Bien, porque Firecrawl maneja JS rendering y LLM extraction nativamente
* Bien, porque control total del código en Trigger.dev para scrapers custom
* Bien, porque frecuencia diferenciada por volumen (mensual vs semanal)
* Mal, porque dos herramientas de scraping diferentes (Apify + Firecrawl) aumentan complejidad
* Neutral, porque el costo combinado (~$139/mo) es razonable para el valor

## Plan de Implementación

* **Sistemas afectados**: `beiqa-scraper` (repo), Apify (dashboard), Trigger.dev (tasks), Supabase (tablas), HubSpot (sync)
* **Dependencias**: Apify actor para I24, Firecrawl API (`FIRECRAWL_API_KEY`), Browserbase SDK (`BROWSERBASE_API_KEY`)
* **Patrones a seguir**: Patrón orchestrator + processor de Trigger.dev (weekly-scrape → discover-urls → scrape-property → download-files)
* **Patrones a evitar**: No construir scrapers Python/Scrapy. No usar Apify para portales sin actor pre-construido.
* **Configuración**: `FIRECRAWL_API_KEY`, `BROWSERBASE_API_KEY`, `BROWSERBASE_PROJECT_ID` en `beiqa-scraper`

### Verificación

- [x] Inmuebles24: Apify en producción, ~25K propiedades en Supabase
- [x] Pincali: scraper implementado en `beiqa-scraper` (Trigger.dev + Firecrawl)
- [x] CBRE: scraper implementado en `beiqa-scraper` (Trigger.dev + Firecrawl, RSC parsing)
- [x] Colliers: scraper implementado en `beiqa-scraper` (Trigger.dev + Firecrawl + Browserbase)
- [ ] Pipeline completo: scraper → Supabase (persistencia via Trigger.dev task)
- [ ] Pipeline completo: scraper → HubSpot (sync de brokers)

## Pros y Contras por Opción

### Apify (I24) + Trigger.dev/Firecrawl (nuevos portales)

* Bien, porque aprovecha actor pre-construido para el portal más complejo (I24)
* Bien, porque Firecrawl ofrece LLM extraction, stealth proxy, y JS rendering
* Bien, porque Trigger.dev da control total sobre el código TypeScript
* Mal, porque dos herramientas de scraping diferentes

### Apify para todos los portales

* Bien, porque una sola herramienta
* Mal, porque no hay actores pre-construidos para Pincali, CBRE, Colliers
* Mal, porque costo escalaría significativamente

### Python/Scrapy custom

* Bien, porque control total
* Mal, porque el equipo usa TypeScript, no Python
* Mal, porque requiere infraestructura de proxy, anti-bot, etc.

### Solo Inmuebles24

* Bien, porque simplicidad máxima
* Mal, porque cobertura insuficiente del mercado

## Información Adicional

- Detalle de portales: [Scraper/Research/Acquisition-Strategy.md](../../01-Modules/Scraper/Research/Acquisition-Strategy.md)
- Repo del scraper: `github.com/pablo-beiqa/beiqa-scraper`
- EasyBroker fue evaluado y **descartado** en favor de los 4 portales actuales
- Portales descartados documentados en Acquisition-Strategy.md (Vivanuncios, Lamudi, Metros Cúbicos, Propiedades.com, Solili, LoopNet)
- Relacionado con: [ADR-003](ADR-003-Trigger-dev.md) (plataforma de ejecución), [ADR-007](ADR-007-Firecrawl.md) (motor de scraping), [ADR-008](ADR-008-Browserbase.md) (cloud browser)

---

## Actualización (Marzo 2026)

**Cambio de estrategia I24**: La migración de Inmuebles24 de Apify+Clay a Trigger.dev+Firecrawl se adelantó de Sprint 7-8 a **Sprint 1-2** (Mar 2-29). Esto eliminará la dependencia de Apify ($300/mes) y Clay, unificando todos los portales bajo Trigger.dev+Firecrawl.

- **Sprint 1**: pruebas de viabilidad técnica y económica
- **Sprint 2**: migración completa — config + push a Supabase + gestión de lógica

**Estado actual de scrapers** (mar 2026):
- CBRE: ✅ Producción (Trigger.dev + Firecrawl, cron martes)
- Colliers: ✅ Producción (Trigger.dev + Firecrawl + Browserbase, cron lunes)
- FINSA: ✅ Producción (Trigger.dev + API directa, cron día 1 y 15)
- Pincali: ✅ Producción (Trigger.dev + Firecrawl + Browserbase, cron lunes, 1,761 props)
- I24: ⚠️ Migrando de Apify a Trigger.dev+Firecrawl
