---
status: "aceptado"
date: 2026-02-04
decision-makers: "Fabrizio (Tech Lead), Pablo (CEO)"
consulted: "Alex (Advisor)"
informed: "Pamela, Jerónimo"
costo-mensual: "$0-25"
---

# Supabase como Plataforma (DB + Auth + Storage + API)

## Contexto y Problema

BEIQA necesita una plataforma de backend que soporte:

- Datos relacionales (propiedades, brokers, tenants, deals)
- Datos geoespaciales (coordenadas, búsqueda por área, análisis por zona)
- Volumen de 60K+ propiedades (ya alcanzado en Fase 1)
- Consultas complejas con filtros múltiples
- API automática para el frontend (sin backend custom)
- Autenticación para la Internal App y Tenant Portal
- Almacenamiento de imágenes y documentos de propiedades

¿Qué plataforma usamos como backend all-in-one que cubra base de datos, autenticación, storage y API, minimizando servicios independientes y costos operativos?

## Factores de Decisión

* Equipo pequeño (5 personas) — no hay capacidad para mantener múltiples servicios
* Presupuesto mínimo — target $30-50/mo en herramientas
* Necesidad geoespacial desde el día 1 (PostGIS)
* Sin backend developer dedicado — la API debe ser automática
* Velocidad de implementación — time-to-market es crítico

## Opciones Consideradas

* Supabase (PostgreSQL 15 + PostGIS + Auth + Storage + REST API)
* PostgreSQL + PostGIS self-hosted
* MySQL + MySQL Spatial
* MongoDB
* Neon / AWS RDS / GCP Cloud SQL

## Decisión

Opción elegida: "**Supabase**", porque provee PostgreSQL con PostGIS, Auth, Storage, REST API automática y RLS en un solo servicio. Elimina la necesidad de Auth0/Clerk, S3, FastAPI/Express como servicios separados.

### Consecuencias

* Bien, porque API REST lista desde el primer día sin backend custom
* Bien, porque Auth incluido (sin Auth0, sin Clerk, sin JWT custom)
* Bien, porque Storage incluido (sin S3, sin Cloudflare R2)
* Bien, porque PostGIS para geoespacial desde el día 1
* Bien, porque costo mínimo en early stage ($0-25/mo)
* Bien, porque sistema de migrations limpio, compatible con flujo de Fabrizio
* Neutral, porque vendor lock-in parcial (Supabase-specific features)
* Mal, porque el free tier tiene límites que habrá que monitorear

## Plan de Implementación

* **Sistemas afectados**: Supabase (proyecto principal), `beiqa-scraper` (escritura de datos), HubSpot (sync)
* **Dependencias**: `@supabase/supabase-js` en cualquier cliente, extensión PostGIS habilitada
* **Patrones a seguir**: Usar Supabase Auth para toda autenticación. Usar RLS para control de acceso. Usar REST API automática (no construir backend custom). Usar migrations para cambios de schema.
* **Patrones a evitar**: No usar Auth0, Clerk, ni JWT custom. No crear backend FastAPI/Express. No usar S3/R2 para storage.
* **Configuración**: `SUPABASE_URL`, `SUPABASE_ANON_KEY`, `SUPABASE_SERVICE_ROLE_KEY`

### Verificación

- [x] PostgreSQL 17 + PostGIS 3.3.7 activo en producción
- [x] 32 migrations implementadas
- [x] ~24,700 propiedades en `inmuebles24_listings` + ~3K en otros 4 portales
- [x] Triggers operando en las 5 tablas de listings (populate_geo, price_per_m2, log_price_change, update_last_seen)
- [x] RLS configurado
- [x] 19+ RPC functions: tendencia_precios, precio_promedio_m2, buscar_listings, buscar_por_radio, broker_stats, etc.
- [x] Supabase Storage configurado (6 buckets: cbre-images, cbre-pdfs, colliers-images, colliers-pdfs, finsa-flyers, pincali-images)
- [ ] Internal App usando Supabase Auth

## Pros y Contras por Opción

### Supabase (PostgreSQL 17 + PostGIS + Auth + Storage + REST API)

* Bien, porque all-in-one: DB + Auth + Storage + REST API en un solo servicio
* Bien, porque PostGIS incluido — soporte geoespacial completo
* Bien, porque Row Level Security nativo — control de acceso a nivel DB
* Bien, porque Free tier → Pro tier ($0 → $25/mo)
* Bien, porque 32 migrations implementadas — setup rápido y iterativo
* Mal, porque vendor lock-in parcial en features específicos de Supabase

### PostgreSQL + PostGIS self-hosted

* Bien, porque sin vendor lock-in, control total
* Mal, porque setup complejo, necesita DevOps, más lento para iterar

### MySQL + MySQL Spatial

* Bien, porque conocido y ampliamente adoptado
* Mal, porque geoespacial inferior a PostGIS

### MongoDB

* Bien, porque schema flexible
* Mal, porque no ACID completo, geoespacial limitado

### Neon / AWS RDS / GCP Cloud SQL

* Bien, porque PostgreSQL managed sin vendor lock-in
* Mal, porque no agregan Auth + Storage + REST API como Supabase

## Información Adicional

- Schema implementado: [Database/Schema-Real.md](../Database/Schema-Real.md)
- Modelo de datos target: [Database/Data-Model.md](../Database/Data-Model.md)
- Stack completo: [Stack-Decidido.md](../Stack-Decidido.md)
- Monitorear: cuando jobs de Trigger.dev generen carga analítica, considerar read replica o schema `analytics` con materialized views
- **Supersede**: Este ADR reemplaza al archivo anterior `ADR-001-Database-Choice.md`

---

*ADR creado: 2026-02-04 | Migrado a MADR 4.0: 2026-02-27*
