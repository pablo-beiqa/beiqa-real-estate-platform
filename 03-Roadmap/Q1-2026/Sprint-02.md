# Sprint 2 (Mar 16-29) — STRETCH

> **Estado**: Planificado
> **Quarter**: Q1-2026
> **Milestones**: Scrapers Consolidados (cierre), Enrichment Agent Operativo (continua), Portal Autenticado (inicio)

---

## OKRs

| Objetivo | Key Result | Owner |
|----------|------------|-------|
| O1: Primer agente AI funcional | KR1: Address Enrichment Agent procesa 1 propiedad E2E | Pablo |
| O1 | KR2: ≥2 modelos LLM evaluados con metricas | Pablo |
| O2: Golden record schema creado | KR3: 5 tablas de golden record con migrations | Fabrizio |
| O3: Auth funcional en portal | KR4: Login magic link funcional + middleware | Pablo |

## Definition of Done

- [ ] Address Enrichment Agent procesa 1 prop E2E (geocode + validate + confidence)
- [ ] ≥2 modelos LLM evaluados con tabla costo/calidad/velocidad
- [ ] Golden record schema creado (properties, property_sources, agent_runs, enrichment_queue, cache_api_responses)
- [ ] Login magic link funcional en beiqa-frontend
- [ ] Middleware protege rutas autenticadas

---

## Deliverables por Track

### Track: AI / Mastra (Pablo)

| Deliverable | Issue | AC | Estado |
|-------------|-------|----|--------|
| Address Enrichment Agent v1 | [#87](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/87) | 1 prop I24 → direccion corregida + confidence (0-100). Google Geocoding como tool. | Pendiente |
| Evaluacion modelos LLM | [#88](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/88) | ≥2 modelos en 10 props. Tabla costo/calidad/velocidad. Decision documentada. | Pendiente |
| Cost estimation AI/LLM 40K+ props | NUEVO | Estimacion de costo para procesar 40K+ props con geocoding + LLM. | Pendiente |
| Coordinate Validator tool | [#89](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/89) | Valida coords en CDMX/EdoMex/Morelos/Puebla. PostGIS. | Pendiente |

### Track: Scraper / Infra (Fabrizio)

| Deliverable | Issue | AC | Estado |
|-------------|-------|----|--------|
| Golden record: migrations (5 tablas) | [#104](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/104) | properties, property_sources, agent_runs, enrichment_queue, cache_api_responses. RLS. | Pendiente |
| Campos comunes golden record | [#105](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/105) | Schema unificado para 8+ portales. Confidence + needs_review. | Pendiente |
| Check de existencia | [#15](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/15) | Update si existe. Marcar unavailable si desaparece. Validacion CBRE/Colliers. | Pendiente |
| Columnas enrichment en I24 | [#106](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/106) | enrichment_status, confidence, address_corrected, golden_record_id en inmuebles24_listings. | Pendiente |

### Track: Frontend / Tenant Portal (Pablo)

| Deliverable | Issue | AC | Estado |
|-------------|-------|----|--------|
| Configurar Supabase Auth | [#47](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/47) | @supabase/ssr configurado. Redirect URLs. Smoke test. | Pendiente |
| Pagina de login (magic link) | [#50](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/50) | Card + logo. Magic link + password fallback. Responsive. | Pendiente |
| Middleware proteccion rutas | [#51](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues/51) | middleware.ts valida getUser(). Session refresh. Redirect /login. | Pendiente |

### Track: Design (Pamela)

| Deliverable | Issue | AC | Estado |
|-------------|-------|----|--------|
| Design system en Figma (continua) | — | Componentes para scoring, shortlists, feedback. | En progreso |

---

## Riesgos y Dependencias

| Riesgo / Dependencia | Impacto | Mitigacion |
|---------------------|---------|------------|
| Sprint STRETCH: 11 deliverables para 2 devs | Alto | Lo que no cierre pasa a Sprint 3 sin penalizacion |
| Golden record schema bloquea normalization (Sprint 3) | Alto | Priorizar #104 y #105 al inicio del sprint |
| LLM evaluation puede requerir mas tiempo | Medio | Limitar a 2 modelos, 10 props. No necesita ser exhaustivo. |
| Auth depende de Supabase Auth config correcta | Bajo | Documentacion de Supabase es buena. Auth-Strategy.md ya existe. |

---

## Review (Mar 29)

> Completar al cierre del sprint.

### Metricas de Cierre

| Metrica | Target | Resultado |
|---------|--------|-----------|
| Issues cerrados | 8-11 | ... |
| KRs completados | 4/4 | ... |

### Retrospectiva

- **Bien**: ...
- **Mejorar**: ...
- **Accion**: ...
