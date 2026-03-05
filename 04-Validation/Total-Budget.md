# Presupuesto Operativo — Plataforma Beiqa

> **Fecha**: Marzo 2026 | **Moneda**: USD (TC: 17.5 MXN/USD)
>
> Este documento refleja los costos reales verificados de operar la plataforma. Solo incluye servicios e infraestructura — no incluye salarios del equipo, HubSpot (costo general de Beiqa), ni Clay (herramienta comercial independiente).

---

## Resumen ejecutivo

| Métrica | Valor |
|---------|-------|
| **Costo mensual actual** | **$747 – $896 USD** |
| **Costo mensual con Mastra** | **~$814 – $984 USD** |
| Costos fijos | $329 – $443 |
| Costos variables | $418 – $453 |
| Mastra LLM costs (nuevo) | ~$67 – $88 (estimado, TBD) |
| Proyección 6 meses (con Mastra) | $842 – $1,028 |
| Proyección 12 meses (con Mastra) | $922 – $1,288 |

**Distribución por categoría:**

| Categoría | USD/mes | % del total |
|-----------|---------|-------------|
| Extracción (scraping) | $403 – $423 | 47 – 54% |
| GIS / Mapas | $178 – $267 | 20 – 30% |
| AI Workspace (Rube + Claude Desktop) | $75 – $100 | 10 – 11% |
| Orquestación (Trigger.dev) | $50 | 6 – 7% |
| AI Processing (OpenRouter, legacy) | $15 – $30 | 2 – 3% |
| AI Agents — Mastra LLM (nuevo) | ~$67 – $88 | 7 – 10% |
| Infraestructura (Supabase + dominio) | $26 | 3% |

---

## 1. Costos fijos mensuales (~$329 – $443)

Estos costos no cambian con el volumen de propiedades extraídas.

| Servicio | USD/mes | Función | Usuarios/detalle |
|----------|---------|---------|------------------|
| Atlas.co | $178 – $267 | Mapas, GIS, análisis geoespacial con AI | 2–3 usuarios × $89 |
| Claude Desktop | $50 – $75 | AI workspace para consultas del equipo | 2–3 usuarios × $25 |
| Trigger.dev | $50 | Orquestación de scrapers, cron, batch AI, sync | Plan pagado |
| Rube | $25 | MCP bridge para conectar Claude Desktop con Supabase | 1 suscripción |
| Supabase Pro | $25 | PostgreSQL + PostGIS + Auth + Storage + REST API | 1 proyecto |
| Dominio | ~$1.25 | beiqa.com | $15/año |

---

## 2. Costos variables mensuales (~$418 – $453)

Estos costos escalan con el volumen de extracciones y procesamiento.

| Servicio | USD/mes | Driver de costo | Detalle |
|----------|---------|-----------------|---------|
| Apify (I24) | $300 | $150/corrida × 2 corridas/mes | 25–30K propiedades por corrida, frecuencia quincenal |
| Firecrawl | ~$103 | Plan fijo $1,800 MXN | Cubre Pincali + CBRE + Colliers. 100K páginas/mes |
| OpenRouter (GPT-4o-mini) | $15 – $30 | ~12K propiedades nuevas/mes | AI extraction de campos, 1 llamada/propiedad nueva |
| Google Maps Platform | $0 | Dentro de crédito $200/mes | Geocoding + Places API, ~500–1,000 requests/mes |
| Browserbase | $0 – $20 | Sesiones browser para edge cases | ⚠️ Pendiente verificar con Fabrizio |

---

## 3. Costo por portal (unit economics de extracción)

| Portal | Herramienta | Props/corrida | Frecuencia | Costo/mes | Costo/propiedad extraída |
|--------|-------------|---------------|------------|-----------|--------------------------|
| Inmuebles24 | Apify | 25–30K | Quincenal | $300 | ~$0.005 – $0.006 |
| Pincali | Firecrawl | 20–30K | Quincenal | ~$103* | ~$0.002 – $0.003 |
| CBRE | Firecrawl | 200–300 | Semanal | Incluido* | ~$0.08 – $0.13 |
| Colliers | Firecrawl | 200–300 | Semanal | Incluido* | ~$0.08 – $0.13 |

*Firecrawl $103 USD/mes ($1,800 MXN) cubre los 3 portales.

**Observación**: CBRE y Colliers tienen un costo por propiedad ~15–25x mayor que I24/Pincali debido al bajo volumen. Esto es normal — el valor por propiedad de brokers premium es proporcionalmente mayor.

---

## 4. Costo por propiedad procesada

El costo total de incorporar una propiedad a la plataforma depende de si es nueva o actualización.

### Propiedad NUEVA (~10–20% por corrida)

| Paso | Costo | Notas |
|------|-------|-------|
| Extracción | $0.002 – $0.006 | Varía por portal (ver tabla anterior) |
| AI processing (OpenRouter) | ~$0.001 – $0.003 | GPT-4o-mini, 1 llamada para extraer/normalizar campos |
| Geocoding | $0 | Google Maps crédito o alternativa OSM (en evaluación) |
| Domain lookup | ~$0 | Búsqueda de broker vía Google |
| **Total propiedad nueva** | **~$0.003 – $0.009** | |

### Propiedad UPDATE (~80–90% por corrida)

| Paso | Costo | Notas |
|------|-------|-------|
| Extracción | $0.002 – $0.006 | Mismo costo que nueva |
| AI processing | $0 | No se reprocesa con AI |
| **Total update** | **~$0.002 – $0.006** | |

**Nota sobre brokers conocidos (CBRE, Colliers)**: Al ser fuentes con datos estructurados, se omite parte del procesamiento AI — los campos ya vienen limpios.

---

## 5. Proyecciones 6–12 meses

### Costos nuevos proyectados

| Concepto | Cuándo | Costo inicial | Costo a 12 meses | Notas |
|----------|--------|---------------|-------------------|-------|
| Supabase Storage (imágenes) | Al implementar descarga | ~$2/mes | ~$5–10/mes | 5–10 fotos/propiedad, ~150–400GB acumulados |
| Supabase Bandwidth (egress) | Con 50–200 usuarios | ~$5/mes | ~$20–50/mes | Usuarios viendo imágenes en la app |
| Vercel Pro (frontend) | Fase 2 | $20/mes | $20/mes | Hosting Next.js, recomendado para el stack |
| Email transaccional | Tenant Portal | $0–5/mes | $10–20/mes | Resend o SendGrid para notificaciones |
| Portales adicionales (1–2) | 6–12 meses | $50–200/mes | $50–200/mes | Mix de portales grandes y pequeños |
| Storage PDFs (CBRE/Colliers) | Al implementar descarga | ~$1/mes | ~$2–5/mes | ~500 props × 15MB promedio |

### Escenario conservador — 6 meses

| Categoría | USD/mes |
|-----------|---------|
| Costos actuales | $747 – $896 |
| + Storage (imágenes + PDFs) | +$3 – $5 |
| + Supabase Bandwidth | +$5 – $15 |
| + Vercel Pro | +$20 |
| + Email | +$0 – $5 |
| **Total proyectado** | **~$775 – $940** |

### Escenario de crecimiento — 12 meses

| Categoría | USD/mes |
|-----------|---------|
| Costos actuales | $747 – $896 |
| + Storage acumulado | +$7 – $15 |
| + Supabase Bandwidth (50–200 users) | +$20 – $50 |
| + Vercel Pro | +$20 |
| + Email | +$10 – $20 |
| + 1–2 portales adicionales | +$50 – $200 |
| **Total proyectado** | **~$855 – $1,200** |

### Costo operativo anualizado

| Escenario | Mes actual | Anualizado |
|-----------|-----------|------------|
| Actual (hoy) | $747 – $896 | $8,964 – $10,752 |
| 6 meses | $775 – $940 | $9,300 – $11,280 |
| 12 meses | $855 – $1,200 | $10,260 – $14,400 |

---

## 6. Puntos de inflexión

Eventos que causan saltos en costo:

| Trigger | Qué pasa | Impacto estimado |
|---------|----------|------------------|
| >100GB en Supabase Storage | Cobro por excedente ($0.021/GB) | +$1–5/mes por cada 100GB |
| >250GB bandwidth/mes | Cobro por excedente ($0.09/GB) | +$5–20/mes por cada 100GB |
| >100K páginas Firecrawl | Necesitan upgrade de plan | +$50–100/mes |
| Nuevo portal grande (>10K props) | Nuevo actor Apify o más créditos Firecrawl | +$100–200/mes |
| Google Maps excede crédito $200 | Batch geocoding de muchos nuevos o usuarios en mapas | Cobros reales comienzan |
| Trigger.dev excede plan actual | Más corridas, más tasks concurrentes | +$50–100/mes al siguiente tier |
| >50 usuarios en Tenant Portal | Supabase Auth: 50K MAU incluidos en Pro | $0 adicional (amplio margen) |

---

## 7. Exclusiones

Costos que **no** se incluyen en este presupuesto:

| Concepto | Razón de exclusión |
|----------|-------------------|
| Salarios (Fabrizio, Pamela) | Costo de nómina del equipo, no de la plataforma |
| HubSpot CRM | Costo general de Beiqa, preexistente a la plataforma |
| Clay | Herramienta comercial independiente de este proyecto |
| Costo de oportunidad | Horas de mantenimiento vs. desarrollo — fuera del alcance |

---

## 8. Costos de Mastra / AI Agents (nuevo)

> **Estado**: Estimación preliminar — costos reales dependen de modelos LLM seleccionados (TBD, requiere testing).

| Concepto | Costo estimado/mes | Notas |
|----------|-------------------|-------|
| Address Enrichment (LLM) | ~$36 | ~60K propiedades × ~$0.0006/propiedad. Modelo TBD. |
| Data Normalization (LLM) | ~$15–20 | Feature extraction de descripciones. Modelo TBD. |
| Deduplication (LLM) | ~$1–2 | Solo para casos ambiguos (score 0.70–0.95). |
| Scoring / Matching | ~$5–10 | On-demand, ~50–100 scorings/mes. |
| Market Intelligence | ~$3–5 | Generación de reportes por zona. |
| GIS Analysis | ~$5–10 | Análisis contextual con LLM. |
| **Total LLM mensual ongoing** | **~$67–88** | |
| **Backfill one-time** | **~$180** | Procesar 60K propiedades existentes. Se paga una vez. |

**Nota**: El framework Mastra es $0 (open source, Apache 2.0). Los costos son exclusivamente de APIs de modelos LLM. Conforme se evalúen modelos (Claude Haiku, GPT-4o-mini, Llama, etc.), estos números se ajustarán.

**Nota sobre OpenRouter**: Con Mastra, la extracción AI que hoy hace OpenRouter ($15–30/mo) se absorbe en los costos de agentes. El costo neto incremental de Mastra es la diferencia: ~$37–58/mo adicionales vs el costo actual de OpenRouter.

---

## 9. Pendientes de verificar

- [ ] **Browserbase**: Confirmar con Fabrizio si se usa activamente y cuánto cuesta (estimado $0–20)
- [ ] **OpenRouter**: Obtener billing real del último mes (estimación actual: $15–30 basada en Pablo). Se absorbe en Mastra.
- [ ] **Atlas.co**: Confirmar número exacto de usuarios (2 o 3 → rango $178–$267)
- [ ] **Google Maps**: Revisar consumo real del crédito en Google Cloud Console
- [ ] **Geocoding**: Decisión pendiente — Google Maps Platform vs. OpenStreetMap/Nominatim para I24/Pincali
- [ ] **Mastra LLM costs**: Costos reales dependen de modelos seleccionados. Evaluar después de Sprint 1.
- [ ] **Backfill cost**: Costo one-time de ~$180 — verificar después de procesar primeras 10K propiedades

---

## Nota sobre el presupuesto anterior

Este documento reemplaza completamente la versión anterior que contenía:
- Estimaciones de outsource ($23K–$56K) que ya no aplican — el desarrollo es interno
- Costo de herramientas a $30–50/mes — subestimación de ~15x respecto a la realidad ($747–$896)
- n8n Cloud — deprecado y migrado a Trigger.dev ([ADR-019](../02-Architecture/ADRs/ADR-019-n8n-Deprecado.md))
- Escenarios de infraestructura hipotéticos (AWS, Neon) — se eligió Supabase

---

*Última actualización: 2026-03-05 | Costos de Mastra/AI Agents agregados (sección 8). Ver [ADR-020](../02-Architecture/ADRs/ADR-020-Mastra.md).*
