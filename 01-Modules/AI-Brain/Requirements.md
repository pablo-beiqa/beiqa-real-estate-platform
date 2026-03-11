# Requerimientos: AI Brain

**Estado**: 🟡 En diseno detallado

> **Prioridades**: **MUST** = Imprescindible | **SHOULD** = Importante pero no bloqueante | **COULD** = Deseable si hay tiempo

---

## Tier 1: Data Pipeline (Background, Automatizado)

### Address Enrichment Agent (P0)

- **MUST** Validacion multi-senal de direcciones
  - [ ] Google Geocoding API
  - [ ] Reverse geocoding
  - [ ] Extraccion de direccion desde descripcion via LLM
  - [ ] Validacion de coordenadas via PostGIS
- **MUST** Generar confidence score 0-100 para cada direccion corregida
- **MUST** Persistir direcciones corregidas a Supabase (`corrected_address`, `corrected_lat`/`corrected_lng`, `confidence_score`, `signals_used`)
- **MUST** Escalacion HITL para confidence <50 via review queue + notificacion a Slack
- **MUST** Procesar las 5 staging tables:
  - [ ] `inmuebles24_listings`
  - [ ] `pincali_listings`
  - [ ] `finsa_listings`
  - [ ] `cbre_listings`
  - [ ] `colliers_listings`
- **SHOULD** Aprender de correcciones del equipo via System Memory (Layer 3)
- **SHOULD** Procesar batch de nuevas propiedades en <60 min post-scrape
- **SHOULD** >80% de accuracy target vs verificacion manual

### Data Normalization Agent (P0)

- **MUST** Mapear datos de 5 staging tables al golden record unificado (tabla `properties`)
- **MUST** Normalizar moneda (MXN/USD), unidades (m2/ft2) y tipos de propiedad a catalogo estandar
- **MUST** Extraer features desde descripciones via LLM (amenidades, condiciones, especificaciones)
- **MUST** Validar rangos de datos (precio, superficie, coordenadas) antes de escribir
- **MUST** Upsert a golden record + mantener tabla `property_sources` de vinculacion
- **SHOULD** Extraccion fallback de PDFs cuando los datos del scraper estan incompletos (flyers FinSA, fichas CBRE/Colliers)
- **SHOULD** Escalacion HITL para datos contradictorios entre fuentes
- **SHOULD** >95% de campos correctamente mapeados target

### Deduplication Agent (P1)

- **MUST** Detectar duplicados cross-portal usando algoritmo hibrido (blocking + scoring + LLM)
- **MUST** Blocking por H3 res 9 para agrupar pares candidatos
- **MUST** Scoring deterministico con 6 senales:
  - [ ] Address fuzzy matching
  - [ ] Distancia de coordenadas
  - [ ] Superficie +-10%
  - [ ] Type match
  - [ ] Precio +-15%
  - [ ] Broker
- **MUST** Auto-merge >0.95, no match <0.70, resolucion LLM 0.70-0.95
- **MUST** Merge de la mejor informacion de cada fuente al golden record
- **SHOULD** Escalacion HITL para casos que el LLM no puede resolver con alta confianza
- **SHOULD** Aprender de pares confirmados/rechazados via System Memory
- **SHOULD** >85% precision, <2% falsos positivos

---

## Tier 2: Client Intelligence (Interactivo + Autonomo)

### Score Discovery Agent (P1)

- **MUST** Procesar transcripts de Circleback para extraer requerimientos del cliente
- **MUST** Soportar input manual del equipo para criterios no mencionados en llamadas
- **MUST** Mapear requerimientos a 7 grupos de criterios (A-G, 160+ totales):
  - [ ] Grupo A — Universal (~30 criterios): siempre activo
  - [ ] Grupo B — Industrial (~35 criterios): activado por tipo de propiedad
  - [ ] Grupo C — Comercial (~30 criterios): activado por tipo de propiedad
  - [ ] Grupo D — Oficinas (~20 criterios): activado por tipo de propiedad
  - [ ] Grupo E — Terrenos (~15 criterios): activado por tipo de propiedad
  - [ ] Grupo F — Operacional (~15 criterios): siempre activo
  - [ ] Grupo G — Negociacion (~15 criterios): siempre activo
- **MUST** Generar subconjunto personalizado por cliente (NO todos los 160+ criterios)
- **MUST** Identificar gaps (criterios importantes no abordados en la conversacion)
- **MUST** Generar ScoringDocument estructurado (JSON) para revision del equipo
- **SHOULD** Generacion de draft en <5 minutos por scoring
- **SHOULD** >80% de criterios correctamente extraidos del transcript
- **COULD** Agregacion multi-transcript (combinar multiples llamadas para el mismo cliente)

### Property Search & Match Agent (P1)

- **MUST** Modo autonomo — evaluar propiedades nuevas/actualizadas contra TODOS los scorings activos de clientes
- **MUST** Modo interactivo (chatbot) — responder consultas del equipo en lenguaje natural
- **MUST** Soporte multi-canal:
  - [ ] Slack
  - [ ] Claude Desktop / MCP
  - [ ] API
  - [ ] Internal App (cuando este disponible)
- **MUST** Memoria por cliente (historial de feedback, preferencias, visitas, outcomes)
- **MUST** Alertas proactivas cuando una nueva propiedad matchea scoring activo por encima del threshold
- **MUST** Alertas van al equipo SOLAMENTE — el agente NUNCA comunica directamente con el cliente
- **MUST** Generar shortlists rankeadas con justificacion por criterio
- **MUST** El feedback del cliente alimenta la memoria pero NUNCA modifica el scoring aprobado
- **SHOULD** Threshold de match configurable por cliente (default: 70%)
- **SHOULD** Tiempo de respuesta interactivo <30 segundos
- **SHOULD** >75% de concordancia con evaluacion humana
- **SHOULD** >50% de alertas proactivas resultan en propiedades compartidas al cliente
- **COULD** Aprendizaje de patrones de feedback para auto-ajustar pesos de ranking

---

## Tier 3: Intelligence & Analysis

### GIS Analysis Agent (P2)

- **MUST** Score compuesto de calidad de zona diferenciado por tipo de propiedad
- **MUST** Analisis de proximidad a POIs relevantes por tipo de propiedad
- **SHOULD** 10+ fuentes de datos:
  - [ ] INEGI DENUE
  - [ ] INEGI socioeconomico
  - [ ] Google Places
  - [ ] Google Distance Matrix
  - [ ] ArcGIS MCP
  - [ ] Catastro
  - [ ] Foot traffic
  - [ ] (fuentes adicionales incrementalmente)
- **SHOULD** Analisis especifico por tipo de propiedad:
  - [ ] Industrial: acceso logistico, vialidades, infraestructura
  - [ ] Comercial: foot traffic, NSE, densidad poblacional
  - [ ] Oficinas: transporte publico, conectividad, servicios
- **SHOULD** Integracion ArcGIS via MCP server
- **COULD** Agregar nuevas fuentes de datos incrementalmente
- **COULD** Analisis de tendencias historicas por zona

### Market Intelligence Agent (P2)

- **MUST** Reportes periodicos automaticos (precio/m2 por zona/tipo, inventario, tendencias)
- **MUST** Analisis comparativo on-demand ("comparar Zona A vs Zona B para tipo X")
- **SHOULD** Narrativa en lenguaje natural que explique insights en espanol
- **SHOULD** Cubrir todas las zonas con >10 propiedades en golden record
- **COULD** Exportar reportes para consumo del Tenant Portal
- **COULD** Analisis estacional y forecasting

---

## Capacidades Cross-cutting

### Arquitectura de Memoria (3 Capas)

- **MUST** Layer 1 — Shared Client Context en Supabase (scoring, feedback, visitas, shortlists)
- **MUST** Layer 2 — Agent-Specific Memory via Mastra (trazas de razonamiento, historial de decisiones)
- **SHOULD** Layer 3 — System Memory via Mastra (patrones de correccion, aprendizajes globales)
- **MUST** Todos los agentes leen Layer 1 para contexto de cliente
- **MUST** Enforcement de RLS — sin leakage de datos entre clientes

### Human-in-the-Loop (HITL)

- **MUST** Tabla `review_queue` en Supabase para casos ambiguos
- **MUST** Notificacion a Slack para cada caso HITL
- **MUST** Detalle del caso incluye: descripcion del problema, error potencial, opciones recomendadas
- **MUST** Feedback del equipo actualiza memoria del agente (no repetir errores)
- **SHOULD** Secciones en Internal App para tracking, revision y resolucion (Sprint 5+)
- **SHOULD** Metricas de volumen HITL (decreciente = sistema aprendiendo)

### Alertas Proactivas

- **MUST** Notificar al equipo cuando nueva propiedad matchea scoring activo por encima del threshold
- **MUST** Incluir: detalles de propiedad, match score, nombre del cliente, top 3 criterios que matchean
- **MUST** Canal: Slack (inmediato, post-scrape)
- **MUST** El agente nunca comunica directamente con el cliente
- **SHOULD** Historial de alertas loggeado para analytics
- **COULD** Integracion con Internal App cuando este disponible

### Observabilidad

- **MUST** Loggear todas las ejecuciones de agentes en tabla `agent_runs` (`agent_name`, `model`, `tokens`, `cost`, `duration`, `status`)
- **MUST** Monitoreo de costos por agente por mes
- **SHOULD** Alertas al 80% del threshold de presupuesto
- **COULD** Dashboard de metricas de performance de agentes

---

## Preguntas Abiertas

1. ¿Que modelos LLM especificos por agente? → Requiere evaluacion empirica (costo vs calidad vs velocidad)
2. ¿Presupuesto mensual LLM por agente? → TBD, depende de volumenes post-produccion
3. ¿Criterio exacto de threshold para merge automatico en dedup (0.95)?  → Validar con datos reales
4. ¿Formato final del ScoringDocument JSON? → Definir schema con Pablo
5. ¿Prioridad de fuentes de datos GIS? → Depende de disponibilidad de APIs y costos
