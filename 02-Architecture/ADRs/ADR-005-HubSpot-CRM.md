---
status: "aceptado"
date: 2026-02-04
decision-makers: "Pablo (CEO)"
informed: "Fabrizio, Jerónimo, Pamela"
costo-mensual: "plan existente"
---

# HubSpot CRM — Source of Truth de Clientes y Deals

## Contexto y Problema

BEIQA necesita un CRM para gestionar clientes corporativos (tenants), deals de representación, y comunicación comercial. ¿Usamos HubSpot (ya en uso en la empresa) o adoptamos otro CRM?

## Decisión

**HubSpot** como CRM y source of truth para clientes, deals, y comunicación comercial. Sincronización one-way Supabase → HubSpot para propiedades y brokers.

**Source of Truth Rules:**
- **Supabase**: propiedades, listings, brokers, analytics, geo data
- **HubSpot**: clientes, deals, comunicación comercial

**Sync:**
- Supabase → HubSpot: one-way para propiedades y brokers (via Trigger.dev)
- HubSpot → Supabase: one-way para deal status
- Clay: enriquece datos que van a HubSpot (transitorio, saliendo del stack)

### Consecuencias

* Bien, porque HubSpot ya está en uso — cero migración
* Bien, porque separación clara de source of truth (Supabase = datos, HubSpot = CRM)
* Mal, porque sync one-way puede causar datos desactualizados temporalmente
* Neutral, porque Clay como intermediario es transitorio

## Plan de Implementación

* **Sistemas afectados**: HubSpot (CRM), Supabase (datos), Trigger.dev (sync jobs), Clay (transitorio)
* **Dependencias**: HubSpot API, Trigger.dev tasks para sync
* **Patrones a seguir**: Supabase es source of truth para propiedades. HubSpot es source of truth para clientes.
* **Patrones a evitar**: No duplicar source of truth. No hacer sync bidireccional complejo.

### Verificación

- [x] HubSpot activo con clientes y deals
- [x] Sync básico de brokers nuevos a HubSpot
- [ ] Sync completo Supabase → HubSpot via Trigger.dev (migrar de n8n/Clay)
- [ ] Clay removido del pipeline de sync

## Información Adicional

- Integraciones detalladas: [Stack-Decidido.md](../Stack-Decidido.md) (Source of Truth Rules)
- Clay es transitorio — su lógica se replica en Trigger.dev
- Relacionado con: [ADR-003](ADR-003-Trigger-dev.md) (sync via Trigger.dev)
