# Fase 1: Scrapers & Inventario

> **Estado**: 🟢 En curso | **Inicio**: Febrero 2026 | **Timeline estimado**: 4-5 semanas

---

## Objetivo

Construir el inventario completo de propiedades comerciales e industriales disponibles en el mercado de CDMX, Estado de México, Morelos y Puebla. Que el equipo de Beiqa tenga acceso en tiempo real a toda la oferta del mercado sin búsqueda manual.

---

## ¿Qué estamos construyendo?

```
Inmuebles24 (vía Apify)
        ↓
      n8n (webhook + normalización)
        ↓
    Supabase DB
    (inmuebles24_listings)
        ↓
  Trigger.dev (batch AI extraction)
        ↓
   property_features pobladas
```

---

## Estado actual

| Componente | Estado | Detalle |
|-----------|--------|---------|
| Supabase en producción | ✅ Activo | 14 migrations, PostGIS, RLS |
| Apify actor Inmuebles24 | ✅ Activo | Actor contratado, corriendo |
| ~60K propiedades en DB | ✅ Activo | Al 24 feb 2026 |
| Cron semanal (n8n) | ✅ Activo | Ejecución automática |
| Trigger manual (Slack) | ✅ Activo | Jerónimo puede disparar desde Slack |
| HubSpot sync (básico) | ✅ Activo | Brokers nuevos → HubSpot |
| Normalización multi-portal | 🟡 En progreso | n8n workflows |
| Deduplicación activa | 🟡 En progreso | tabla `possible_duplicates` |
| Batch AI extraction | 🟡 En progreso | Trigger.dev + Claude API |
| Internal App (Next.js) | 🔴 Por iniciar | Pamela asignada |
| EasyBroker (segundo portal) | 🔴 Por iniciar | Apify actor pendiente |

---

## Entregables de Fase 1

### Scraping & Datos
- [x] Apify actor Inmuebles24 operando
- [x] ~60K propiedades en sistema
- [ ] Normalización completa en n8n (todos los campos)
- [ ] Deduplicación activa con reviews automáticas
- [ ] `property_features` pobladas con AI para listings actuales
- [ ] EasyBroker como segundo portal
- [ ] Cobertura: CDMX, EdoMex, Morelos, Puebla

### Internal App (Next.js)
- [ ] Auth con Supabase Auth
- [ ] Lista de propiedades con filtros: tipo, operación, precio, superficie, zona
- [ ] Vista de mapa (propiedades en PostGIS → mapa interactivo)
- [ ] Detalle de propiedad con todos los campos
- [ ] Gestión básica de tenants y búsquedas
- [ ] Shortlists por cliente

---

## Stack en uso

| Componente | Tecnología |
|-----------|-----------|
| Scraping | Apify (actor contratado) |
| Orquestación | n8n Cloud (cron, webhooks, Slack) |
| Jobs AI | Trigger.dev (batch extraction) |
| Base de datos | Supabase (PostgreSQL 15 + PostGIS) |
| AI/NLP | Claude API (vía Rube) |
| Frontend | Next.js (Pamela) |
| Auth | Supabase Auth |

---

## Criterios de salida → Fase 2

Pasamos a Fase 2 (Portal Web + Data Ingestion) cuando:

- [ ] >80% de listings tienen `property_features` pobladas
- [ ] Deduplicación activa con tasa de falsos positivos < 5%
- [ ] Internal App funcional en uso diario por el equipo (Fabrizio, Jerónimo)
- [ ] EasyBroker como segundo portal activo
- [ ] Frescura de datos: >80% actualizados en últimos 7 días

---

## Qué viene en Fase 2

→ **[Roadmap completo](./Phase-1-MVP.md)**

- Portal Web para clientes (tenants ven sus shortlists)
- Data Ingestion: INEGI DENUE, Google Places, AGEB
- H3 index para analytics geoespaciales
- Market Intelligence básico (tendencias de precio por zona)

---

*Documento creado: 2026-02-24*
