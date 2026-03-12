# Sprint 2 (Mar 16-29) — Fundaciones + Experimentación

> **Estado**: Planificado
> **Quarter**: Q1-2026
> **Tema**: Cimentar pipeline de datos (golden record + scrapers nuevos) y lanzar experimentación de enrichment multimodal con AI
> **Milestones**: Scrapers Consolidados (cierre), Enrichment Agent Operativo (inicio), Portal Autenticado (inicio)

---

## OKRs

| Objetivo | Key Result | Owner |
|----------|------------|-------|
| **O1: Pipeline de Datos Escalable** | KR1: Golden Record schema creado en Supabase con primeros datos ingested de ≥1 portal | Fabrizio |
| O1 | KR2: 2 scrapers nuevos (Cushman + Proximity Parks) en producción y validados | Pablo (dev) + Fabrizio (test/deploy) |
| O1 | KR3: Framework de testing de scrapers con reportes de primera corrida | Fabrizio |
| **O2: Foundations de AI + Experimentación** | KR4: `beiqa-agents` repo inicializado, `mastra dev` funcionando, estructura escalable | Pablo |
| O2 | KR5: Primeros experiments de enrichment documentados (≥2 LLMs × ≥2 señales incluyendo fotos, 10 props) | Pablo |
| O2 | KR6: Pablo + Fabrizio completaron onboarding de Mastra | Pablo + Fabrizio |
| O2 | KR7: Estimación de costos AI/LLM documentada + modelo monetario actualizado | Pablo |
| **O3: Portal Autenticado** | KR8: Login con magic link funcional en beiqa-frontend | Pablo (UI) + Fabrizio (config) |
| O3 | KR9: Middleware protegiendo rutas autenticadas | Pablo |

### Actividades continuas (se evalúan al cierre, no son deliverables formales)

- **Mastra learning** — Pablo + Fabrizio estudian framework (videos, docs, hands-on)
- **GIS learning** — Educación general sobre H3, PostGIS, AGEB (background)
- **Repo docs** — Actualización de este repositorio cada 2-3 días conforme se avanza

---

## Definition of Done

- [ ] ADR-023 (multi-unit buildings) documentado y aceptado
- [ ] Golden Record schema (≥5 tablas) creado con migrations + RLS + primeros datos ingested
- [ ] Scrapers Cushman + Proximity Parks en producción con staging tables, H3, triggers
- [ ] Framework de testing de scrapers funcionando con reportes de ≥1 corrida
- [ ] I24 validado: scraper corriendo correctamente con resultados documentados
- [ ] TriggerDev en plan de pago, jobs deployed a la nube
- [ ] `beiqa-agents` repo: `mastra dev` arranca, estructura agents/tools/workflows
- [ ] Enrichment experiments: tabla con resultados de ≥2 LLMs × ≥2 señales (texto + fotos)
- [ ] Estimación de costos AI/LLM para 40K+ props documentada
- [ ] Login magic link funcional + middleware protegiendo rutas
- [ ] Todo commiteado y pusheado

---

## Deliverables por Track

### Track: AI / Mastra (Pablo)

| Deliverable | Issue | AC | Estado |
|-------------|-------|----|--------|
| Inicializar beiqa-agents + mastra dev | [#86](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/86) (carryover S1) | `mastra dev` arranca. Estructura agents/tools/workflows. README + .env.example. TypeScript. | Pendiente |
| Enrichment experiments multimodal | [#87](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/87) + [#88](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/88) | Test ≥2 LLMs (ej: Claude Sonnet, GPT-4o) × ≥2 señales (texto descripción + fotos). 10 props reales. Tabla resultados: accuracy, confidence, costo, velocidad. | Pendiente |
| Estimación costos AI/LLM + modelo monetario | [#130](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/130) | Basado en experiments, extrapolar costo para 40K+ props. Actualizar Total-Budget.md con modelo. | Pendiente |

### Track: Scraper (Pablo dev + Fabrizio test/deploy)

| Deliverable | Issue | Owner | AC | Estado |
|-------------|-------|-------|----|--------|
| Desarrollar scraper Cushman | [#36](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/36) (mover S5+→S2) | Pablo | Scraper funcional basado en template existente. Extrae listings de Cushman. | Pendiente |
| Desarrollar scraper Proximity Parks | [#111](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/111) (mover S5+→S2) | Pablo | Scraper funcional basado en template existente. Extrae listings de Proximity Parks. | Pendiente |
| Test + deploy Cushman a producción | Parte de [#36](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/36) | Fabrizio | Staging table creada, H3 indexing, triggers, Supabase integration validada, TriggerDev job corriendo. | Pendiente |
| Test + deploy Proximity Parks a producción | Parte de [#111](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/111) | Fabrizio | Staging table creada, H3 indexing, triggers, Supabase integration validada, TriggerDev job corriendo. | Pendiente |

### Track: Data / Infra (Fabrizio)

| Deliverable | Issue | AC | Estado |
|-------------|-------|----|--------|
| ADR-023: Multi-unit buildings | NUEVO | Decisión documentada: edificios multi-piso, misma propiedad cross-portal, renta+venta simultánea. Formato MADR. | Pendiente |
| Golden Record schema + primeros datos | [#104](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/104) + [#105](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/105) | Schema ≥5 tablas basado en ADR-023. Migrations aplicadas. Primeros datos ingested de ≥1 portal. RLS. | Pendiente |
| Framework testing scrapers + reportes | NUEVO | Framework que documenta: # éxitos, # errores, gestión de casos, datos validados por portal. Reportes de primera corrida. | Pendiente |
| Pruebas I24 | NUEVO | Verificar que scraper I24 (Apify) funciona correctamente. Documentar resultados, errores, datos validados en Supabase. | Pendiente |
| TriggerDev infra: paid plan + cloud deploy | NUEVO | Plan de pago activado. Todos los jobs deployed a la nube. Todo corriendo en producción. | Pendiente |
| Configurar Supabase Auth | [#47](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/47) | @supabase/ssr configurado en beiqa-frontend. Redirect URLs. Email provider para magic links. Smoke test. | Pendiente |

### Track: Frontend / Tenant Portal (Pablo)

| Deliverable | Issue | AC | Estado |
|-------------|-------|----|--------|
| Pagina de login (magic link) | [#50](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/50) | Card + logo placeholder. Magic link + password fallback. Responsive. UI genérica (shadcn/ui). | Pendiente |
| Middleware proteccion rutas | [#51](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/51) | middleware.ts valida getUser(). Session refresh. Redirect /login si no autenticado. /login redirect a / si ya autenticado. | Pendiente |

### Track: Design (Pamela)

| Deliverable | Issue | AC | Estado |
|-------------|-------|----|--------|
| Design system en Figma (continua) | — | Componentes para scoring, shortlists, feedback. Base para implementación S3+. | En progreso |

---

## Dependencias y Orden (Critical Path)

```
Semana 1 (Mar 16-22):
  Pablo:  Scrapers Cushman + Proximity (1-2d) → Mastra init (#86, 1d) → Experiments comienzan
          Login page (#50, 1d)
  Fabrizio: ADR-023 multi-unit (2d) → Golden Record design comienza
            TriggerDev infra paid plan (1d)
            Pruebas I24 (1d)
            Supabase Auth config (#47, 0.5d)

Semana 2 (Mar 23-29):
  Pablo:  Experiments continúan + documentar → Cost estimation (#130)
          Middleware (#51, 0.5d)
  Fabrizio: Golden Record migrations + first data ingest
            Test/deploy Cushman + Proximity Parks
            Testing framework + reportes
```

**Dependencia crítica**: ADR-023 DEBE aceptarse antes de que golden record migrations se finalicen. Target: ADR aceptado para miércoles Mar 18.

---

## Riesgos y Dependencias

| Riesgo / Dependencia | Impacto | Mitigación |
|---------------------|---------|------------|
| ADR multi-unit buildings genera debate largo | Alto | Timebox a 2 días. Si no hay consenso para Mar 18, usar modelo más simple e iterar. |
| LLM experiments open-ended: fácil perderse experimentando | Medio | Timebox 4 días. Máximo 10 props por combinación. Documentar resultados incrementalmente. |
| Pablo split 4 tracks: scrapers + AI + frontend + planning | Alto | Scrapers primero (1-2d, copy+paste template). Frontend (1.5d). Resto = AI experiments. |
| Golden record schema requiere iteraciones | Medio | Fabrizio comienza draft paralelo al ADR. ADR valida, luego migrations finalizan. |
| Fotos de propiedades: costo multimodal puede ser alto | Medio | Experiments cuantifican costo (#130). Si es caro, probar vision models solo en props con baja confianza textual. |
| TriggerDev paid plan: posible cambio de pricing | Bajo | Evaluar antes de activar. Documentar costo mensual esperado. |

---

## Issues movidos fuera de Sprint 2

| Issue | Titulo | Destino | Razón |
|-------|--------|---------|-------|
| [#15](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/15) | Existence check module | S1 / Completado | Ya implementado en Sprint 1 |
| [#89](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/89) | Coordinate Validator tool | Backlog | No prioritario, puede integrarse en enrichment experiments |
| [#106](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/106) | Enrichment columns en I24 | Backlog | No hace sentido sin pipeline funcional |
| [#125](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/125) | I24 migration tracking | S3 | Migration completa es tarea de S3 |
| [#27](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/27) | Replicar lógica Clay en TriggerDev | S3 | Parte de la migración I24 |
| [#29](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/29) | Testing Clay vs TriggerDev | S3 | Parte de la migración I24 |
| [#30](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/30) | Migración definitiva Clay | S3 | Parte de la migración I24 |

---

## Issues nuevos a crear

| Titulo | Labels | Sprint | Owner | Descripción |
|--------|--------|--------|-------|-------------|
| ADR-023: Multi-unit buildings data model | `docs`, `data` | S2 | Fabrizio | Cómo modelar edificios multi-piso, misma propiedad cross-portal, renta+venta |
| Framework testing scrapers | `scraper`, `testing` | S2 | Fabrizio | Framework de reportes: éxitos, errores, gestión de casos |
| Pruebas I24 — verificar funcionamiento | `scraper`, `testing` | S2 | Fabrizio | Validar que I24 Apify funciona correctamente, documentar resultados |
| TriggerDev infra: paid plan + cloud deploy | `infra`, `triggerdev` | S2 | Fabrizio | Activar plan de pago, deploy todos los jobs a la nube |

---

## Review (Mar 29)

> Completar al cierre del sprint.

### Métricas de Cierre

| Métrica | Target | Resultado |
|---------|--------|-----------|
| Issues cerrados | 10-15 | ... |
| KRs completados | 9/9 | ... |
| ADRs written | 1 (ADR-023) | ... |
| LLM models tested | ≥2 | ... |
| Signal types tested | ≥2 (incl. fotos) | ... |
| Scrapers nuevos en producción | 2 | ... |

### Retrospectiva

- **Bien**: ...
- **Mejorar**: ...
- **Acción**: ...
