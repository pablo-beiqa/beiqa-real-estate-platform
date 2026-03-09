# Beiqa Platform — Resumen Ejecutivo

**Versión**: 2.0 | **Fecha**: Marzo 2026 | **Estado**: Desarrollo Activo — Sprint 1

---

## TL;DR

Beiqa es una empresa de **representación de tenants corporativos** en bienes raíces comerciales/industriales (CDMX, EdoMex, Morelos, Puebla). La plataforma tecnológica automatiza la búsqueda de propiedades, centraliza datos de múltiples portales inmobiliarios, y genera inteligencia de mercado para cerrar más deals, más rápido.

**Estado actual**: El equipo de 5 personas (desarrollo 100% interno) ya tiene en producción una base de datos con ~25,000 propiedades (I24) + 4 scrapers adicionales activos (CBRE, Colliers, FINSA, Pincali), CRM integrado con HubSpot, y un scoring dashboard funcional. El costo operativo verificado es de **$747–$896 USD/mes** y el desarrollo no tiene costo incremental (equipo interno).

**Diferenciación**: Mientras brokers tradicionales buscan manualmente, Beiqa cubre el mercado completo a un costo de $0.003–$0.009 USD por propiedad nueva. Con 6 agentes AI en diseño (matching inteligente, deduplicación cross-portal, inteligencia de mercado), la plataforma transforma datos crudos en recomendaciones accionables para clientes corporativos.

---

## El Problema

| Problema Actual | Impacto | Cómo Beiqa lo Resuelve |
|----------------|---------|------------------------|
| Búsqueda manual en múltiples portales | Pérdida de tiempo, oportunidades perdidas | Scraper automatizado + búsqueda centralizada |
| Información dispersa (Excel, HubSpot, emails) | Decisiones con información incompleta | Base de datos unificada (~25K propiedades I24 + CBRE/Colliers/FINSA/Pincali) |
| Falta de inteligencia de mercado | Menor valor agregado para clientes | Análisis y reportes automatizados por AI |
| Sin análisis geoespacial | Análisis de ubicación manual y limitado | Mapas interactivos, H3 indexing, análisis de proximidad |
| Datos desactualizados | Recomendaciones basadas en info obsoleta | Actualización automática quincenal+ |

---

## Qué Ya Está Construido

| Componente | Estado | Detalle |
|-----------|--------|---------|
| Base de datos (Supabase) | ✅ Producción | PostgreSQL 17 + PostGIS, 32 migrations, RLS, ~25K propiedades I24 + ~3K otros portales |
| Scraper Inmuebles24 | ⚠️ Migrando | Apify → Trigger.dev+Firecrawl (Sprint 1-2), ~25K propiedades |
| Scraper FinSA | ✅ Producción | beiqa-scraper, API directa, H3, validación, Slack, flyers |
| Scrapers CBRE, Colliers | ✅ Producción | Trigger.dev + Firecrawl, persistencia a Supabase + Storage |
| Scraper Pincali | ✅ Producción | Trigger.dev + Firecrawl, persistencia a `pincali_listings` (1,761 propiedades) |
| CRM | ✅ Activo | HubSpot — clientes, deals, pipeline |
| Geocodificación | ✅ Activo | Google Maps Platform (Geocoding + Places API) |
| GIS | ✅ Activo | Atlas.co (2-3 usuarios), H3 indexing en pruebas |
| Orquestación | ✅ Activo | Trigger.dev — scrapers, cron, sync, batch AI |
| Frontend | 🟡 Phase 0 | Next.js 15, scoring dashboard funcional |
| UI interim | ✅ Activo | Rube + Claude Desktop (MCP bridge) — transitorio |

---

## Stack Tecnológico

| Capa | Tecnología | Función |
|------|-----------|---------|
| Base de datos | Supabase (PostgreSQL 17 + PostGIS + Auth + Storage + REST API) | Source of truth de propiedades |
| Scraping | Apify + Firecrawl + Browserbase | Extracción multi-portal |
| Orquestación | Trigger.dev | Scrapers, cron, sync, batch AI |
| AI Agents | Mastra (en implementación) | Enrichment, normalization, dedup, scoring, market intel |
| Frontend | Next.js 15 | Tenant Portal + Internal App (Fase 2-3) |
| CRM | HubSpot | Clientes y deals |
| GIS | Google Maps Platform + H3 + Atlas.co | Geocodificación + análisis geoespacial |

Todas las decisiones de arquitectura están documentadas en 21 ADRs (14 aceptados, 5 propuestos, 2 deprecados). Ver [Stack-Decidido.md](../02-Architecture/Stack-Decidido.md) para el detalle completo.

---

## Arquitectura AI — 6 Agentes Mastra

La capa de inteligencia artificial es transversal a toda la plataforma. Framework: [Mastra](https://mastra.ai) (open source, Apache 2.0, $0).

| Agente | Qué hace | Prioridad | Sprint |
|--------|---------|-----------|--------|
| Address Enrichment | Corrige direcciones y coordenadas incorrectas (~50% de I24/Pincali) | P0 | 1 |
| Data Normalization | Normaliza datos de múltiples portales a un golden record unificado | P0 | 1-2 |
| Deduplication | Detecta misma propiedad en diferentes portales (híbrido: determinístico + LLM) | P1 | 3 |
| Scoring / Matching | Cruza requerimientos de clientes con propiedades para generar shortlists rankeadas | P1 | 3-4 |
| Market Intelligence | Tendencias de precio, oferta/demanda, comparables por zona | P2 | 5-6 |
| GIS Analysis | H3 indexing, AGEB lookup, proximidad, calidad de zona | P2 | 5-6 |

Ver [Agent-Architecture.md](../02-Architecture/Agent-Architecture.md) para la arquitectura completa con tools, métricas de evaluación, y diagramas de secuencia.

---

## Equipo

| Persona | Rol | Responsabilidad |
|---------|-----|-----------------|
| **Pablo** | CEO / Product Owner | Visión, prioridades, decisiones de negocio. Desarrolla frontend + agentes AI |
| **Fabrizio** | Tech Lead | Scraper, Supabase, Trigger.dev, H3, arquitectura. Desarrolla infraestructura |
| **Pamela** | Frontend Design | Diseño de Internal App y Tenant Portal (Figma) |
| **Jerónimo** | Ops | Operación con clientes, feedback de campo |
| **Alex** | Advisor externo | Consultoría técnica estratégica |

**Desarrollo**: 100% interno. Sin outsource.

---

## Presupuesto Operativo Verificado

| Métrica | Valor |
|---------|-------|
| **Costo mensual actual** | **$747 – $896 USD** |
| **Costo mensual con Mastra + Hosting** | **~$834 – $1,004 USD** |
| Proyección 12 meses | $942 – $1,308 USD/mes |
| Costo de desarrollo | $0 incremental (equipo interno) |

**Distribución por categoría:**

| Categoría | USD/mes | % del total |
|-----------|---------|-------------|
| Extracción (scraping) | $403 – $423 | 47 – 54% |
| GIS / Mapas | $178 – $267 | 20 – 30% |
| AI Workspace (Rube + Claude Desktop) | $75 – $100 | 10 – 11% |
| Orquestación (Trigger.dev) | $50 | 6 – 7% |
| AI Agents — Mastra LLM | ~$67 – $88 | 7 – 10% |
| Hosting (Vercel Pro) | $20 | ~2% |
| Infraestructura (Supabase + dominio) | $26 | ~3% |

**Costo por propiedad nueva**: $0.003 – $0.009 USD (extracción + AI processing + geocoding).

Ver [Total-Budget.md](../04-Validation/Total-Budget.md) para desglose completo, proyecciones, y puntos de inflexión.

---

## Roadmap

**Metodología**: Scrum (sprints de 2 semanas). Tracks paralelos: Scraper, AI/Mastra, Frontend.

| Milestone | Sprints | Due Date | Qué entrega |
|-----------|---------|----------|-------------|
| **M1: Datos Confiables** | 1-2 | Mar 29 | Scrapers en Supabase, golden record, primer agente AI, auth en portal |
| **M2: Búsqueda Inteligente** | 3-4 | Abr 26 | Scoring AI, dedup cross-portal, shortlists para clientes |
| **M3: Experiencia del Cliente** | 5-6 | May 24 | Portal live, feedback de clientes, pipeline E2E automatizado |
| **M4: Operación Estable** | 7-8 | Jun 21 | Sistema estable, CI/CD, evals, documentación operativa |

**Sprint activo**: Sprint 1 (Mar 2-15) — Address Enrichment Agent, CBRE persistence a Supabase, golden record migrations, login magic link.

Ver [Roadmap.md](../03-Roadmap/Roadmap.md) para OKRs, deliverables, y acceptance criteria por sprint.

---

## Riesgos Actuales

| Riesgo | Probabilidad | Mitigación implementada |
|--------|-------------|------------------------|
| Portales bloquean scraping | Media | Apify (cloud actor), Firecrawl (stealth proxy), Browserbase (browser sessions) |
| LLM no funciona bien en español inmobiliario | Media | Evaluación de ≥2 modelos por agente, métricas de calidad definidas |
| Deduplicación imprecisa cross-portal | Media | Algoritmo híbrido (determinístico + LLM), target >85% precisión |
| Scope creep | Alta | Scrum, backlog centralizado en GitHub Issues, sprints de 2 semanas |
| Costos de APIs escalan | Baja | Presupuesto con puntos de inflexión documentados, crédito Google $200/mes |

---

## Valor para el Negocio

### Para el Equipo de Beiqa

| Beneficio | Impacto esperado |
|-----------|-----------------|
| Búsqueda automatizada | Reducir tiempo de búsqueda en 50%+ |
| Datos consolidados de 5 portales | Una sola fuente de verdad (~25K propiedades I24 + CBRE/Colliers/FINSA/Pincali) |
| AI matching | Shortlists rankeadas en minutos, no días |
| Inteligencia de mercado | Reportes automatizados por zona/tipo |

### Para los Clientes

| Beneficio | Impacto esperado |
|-----------|-----------------|
| Portal dedicado | Ver opciones preseleccionadas en cualquier momento |
| Feedback integrado | Comunicar preferencias directamente en la plataforma |
| Reportes de mercado | Tomar decisiones informadas con datos reales |

### Para el Negocio

| Beneficio | Impacto esperado |
|-----------|-----------------|
| Diferenciación | Servicio único en el mercado mexicano de representación de tenants |
| Escalabilidad | Más clientes sin más personal |
| Datos de mercado | Posicionamiento como expertos con evidencia cuantitativa |

---

## Contacto

**Project Owner**: Pablo — CEO / Product Owner
**Repositorio**: [beiqa-real-estate-platform](https://github.com/pablo-beiqa/beiqa-real-estate-platform)
**Última actualización**: 2026-03-09
