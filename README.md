# BEIQA Real Estate Platform

> Plataforma tecnológica interna que centraliza la búsqueda de inmuebles comerciales/industriales, inteligencia de mercado y gestión de clientes para el equipo de representación de tenants corporativos de Beiqa.

**Fase actual**: 🟢 Desarrollo Activo — Fase 1: Scrapers & Inventario
**Última actualización**: 2026-02-24

---

## ¿Qué es BEIQA Platform?

BEIQA Platform es una herramienta interna diseñada para **reducir el tiempo de búsqueda de propiedades en 50%** y **aumentar la calidad de las recomendaciones** mediante:

- **Scraping automatizado** de portales inmobiliarios (Apify + n8n)
- **Inteligencia de mercado** consolidada de múltiples fuentes
- **Análisis geoespacial** avanzado (PostGIS + H3)
- **Portal para clientes** con seguimiento de opciones

**Objetivo principal**: Diferenciar a Beiqa de brokers tradicionales mediante tecnología y datos superiores.

> **[Leer más sobre la visión y objetivos →](./00-Project/Vision-and-Goals.md)**

---

## Quick Links

### Para entender el proyecto
- [Visión y Objetivos](./00-Project/Vision-and-Goals.md) — ¿Qué queremos lograr?
- [Contexto de Negocio](./00-Project/Business-Context.md) — ¿Por qué existe este proyecto?
- [Resumen Ejecutivo](./05-Communication/Executive-Summary.md) — Overview para stakeholders

### Para el equipo técnico
- [Stack Decidido](./02-Architecture/Stack-Decidido.md) — Todas las decisiones técnicas tomadas
- [Arquitectura del Sistema](./02-Architecture/System-Architecture.md) — Diagrama del stack real
- [Integraciones](./02-Architecture/Integraciones.md) — Cómo se conectan Apify, n8n, Trigger.dev, Supabase, HubSpot
- [Schema Real de la DB](./02-Architecture/Database/Schema-Real.md) — Tablas, triggers y RPCs implementados

### Para planificación
- [Mapa de Módulos](./01-Modules/) — Todos los módulos con dependencias y fases
- [Fase 1: Scrapers & Inventario](./03-Roadmap/Fase-Real-1-Scrapers.md) — En curso ahora
- [Roadmap completo](./03-Roadmap/Phase-1-MVP.md) — Las 3 fases del proyecto
- [Presupuesto y Costos Reales](./04-Validation/Total-Budget.md) — ~$30-50/mes en herramientas

---

## Estado del Proyecto

### Fase Actual: Desarrollo Activo — Fase 1

**Estado**: 🟢 En construcción | **Timeline**: ~4-5 semanas desde inicio

#### Lo que ya está funcionando
- ✅ Base de datos Supabase en producción (14 migrations, PostGIS, RLS)
- ✅ Apify actor para Inmuebles24 contratado y operando
- ✅ ~60,000 propiedades en el sistema
- ✅ Workflows de n8n configurados (scraping, notificaciones Slack, sync HubSpot)
- ✅ Trigger.dev integrado para jobs pesados (batch AI extraction)
- ✅ Claude API (vía Rube) para procesamiento de descripciones

#### Próximas prioridades
- Normalización multi-portal en n8n
- Deduplicación activa
- Internal App (Next.js, Pamela)
- EasyBroker como segundo portal

> **[Ver detalles de Fase 1 →](./03-Roadmap/Fase-Real-1-Scrapers.md)**

---

## Módulos de la Plataforma

| Módulo | Descripción | Fase | Estado |
|--------|-------------|------|--------|
| [Scraper](./01-Modules/Scraper/) | Extracción automatizada de propiedades de portales | Fase 1 | 🟢 En desarrollo |
| [Internal App](./01-Modules/Internal-App/) | Aplicación web para el equipo Beiqa (Next.js) | Fase 1 | 🟡 Diseño activo |
| [Data Ingestion](./01-Modules/Data-Ingestion/) | Integración de fuentes externas (INEGI, Google, Catastro) | Fase 2 | 🔴 Por iniciar |
| [Market Intelligence](./01-Modules/Market-Intelligence/) | Análisis y reportes de mercado automatizados | Fase 2 | 🔴 Por iniciar |
| [Geospatial](./01-Modules/Geospatial/) | Análisis geoespacial, H3 index, AGEB | Fase 2 | 🔴 Por iniciar |
| [Tenant Portal](./01-Modules/Tenant-Portal/) | Portal web para clientes (shortlists, feedback) | Fase 2 | 🔴 Por iniciar |
| [AI Brain](./01-Modules/AI-Brain/) | Matching inteligente, NLP, procesamiento de llamadas | Fase 3 | 🔴 Por iniciar |

> **[Ver mapa completo de módulos con dependencias →](./01-Modules/)**

---

## Stack Tecnológico

| Componente | Tecnología | Estado |
|-----------|-----------|--------|
| Base de datos | Supabase (PostgreSQL + PostGIS) | ✅ En producción |
| Scraping | Apify (actor contratado) | ✅ Activo |
| Orquestación | n8n Cloud | ✅ Activo |
| Jobs pesados | Trigger.dev | ✅ Activo |
| AI/NLP | Claude API (vía Rube) | ✅ Activo |
| CRM | HubSpot | ✅ Activo |
| Frontend | Next.js | 🟡 En diseño |
| Data enrichment | Clay | ✅ Activo |

> **[Ver todas las decisiones técnicas →](./02-Architecture/Stack-Decidido.md)**

---

## Equipo

| Rol | Nombre |
|-----|--------|
| CEO / Product Owner | Pablo |
| Tech Lead | Fabrizio |
| Ops / Llamadas | Jerónimo |
| Frontend | Pamela |
| Advisor externo | Alex |

> **[Ver roles y responsabilidades completos →](./00-Project/Stakeholders.md)**

---

## Estructura de la Documentación

```
beiqa-real-estate-platform/
├── 00-Project/          # Visión, contexto, stakeholders, personas
├── 01-Modules/          # 7 módulos funcionales auto-contenidos
│   ├── Scraper/         # Extracción de datos (Apify + n8n)
│   ├── Internal-App/    # App web del equipo (Next.js)
│   ├── Data-Ingestion/  # APIs INEGI, Google, Catastro
│   ├── Market-Intelligence/  # Análisis y tendencias
│   ├── Geospatial/      # Mapas, H3, AGEB
│   ├── AI-Brain/        # IA, matching, procesamiento de llamadas
│   └── Tenant-Portal/   # Portal para clientes
├── 02-Architecture/     # Stack decidido, integraciones, DB, ADRs
├── 03-Roadmap/          # Fases del proyecto
├── 04-Validation/       # Presupuesto y costos reales
└── 05-Communication/    # Materiales para stakeholders externos
```

---

## Convenciones de la Documentación

| Emoji | Significado |
|-------|-------------|
| 🟢 | Activo / En desarrollo |
| 🟡 | En diseño / Draft |
| 🔴 | Por iniciar |
| ✅ | Completado |

| Tag | Significado |
|-----|-------------|
| **MUST** | Imprescindible para que funcione |
| **SHOULD** | Importante pero no bloqueante |
| **COULD** | Deseable si hay tiempo/recursos |

---

## Changelog del Proyecto

| Fecha | Cambio |
|-------|--------|
| 2026-02-24 | Actualización completa para reflejar estado real: stack decidido, schema real, fases actuales, equipo |
| 2026-02-24 | Reestructuración a modelo module-centric por Pablo: 6 carpetas, 7 módulos auto-contenidos |
| 2026-02-06 | Expansión de cuestionarios de producto: 463 preguntas en 9 módulos |
| 2026-02-05 | Reestructuración del README |
| 2026-02-04 | Creación de estructura inicial de documentación |

---

*Última actualización: 2026-02-24*
