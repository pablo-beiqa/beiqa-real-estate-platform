---
status: "propuesto"
date: 2026-02-27
decision-makers: "Pablo (CEO), Fabrizio (Tech Lead)"
costo-mensual: "por definir"
---

# Estrategia CI/CD — Integración y Despliegue Continuo

## Contexto y Problema

Actualmente los deploys son manuales. Con el frontend activo (Fase 2) y el scraper en producción (beiqa-scraper), se necesita un pipeline de CI/CD que automatice: lint, tests, build, y deploy. ¿Qué plataforma y estrategia usar?

## Factores de Decisión

* Repos: beiqa-real-estate-platform (docs), beiqa-scraper (Trigger.dev), futuro(s) repo(s) frontend
* GitHub como plataforma de código — integración nativa con GitHub Actions
* Trigger.dev tiene su propio mecanismo de deploy (CLI `trigger deploy`)
* Frontend probablemente en Vercel (integración natural con Next.js)
* Equipo pequeño (2-3 devs) — la simplicidad es prioridad
* Presupuesto limitado — preferir opciones con free tier generoso

## Opciones a Evaluar

### GitHub Actions
* Nativo de GitHub, YAML-based, 2,000 min/mes gratis
* Pros: integración directa con repos, marketplace de actions, $0 para el volumen actual
* Contras: YAML verboso, debugging puede ser tedioso

### Vercel (solo frontend)
* Deploy automático en push, preview deployments por PR
* Pros: zero-config para Next.js, preview URLs, $0 en Hobby tier
* Contras: solo cubre frontend, no el scraper ni otros servicios

### GitHub Actions + Vercel (combinación)
* GitHub Actions para CI (lint, tests) + Vercel para deploy del frontend
* Pros: cada herramienta hace lo que mejor sabe, free tiers combinados
* Contras: dos sistemas a mantener

### Railway / Render
* PaaS con deploy automático desde GitHub
* Pros: simple, auto-deploy en push
* Contras: costo mensual fijo, menos control que GitHub Actions

## Decisión

**Pendiente.** Se definirá cuando haya un frontend activo que deployar (Fase 2). GitHub Actions es el candidato natural para CI. Para deploy del frontend, Vercel es la opción obvia con Next.js.

## Información Adicional

- Repos actuales: `beiqa-real-estate-platform` (docs), `beiqa-scraper` (Trigger.dev)
- Frontend futuro: [ADR-015](ADR-015-Frontend-Next-js.md)
- Deploy actual de scrapers: `trigger deploy` (CLI de Trigger.dev)
- Condición para activar: cuando haya código deployable más allá de Trigger.dev
