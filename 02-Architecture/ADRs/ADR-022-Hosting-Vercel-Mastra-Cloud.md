---
status: "aceptado"
date: 2026-03-05
decision-makers: "Pablo (CEO), Fabrizio (Tech Lead)"
consulted: "Alex (Advisor)"
informed: "Pamela, Jerónimo"
costo-mensual: "$20 (Vercel Pro) + $0 (Mastra Cloud beta, pricing TBD)"
---

# Hosting Strategy — Vercel (Frontend) + Mastra Cloud (Agents)

## Contexto y Problema

Dos componentes de la plataforma no tienen hosting de producción:

1. **Frontend (Next.js 15)**: El repo `beiqa-frontend` corre en local. Con el Tenant Portal entrando en uso real (12-15 tenants × 4-5 personas = 50-120 usuarios externos), se necesita hosting con uptime garantizado.

2. **Agentes AI (Mastra)**: El repo `beiqa-agents` aún no existe. Mastra expone un servidor HTTP (Hono) que necesita un host persistente. La decisión de hosting debe tomarse ahora para no crear deuda arquitectónica.

**Contexto de negocio**: El CEO identificó la caída de plataforma durante reuniones con clientes como "grave, perdemos deals". Esto eleva el SLA a factor de decisión crítico.

**Contexto técnico**: Todo el stack actual es SaaS/managed (Supabase, Trigger.dev, Apify, Firecrawl, Browserbase). Una decisión que rompa ese patrón introduce carga operativa inconsistente con la capacidad del equipo.

**Hallazgo**: Mastra Cloud existe, está en beta (gratis), incluye observabilidad + Studio + deployment, y es del mismo equipo que hace el framework. Mastra también tiene deployers oficiales para Vercel, Netlify, Cloudflare, DigitalOcean y AWS.

Referencias: [ADR-015](ADR-015-Frontend-Next-js.md) (Next.js, hosting pendiente), [ADR-020](ADR-020-Mastra.md) (Mastra framework), [ADR-021](ADR-021-Separacion-Trigger-Mastra.md) (Mastra expone HTTP API vía Hono).

## Factores de Decisión

* Uptime crítico: caída durante reunión con cliente = deal perdido. Requiere SLA verificable
* Filosofía SaaS: todo el stack es managed. No introducir self-managed servers
* Frontend es Next.js 15 con App Router, server components, SSR — no es sitio estático
* Mastra necesita servidor HTTP persistente (Hono) para tareas de enrichment que duran minutos
* AI workloads mixtos: real-time (chat, scoring en segundos) + batch (enrichment, reportes en minutos)
* Equipo pequeño (2 devs): minimizar carga operativa
* CI/CD: git push → deploy sin intervención manual
* Presupuesto: incremento debe ser justificado y proporcional

## Opciones Consideradas

**Para Frontend:**
* Vercel Pro
* Railway
* DigitalOcean App Platform
* Netlify

**Para Mastra Agents:**
* Mastra Cloud (beta)
* Vercel (deployer oficial de Mastra)
* Railway
* DigitalOcean App Platform
* DigitalOcean Droplet (VPS)
* Fly.io

## Decisión

Opción elegida: "**Vercel Pro para frontend + Mastra Cloud para agentes AI**", con **Vercel como fallback** para Mastra si Mastra Cloud no funciona o el pricing resulta excesivo.

### Justificación — Vercel para Frontend

- Hecho por el mismo equipo que hace Next.js — compatibilidad garantizada
- Zero-config: git push → deploy en producción con preview deployments por PR
- CDN global: tenants en CDMX reciben contenido desde nodo cercano
- 99.99% uptime SLA en Pro — responde al factor crítico de uptime
- $0 free tier para staging → $20/mo Pro para producción

### Justificación — Mastra Cloud para Agents

- Purpose-built para Mastra: incluye observabilidad (logs, tracing), Studio (UI para inspeccionar agentes), y deployment desde GitHub
- Del mismo equipo que hace Mastra — máxima compatibilidad, mínima fricción
- Gratis en beta (pricing Q1 2026)
- Si el pricing sale caro, Mastra tiene deployer nativo para Vercel ($0 extra)

### Consecuencias

* Bien, porque Vercel garantiza 99.99% SLA — mitiga riesgo de "perdemos deals"
* Bien, porque Mastra Cloud incluye tracing + evals nativos — no hay que configurar observabilidad aparte
* Bien, porque ambos servicios son 100% managed — sin carga operativa
* Bien, porque git push → deploy en ambos
* Bien, porque $20/mo incremento es <3% sobre presupuesto actual
* Mal, porque Mastra Cloud está en beta — riesgo de inestabilidad
* Mal, porque pricing de Mastra Cloud es TBD — puede no ser competitivo
* Neutral, porque el fallback (Vercel) es maduro y tiene deployer oficial de Mastra

## Plan de Implementación

* **Sistemas afectados**:
  - `beiqa-frontend` → conectar a Vercel project, configurar dominio
  - `beiqa-agents` → configurar Mastra Cloud al crear el repo
  - DNS → configurar subdominios `portal.beiqa.com`, `app.beiqa.com`
  - `beiqa-scraper` (Trigger.dev) → actualizar env var `MASTRA_URL` con URL de Mastra Cloud

* **Dependencias**:
  - Cuenta de Vercel conectada a GitHub org `pablo-beiqa`
  - Cuenta de Mastra Cloud (signup en https://cloud.mastra.ai)
  - Dominio `beiqa.com` con acceso a DNS

* **Patrones a seguir**:
  - Git push a `main` → deploy a producción
  - Git push a branch → preview deployment (Vercel)
  - Variables de entorno vía dashboard de cada plataforma (no en .env commiteado)

* **Patrones a evitar**:
  - No usar Vercel serverless para Mastra si hay tasks >5 min (usar Mastra Cloud o Railway)
  - No usar DigitalOcean Droplets (rompe filosofía SaaS)
  - No self-hostear en servidor propio

* **Configuración**:
  - Vercel: `NEXT_PUBLIC_SUPABASE_URL`, `NEXT_PUBLIC_SUPABASE_ANON_KEY`, `MASTRA_URL`
  - Mastra Cloud: `SUPABASE_URL`, `SUPABASE_SERVICE_ROLE_KEY`, `GOOGLE_MAPS_API_KEY`, `[LLM API KEYS]`
  - Trigger.dev: `MASTRA_URL` (URL de Mastra Cloud)

### Verificación

- [ ] `beiqa-frontend` deployado en Vercel, accesible en dominio configurado
- [ ] Preview deployments activos por PR en `beiqa-frontend`
- [ ] `beiqa-agents` deployado en Mastra Cloud, Hono server respondiendo en `/health`
- [ ] Trigger.dev enviando HTTP POST a URL de Mastra Cloud post-scrape
- [ ] Frontend consumiendo Mastra API (scoring) desde URL de Mastra Cloud
- [ ] Variables de entorno configuradas (sin .env commiteado)

## Pros y Contras por Opción

### Frontend: Vercel Pro ($20/mo)

* Bien, porque zero-config para Next.js, CDN global, preview deployments, 99.99% SLA
* Mal, porque $20/mo vs $0 free tier (pero free tier no tiene SLA)

### Frontend: Railway ($5-20/mo)

* Bien, porque simplicidad general
* Mal, porque no optimizado para Next.js (SSR, edge functions, image optimization son ciudadanos de segunda clase)

### Frontend: DigitalOcean App Platform ($5-25/mo)

* Bien, porque flexible
* Mal, porque más configuración que Vercel para el mismo resultado

### Frontend: Netlify

* Bien, porque bueno para sitios estáticos
* Mal, porque soporte parcial para Next.js con server components

### Agents: Mastra Cloud (beta, $0 hoy)

* Bien, porque purpose-built para Mastra, incluye tracing + evals + Studio
* Bien, porque del mismo equipo — máxima compatibilidad
* Mal, porque beta — riesgo de inestabilidad
* Mal, porque pricing TBD

### Agents: Vercel (deployer oficial, $0 extra)

* Bien, porque maduro, ya está en el stack
* Mal, porque serverless timeout (300s Pro) — insuficiente para enrichment batch largo

### Agents: Railway ($5-20/mo)

* Bien, porque simple para servidores persistentes
* Mal, porque NO está en lista oficial de cloud providers de Mastra

### Agents: DigitalOcean Droplet ($5-12/mo)

* Bien, porque lo más barato
* Mal, porque self-managed — requiere OS updates, security patches. Inconsistente con filosofía SaaS

### Agents: DigitalOcean App Platform ($5-25/mo)

* Bien, porque está en lista oficial de providers de Mastra
* Mal, porque más configuración que Mastra Cloud para el mismo resultado

### Agents: Fly.io ($5-15/mo)

* Bien, porque buena latencia edge
* Mal, porque requiere Docker, curva de aprendizaje mayor. No está en lista oficial de Mastra

## Información Adicional

- ADR relacionados: [ADR-015](ADR-015-Frontend-Next-js.md) (hosting resuelto), [ADR-020](ADR-020-Mastra.md) (Mastra hosting resuelto), [ADR-018](ADR-018-CI-CD.md) (CI/CD frontend resuelto por Vercel)
- Esta decisión resuelve la sección "Deploy y hosting" pendiente en ADR-015
- **Condiciones para revisitar**: si Mastra Cloud pricing resulta >$50/mo; si Mastra Cloud presenta problemas de estabilidad; si el equipo necesita un PaaS unificado
- **Fallback documentado**: Si Mastra Cloud no funciona → deploy a Vercel usando deployer oficial de Mastra
- Vercel pricing: https://vercel.com/pricing
- Mastra Cloud: https://cloud.mastra.ai
- Mastra deployment docs: https://mastra.ai/docs/deployment/cloud-providers
