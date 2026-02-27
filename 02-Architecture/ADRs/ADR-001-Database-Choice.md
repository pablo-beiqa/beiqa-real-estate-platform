# ADR-001: Elección de Base de Datos (ARCHIVO HISTÓRICO)

> **REEMPLAZADO**: Este archivo fue reemplazado por [ADR-001-Supabase-Plataforma.md](ADR-001-Supabase-Plataforma.md) que usa formato MADR 4.0 y cubre Supabase como plataforma completa (DB + Auth + Storage + API).
> Se mantiene como referencia histórica del ADR original.

**Fecha**: 2026-02-04
**Estado**: ✅ Aceptado — Febrero 2026
**Decisores**: Fabrizio (Tech Lead), Pablo (CEO)

---

## Decisión

**Supabase (PostgreSQL 15 + PostGIS)**

Supabase fue elegido como la plataforma de base de datos. Provee PostgreSQL con PostGIS, junto con Auth, Storage, REST API automática y RLS en un solo servicio.

---

## Contexto

BEIQA necesita una base de datos que soporte:
- Datos relacionales (propiedades, brokers, tenants, deals)
- Datos geoespaciales (coordenadas, búsqueda por área, análisis por zona)
- Volumen de 60K+ propiedades (ya alcanzado en Fase 1)
- Consultas complejas con filtros múltiples
- API automática para el frontend (sin backend custom)

---

## Justificación de la decisión

| Criterio | Por qué Supabase |
|---------|-----------------|
| **All-in-one** | DB + Auth + Storage + REST API automática en un solo servicio. Sin necesidad de FastAPI, Express, ni Auth0 separados. |
| **PostGIS incluido** | Soporte geoespacial completo desde el día 1. Extensión habilitada. |
| **Row Level Security** | RLS nativo permite controlar qué datos ve cada usuario directamente en la DB. |
| **Free tier → Pro tier** | $0 en desarrollo, $25/mes en producción. No pagar por lo que no se usa. |
| **API REST automática** | Supabase genera automáticamente una API REST desde el schema. Sin backend custom. |
| **Migrations** | Sistema de migrations limpio, compatible con el flujo de trabajo de Fabrizio. |
| **Velocidad de implementación** | 14 migrations implementadas en días. Setup mucho más rápido que self-hosted. |

---

## Estado actual (Febrero 2026)

- 14 migrations activas en producción
- ~60,000 propiedades en tabla `inmuebles24_listings`
- PostGIS activo: trigger `populate_geo` operando
- RLS configurado
- RPC functions: `tendencia_precios`, `precio_promedio_m2`
- Costo real: $0-25/mes (free tier → Pro cuando se active más carga)

Ver schema completo: [Database/Schema-Real.md](../Database/Schema-Real.md)

---

## Opciones consideradas (para referencia)

### 1. PostgreSQL + PostGIS (self-hosted)
- Pros: sin vendor lock-in, control total
- Contras: setup complejo, necesita DevOps, más lento para iterar
- **No elegido**: Supabase provee lo mismo con menos overhead

### 2. MySQL + MySQL Spatial
- Pros: conocido
- Contras: geoespacial inferior a PostGIS
- **No elegido**: geoespacial es crítico

### 3. MongoDB
- Pros: schema flexible
- Contras: no ACID completo, geoespacial limitado
- **No elegido**: datos relacionales y transacciones necesarios

### 4. Neon / AWS RDS / GCP Cloud SQL
- Alternativas de PostgreSQL managed
- **No elegido**: Supabase agrega Auth + Storage + REST API que los otros no tienen

---

## Consecuencias

### Positivas
- API REST lista desde el primer día (sin backend custom)
- Auth incluido (Supabase Auth, sin Auth0)
- Storage incluido (sin S3)
- PostGIS para geoespacial
- Costo mínimo en early stage

### A monitorear
- Cuando los jobs de Trigger.dev (batch AI) generen carga analítica real, considerar read replica o schema `analytics` con materialized views
- Supabase tiene límites en el free tier — monitorear uso

---

## Decisiones relacionadas

- Stack completo: [02-Architecture/Stack-Decidido.md](../Stack-Decidido.md)
- Schema implementado: [02-Architecture/Database/Schema-Real.md](Schema-Real.md)

---

*ADR creado: 2026-02-04 | Actualizado a ✅ Aceptado: 2026-02-24*
