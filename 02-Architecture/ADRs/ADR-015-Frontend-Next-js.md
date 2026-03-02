---
status: "parcialmente aceptado"
date: 2026-03-02
decision-makers: "Pablo (CEO), Pamela (Frontend)"
consulted: "Fabrizio (Tech Lead)"
costo-mensual: "$0 (Vercel free tier)"
---

# Frontend Next.js — Detalles de Implementación

## Contexto y Problema

La decisión macro de usar Next.js para el frontend ya está tomada (Pamela asignada). Varios detalles de implementación se han definido al construir el Phase 0 del Tenant Portal en [beiqa-frontend](https://github.com/pablo-beiqa/beiqa-frontend).

## Factores de Decisión

* **Repos separados**: Tenant Portal (beiqa-frontend) e Internal App (repo futuro) son proyectos independientes
* Supabase como backend (REST API automática, Auth, Storage)
* Pamela es la desarrolladora principal
* Necesidad de mapas interactivos (Fase 2) — Atlas.co como proveedor preferido
* Performance no es crítica en MVP (5-15 clientes concurrentes)
* Mobile + desktop con importancia igual desde día 1

## Decisiones Tomadas

### App Router ✅ Decidido

**Elección**: App Router (nuevo estándar de Next.js 15)

Implementado en beiqa-frontend con server components, route groups `(dashboard)`, y `generateStaticParams` para pre-rendering.

### Sistema de componentes ✅ Decidido

**Elección**: shadcn/ui + Tailwind CSS

3 componentes implementados: Card, Badge, Table. Estilo "new-york", paleta neutral. CSS variables para theming con soporte de modo oscuro.

### Conexión al backend ✅ Decidido

**Elección**: Supabase SSR client directo + HubSpot API

- Browser client: `@supabase/supabase-js` con anon key
- Server client: `@supabase/ssr` con service role key
- HubSpot: API REST directa con Bearer token (server-side only)

### Repos separados ✅ Decidido

**Elección**: Repos independientes, mismo stack

- `beiqa-frontend` = Tenant Portal (client-facing)
- Internal App = repo futuro (team-facing)
- Mismo stack (Next.js 15 + shadcn/ui) pero sin monorepo

## Decisiones Pendientes

### Monorepo vs componentes compartidos
Si Internal App y Tenant Portal comparten componentes, se evaluará extraer un paquete de UI compartido. Por ahora, cada repo es independiente.

### Deploy y hosting
Vercel es la opción natural para Next.js. Pendiente crear proyecto en Vercel y configurar CI/CD ([ADR-018](ADR-018-CI-CD.md)).

### Mapas
Atlas.co como proveedor preferido ([ADR-017](ADR-017-Plataforma-GIS.md)). Pendiente evaluación técnica de pricing e integración con embed.

## Implementación Actual (Phase 0)

**Repo**: [beiqa-frontend](https://github.com/pablo-beiqa/beiqa-frontend)

| Componente | Estado |
|-----------|--------|
| Next.js 15 + App Router | ✅ |
| TypeScript strict mode | ✅ |
| Tailwind CSS + shadcn/ui | ✅ |
| Supabase SSR client | ✅ |
| HubSpot API client | ✅ |
| Scoring Dashboard (3 páginas) | ✅ |
| ScoringDocument schema (147 líneas) | ✅ |
| Claude agent (score-discovery) | ✅ |
| Autenticación de clientes | 🔴 Por implementar |
| Shortlist + feedback | 🔴 Por implementar |
| Mapas (Atlas.co) | 🔴 Por implementar |

## Información Adicional

- Stack decidido: [Stack-Decidido.md](../Stack-Decidido.md)
- UI actual (transitoria): Rube + Claude Desktop ([ADR-004](ADR-004-Rube-MCP-Bridge.md))
- Tenant Portal: [01-Modules/Tenant-Portal/README.md](../../01-Modules/Tenant-Portal/README.md)
- Mapas GIS: [ADR-017](ADR-017-Plataforma-GIS.md)
