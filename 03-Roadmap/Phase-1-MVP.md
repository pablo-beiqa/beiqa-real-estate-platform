# Roadmap: Las 3 Fases del Proyecto

> **Actualizado**: 2026-02-24
>
> Este documento reemplaza el plan original de MVP de 6-8 semanas con 4 sprints. El proyecto ya está en desarrollo activo.
>
> Para el estado detallado de la Fase 1 actual: **[→ Fase-Real-1-Scrapers.md](./Fase-Real-1-Scrapers.md)**

---

## Visión del roadmap

```
FASE 1: Scrapers & Inventario     FASE 2: Portal & Datos           FASE 3: AI Brain
(4-5 semanas)                     (4-5 semanas)                    (4-5 semanas)
━━━━━━━━━━━━━━━━━━━━━━            ━━━━━━━━━━━━━━━━━━━━━━━━━        ━━━━━━━━━━━━━━━━━━
🟢 EN CURSO                       🔴 Por iniciar                   🔴 Por iniciar

• Apify actor Inmuebles24 ✅       • Portal Web (Next.js)           • AI Brain de llamadas
• ~60K propiedades en DB ✅        • EasyBroker (portal 2)          • Procesamiento CircleBack
• n8n workflows ✅                 • INEGI + Google Places           • Matching propiedades IA
• Trigger.dev + Claude AI ✅       • H3 index + AGEB                 • Recomendaciones proactivas
• Internal App (Next.js)           • Market Intelligence             • NLP en búsquedas
• Deduplicación activa             • Tenant Portal completo
• Normalización completa
```

---

## Fase 1: Scrapers & Inventario

**Estado**: 🟢 En curso | **Owner**: Fabrizio

### Objetivo
Inventario completo y actualizado de propiedades comerciales en CDMX/EdoMex/Morelos/Puebla, accesible para el equipo vía Internal App.

### Entregables clave
- Apify scraping Inmuebles24 + EasyBroker
- ~60K+ propiedades normalizadas en Supabase
- Deduplicación activa
- AI extraction de `property_features` (Trigger.dev + Claude)
- Internal App (Next.js): lista, mapa, filtros, shortlists

### Stack
Apify → n8n → Supabase ← Trigger.dev (Claude API) ← Next.js

---

## Fase 2: Portal Web + Data Ingestion

**Estado**: 🔴 Por iniciar | **Prerequisito**: Fase 1 completa

### Objetivo
Clientes de Beiqa (tenants) pueden ver sus shortlists directamente en un portal. El sistema tiene datos contextuales de zonas (INEGI, Google) enriqueciendo cada propiedad.

### Entregables clave
- **Tenant Portal** (Next.js): los clientes ven sus opciones, dan feedback, ven en mapa
- **Data Ingestion**: Google Places (POIs cercanos), INEGI DENUE (datos socioeconómicos)
- **H3 index**: cada listing tiene H3 index precalculado al ingresar (resolución 7 y 9)
- **AGEB**: cada listing tiene AGEB de INEGI (spatial join con shapefiles)
- **Cache de APIs**: tabla `cache_api_responses` para no repetir requests a Google/INEGI por zona
- **Market Intelligence básico**: tendencia de precios por zona, precio promedio m2, heatmaps

### Stack adicional
INEGI APIs + Google Places → n8n → Supabase (con H3 + AGEB)

### Decisiones pendientes para Fase 2
- Confirmar proveedor de mapas (Mapbox vs Leaflet+OSM vs Google Maps)
- Diseño de Tenant Portal (Pamela)
- Granularidad de H3 (resolución 7 para zonas grandes, 9 para búsquedas precisas)

---

## Fase 3: AI Brain + Procesamiento de Llamadas

**Estado**: 🔴 Por iniciar | **Prerequisito**: Fase 2 completa

### Objetivo
El sistema aprende de las llamadas del equipo con clientes y puede hacer matching inteligente de propiedades, anticipar necesidades y procesar comunicaciones automáticamente.

### Entregables clave
- **CircleBack** integrado: transcripción automática de llamadas con clientes
- **Procesamiento de transcripts**: Trigger.dev extrae criterios de búsqueda de transcripts
- **Property Matching AI**: dado un cliente, el sistema sugiere propiedades con score de match
- **Recomendaciones proactivas**: cuando entra una propiedad nueva, el sistema la matchea contra búsquedas activas
- **NLP en búsquedas**: el equipo puede describir lo que busca en lenguaje natural

### Stack adicional
CircleBack → Trigger.dev (Claude API) → Supabase → n8n (notificaciones match)

---

## Qué NO está en el roadmap

Decisiones arquitectónicas confirmadas como "no necesario ahora":

| Item | Razón |
|------|-------|
| Separar DB operativa / analítica | Prematuro: 4 usuarios, 60K listings |
| Redis cache | No necesario: volumen actual |
| GraphQL API | Supabase REST es suficiente |
| FastAPI / Express custom | Supabase REST cubre el caso de uso |
| Data Lake | Una sola DB centralizada es correcto para el volumen actual |
| AWS S3 / GCP Cloud Storage | Supabase Storage incluido |

---

*Documento actualizado: 2026-02-24*
*Reemplaza el plan original de "4 sprints, 6-8 semanas" de discovery phase*
