---
status: "propuesto"
date: 2026-02-27
decision-makers: "Pablo (CEO), Pamela (Frontend)"
consulted: "Fabrizio (Tech Lead)"
costo-mensual: "por definir (hosting)"
---

# Frontend Next.js — Detalles de Implementación

## Contexto y Problema

La decisión macro de usar Next.js para el frontend ya está tomada (Pamela asignada). Pero los detalles de implementación no están definidos:

- ¿App Router o Pages Router?
- ¿Qué sistema de componentes UI? (shadcn/ui, Tailwind puro, Material UI)
- ¿Monorepo para Internal App + Tenant Portal, o repos separados?
- ¿Cómo se conecta al backend? (Supabase client directo, edge functions, API routes)

Estos detalles se definen cuando Pamela inicie desarrollo activo (Fase 2).

## Factores de Decisión

* Internal App (uso interno) y Tenant Portal (clientes externos) comparten stack
* Supabase como backend (REST API automática, Auth, Storage)
* Pamela es la desarrolladora principal
* Necesidad de mapas interactivos (Fase 2)
* Performance no es crítica en MVP (4-10 usuarios concurrentes)

## Opciones a Evaluar

### App Router vs Pages Router
* App Router: nuevo estándar de Next.js, server components, streaming
* Pages Router: más estable, más documentación, más predecible

### Sistema de componentes
* shadcn/ui: componentes accesibles, Tailwind-based, copy-paste (no dependencia)
* Tailwind puro: máxima flexibilidad, sin componentes pre-hechos
* Radix + Tailwind: primitives accesibles + styling custom
* Material UI: componentes completos pero más pesado

### Monorepo vs repos separados
* Monorepo (Turborepo): compartir componentes UI, types, config
* Repos separados: independencia total, deploy separado

## Decisión

**Pendiente.** Se definirá cuando Pamela inicie desarrollo activo de la Internal App (Fase 2-3).

## Información Adicional

- Stack decidido: Next.js ([Stack-Decidido.md](../Stack-Decidido.md))
- UI actual: Rube + Claude Desktop ([ADR-004](ADR-004-Rube-MCP-Bridge.md))
- Internal App: [01-Modules/Internal-App/README.md](../../01-Modules/Internal-App/README.md)
- Tenant Portal: [01-Modules/Tenant-Portal/README.md](../../01-Modules/Tenant-Portal/README.md)
