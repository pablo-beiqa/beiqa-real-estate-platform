# Decisión de Stack Tecnológico

**Fase**: R-2 (Tecnología y Plataforma)
**Estado**: 🔴 Por investigar
**Depende de**: Data Acquisition Strategy, Competitive Landscape

---

## Objetivo

Documentar y resolver TODAS las decisiones tecnológicas abiertas. Cada decisión sigue el formato ADR (Architecture Decision Record) para que quede clara la justificación.

> **Contexto crítico**: El mayor miedo del fundador es tomar decisiones tecnológicas equivocadas que bloqueen el proyecto. Cada ADR debe ser evaluado con evidencia, no con preferencia.

---

## ADR-001: Lenguaje Backend Principal

| Opción | Scraping | Data Processing | Geospatial | API | AI/ML | Ecosystem |
|--------|----------|-----------------|------------|-----|-------|-----------|
| **Python** | Scrapy, Playwright ✅ | pandas, polars ✅ | geopandas, shapely ✅ | FastAPI ✅ | LangChain, OpenAI SDK ✅ | Más amplio para data |
| **Node.js/TypeScript** | Playwright, Puppeteer ✅ | Limitado ⚠️ | turf.js ⚠️ | Express, NestJS ✅ | LangChain.js ✅ | Más amplio para web |
| **Ambos** | Python para data | Node para API | Mixto | Node API | Python ML | Complejo, más hires |

### Preguntas de Investigación

- [ ] ¿Qué lenguaje tiene mejor soporte para las bibliotecas GIS que necesitamos?
- [ ] ¿El equipo que se va a contratar tiene más experiencia en Python o Node?
- [ ] ¿FastAPI es suficiente como framework de API, o se necesita algo más enterprise?
- [ ] ¿Un solo lenguaje simplifica contratación y mantenimiento lo suficiente para justificarlo?

### Decisión

🔴 _Por tomar_

---

## ADR-002: Framework de API

| Opción | Lenguaje | Tipo | Performance | Learning Curve | Docs autogenerados | Async |
|--------|----------|------|-------------|----------------|-------------------|-------|
| **FastAPI** | Python | REST + OpenAPI | Alta | Baja | Sí (Swagger) | Sí |
| **Express** | Node.js | REST | Alta | Baja | No (manual) | Sí |
| **NestJS** | Node.js/TS | REST + GraphQL | Alta | Media | Sí (Swagger) | Sí |
| **Django REST** | Python | REST | Media | Media | Sí | Limitado |

### Sub-decisión: REST vs GraphQL vs Ambos

| Opción | Pros | Contras | ¿Para MVP? |
|--------|------|---------|------------|
| **REST only** | Simple, estándar, fácil de debuggear | Overfetching, múltiples endpoints | ✅ Suficiente |
| **GraphQL only** | Flexibilidad, un endpoint | Complejidad, N+1, caching difícil | ⚠️ Posiblemente innecesario |
| **REST + GraphQL** | Lo mejor de ambos | Doble mantenimiento | ❌ No para MVP |

### Preguntas de Investigación

- [ ] ¿Para 6-15 usuarios internos, vale la pena la complejidad de GraphQL?
- [ ] ¿El portal de tenants necesita queries flexibles o endpoints predefinidos son suficientes?

### Decisión

🔴 _Por tomar_

---

## ADR-003: Interfaz de Usuario

> **Documento dedicado**: [Interface-Strategy.md](./Interface-Strategy.md) -- análisis completo de opciones de interfaz.

| Opción | Complejidad | Time to MVP | Skill del usuario |
|--------|-------------|-------------|-------------------|
| Web Dashboard (React/Next.js) | Alta | 2-3 meses | Medio |
| Low-code (Retool/Appsmith) | Baja | 2-4 semanas | Bajo |
| AI Chatbot (Claude/GPT) | Media | 1-2 meses | Muy bajo |
| Híbrido | Variable | Variable | Variable |

### Decisión

🔴 _Por tomar (ver Interface-Strategy.md)_

---

## ADR-004: Cloud Provider

| Opción | DB Managed | Object Storage | Serverless | Costo MVP | Complejidad | Lock-in |
|--------|-----------|----------------|------------|-----------|-------------|---------|
| **AWS** | RDS PostgreSQL | S3 | Lambda | $50-150/mes | Alta | Alto |
| **GCP** | Cloud SQL | Cloud Storage | Cloud Run | $50-150/mes | Media | Alto |
| **Azure** | Azure DB for PG | Blob Storage | Functions | $50-150/mes | Media | Alto |
| **Railway** | PostgreSQL | N/A (use S3) | N/A | $20-50/mes | Muy baja | Bajo |
| **Render** | PostgreSQL | N/A (use S3) | N/A | $25-75/mes | Muy baja | Bajo |
| **Supabase** | PostgreSQL + PostGIS | Supabase Storage | Edge Functions | $0-25/mes | Muy baja | Medio |
| **Vercel + Neon** | Neon PostgreSQL | Vercel Blob | Vercel Functions | $0-40/mes | Baja | Medio |

### Preguntas de Investigación

- [ ] ¿Supabase soporta PostGIS sin fricción?
- [ ] ¿Railway/Render pueden manejar scheduled jobs (scraping)?
- [ ] ¿Los providers simples escalan cuando el proyecto crece?
- [ ] ¿Cuál es el costo real por mes en cada opción para nuestro volumen?
- [ ] ¿Se necesita container orchestration (Docker/K8s) para MVP?

### Decisión

🔴 _Por tomar_

---

## ADR-005: Hosting de Base de Datos

> PostgreSQL + PostGIS ya está decidido como motor. La pregunta es **dónde** hostear.

| Opción | PostGIS | Costo/mes | Backups | Scaling | Complejidad |
|--------|---------|-----------|---------|---------|-------------|
| **Supabase** | Sí | $0-25 | Auto | Manual trigger | Muy baja |
| **AWS RDS** | Sí | $30-100+ | Auto | Auto | Media |
| **GCP Cloud SQL** | Sí | $30-100+ | Auto | Auto | Media |
| **Neon** | Sí | $0-19 | Auto | Auto (serverless) | Baja |
| **Railway** | Sí (¿PostGIS?) | $5-50 | Manual | Manual | Baja |
| **Self-hosted (VPS)** | Sí | $10-40 | Manual | Manual | Alta |

### Preguntas de Investigación

- [ ] ¿Supabase PostGIS tiene limitaciones vs. RDS PostGIS?
- [ ] ¿Neon soporta PostGIS con todas las extensiones?
- [ ] ¿Cuál es el storage incluido en cada tier?
- [ ] ¿Cuál es la latencia desde CDMX para cada opción?

### Decisión

🔴 _Por tomar_

---

## ADR-006: Scheduling / Orquestación

| Opción | Tipo | Complejidad | Monitoreo | Retry | Costo |
|--------|------|-------------|-----------|-------|-------|
| **Cron (sistema)** | Timer OS | Muy baja | Logs | Manual | $0 |
| **Cloud Scheduler** (GCP/AWS) | Managed cron | Baja | Dashboard | Config | $0-5/mes |
| **Celery + Redis** | Task queue | Media-Alta | Flower dashboard | Built-in | Redis cost |
| **BullMQ** (Node.js) | Job queue | Media | Bull Board | Built-in | Redis cost |
| **GitHub Actions** | CI/CD cron | Baja | GitHub UI | Config | Free tier |
| **Manual trigger** | UI button | Muy baja | In-app | N/A | $0 |
| **Híbrido** | Manual + auto | Baja | Combinado | Combinado | Variable |

### Preguntas de Investigación

- [ ] ¿El scraping necesita ser automático desde el día 1, o el equipo puede triggerearlo?
- [ ] ¿Cuántos jobs concurrentes se anticipan?
- [ ] ¿Se necesitan retries automáticos si un job falla?

### Decisión

🔴 _Por tomar_

---

## Decisiones No Documentadas (Adicionales)

### Autenticación

| Opción | Costo | Complejidad | Features |
|--------|-------|-------------|----------|
| Auth0 | Free tier + $$ | Baja | SSO, MFA, social |
| Clerk | Free tier + $$ | Baja | Modern, React-friendly |
| Supabase Auth | Incluido | Muy baja | Basic pero suficiente |
| Custom JWT | $0 | Media-Alta | Control total |

### Object Storage

| Opción | Costo | Egress | Compatible S3 |
|--------|-------|--------|---------------|
| AWS S3 | Bajo | Sí (caro) | Sí |
| Cloudflare R2 | Bajo | **No** | Sí |
| GCP Cloud Storage | Bajo | Sí | Parcial |
| Supabase Storage | Incluido | Varía | No |

### Cache (¿Redis para MVP?)

| Escenario | ¿Necesario? | Justificación |
|-----------|-------------|---------------|
| 6-15 usuarios, pocas queries | Probablemente no | PostgreSQL puede manejar el volumen |
| Rate limiting de APIs | Útil | Pero hay alternativas (in-memory) |
| Sesiones | No necesario | JWT stateless o Supabase sessions |

### Monitoreo

| Opción | Costo | Setup |
|--------|-------|-------|
| Sentry (errores) | Free tier | Fácil |
| Uptime Robot | Free | Muy fácil |
| Datadog | Caro | Completo |
| Self-hosted (Grafana) | $0 + hosting | Complejo |

### CI/CD

| Opción | Costo | Integración |
|--------|-------|-------------|
| GitHub Actions | Free tier generoso | Nativa con GitHub |
| GitLab CI | Free tier | Si se usa GitLab |

---

## Resumen de Decisiones

| ADR | Decisión | Estado | Justificación |
|-----|----------|--------|---------------|
| ADR-001 Backend Language | 🔴 | Por decidir | |
| ADR-002 API Framework | 🔴 | Por decidir | |
| ADR-003 Interface | 🔴 | Por decidir | Ver Interface-Strategy.md |
| ADR-004 Cloud Provider | 🔴 | Por decidir | |
| ADR-005 DB Hosting | 🔴 | Por decidir | |
| ADR-006 Scheduling | 🔴 | Por decidir | |
| Auth | 🔴 | Por decidir | |
| Object Storage | 🔴 | Por decidir | |
| Cache | 🔴 | Por decidir | |
| Monitoring | 🔴 | Por decidir | |
| CI/CD | 🔴 | Por decidir | |
