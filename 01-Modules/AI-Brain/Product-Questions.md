# AI Brain — Product Questions

> Cuestionario de discovery del módulo. Respuestas provienen de entrevista con Pablo (CEO) el 2026-03-11.
>
> **Estado**: ✅ Respondido (primera ronda)

---

## A. Visión General de IA

**1. Si pudieras describir un asistente de IA perfecto para bienes raíces, ¿qué haría?**
> Automatizaría todo el proceso manual actual: buscar en múltiples portales, procesar datos en Excel, verificar direcciones en Google Maps, armar presentaciones para clientes. El asistente tendría memoria por cliente, buscaría inteligentemente con los parámetros correctos, se desviaría lo menos posible de la realidad, y tendría un mecanismo de human-in-the-loop para casos ambiguos donde pueda pausar el flujo y pedir revisión humana.

**2. ¿Qué tareas repetitivas hace el equipo que podrían automatizarse?**
> 1. Buscar propiedades en múltiples portales (Inmuebles24, Pincali, FinSA, CBRE, Colliers)
> 2. Limpiar y verificar datos de propiedades (direcciones incorrectas, precios inconsistentes, duplicados)
> 3. Generar presentaciones y shortlists para clientes
> Las tres fueron seleccionadas como los mayores pain points.

**3. ¿Qué decisiones toma el equipo hoy que podrían beneficiarse de sugerencias de IA?**
> - Qué propiedades incluir en un shortlist para un cliente específico
> - Qué zonas recomendar basado en los requerimientos del cliente
> - Detectar cuándo una propiedad nueva es relevante para un cliente activo (alertas proactivas)

**4. ¿Qué experiencia tiene el equipo con herramientas de IA?**
> Pablo usa Claude Desktop con MCP bridges (Rube) como interfaz actual para consultar datos en Supabase. El scoring dashboard del frontend usa un agente Claude para procesar transcripts de Circleback (PoC, no en producción). Fabrizio construye scrapers con Trigger.dev + Firecrawl.

**5. ¿Qué expectativas tiene el equipo sobre lo que la IA puede y no puede hacer?**
> Expectativas altas pero realistas. El equipo entiende que la IA necesita datos limpios para funcionar, que requiere evaluación empírica, y que el sistema debe tener mecanismos de control (confidence scores, HITL, umbrales configurables).

---

## B. Matching y Recomendaciones

**6. ¿Cómo debería la IA "emparejar" una propiedad con los requerimientos de un cliente?**
> Mediante un scoring de 160+ criterios organizados en 7 grupos (A-G). Grupos A (Universal), F (Operacional) y G (Negociación) siempre activos. Grupos B (Industrial), C (Comercial), D (Oficinas), E (Terreno) se activan según tipo de propiedad. Cada cliente tiene un subconjunto personalizado — no todos los criterios aplican. Ver [Research/Scoring-Criteria.md](Research/Scoring-Criteria.md) para el catálogo completo.

**7. Si un cliente envía una solicitud vaga, ¿cómo debería la IA interpretar y expandir eso?**
> El agente debe usar el scoring del cliente como base, complementado con la memoria (feedback previo, visitas, preferencias). Si no hay suficiente contexto, el agente debe preguntar (modo interactivo) o escalar al equipo (modo autónomo).

**8. ¿Qué factores no explícitos debería considerar la IA al recomendar?**
> La memoria del cliente: qué propiedades rechazó y por qué, qué le gustó de propiedades anteriores, outcomes de visitas. También el contexto operativo del cliente (tipo de negocio, vehículos, empleados, rutas de distribución — Grupo F de criterios).

**9. ¿Cómo debería la IA manejar casos donde no hay matches perfectos?**
> Presentar las mejores opciones con justificación transparente de dónde falla el match. El equipo Beiqa interpreta y decide si vale la pena presentar al cliente.

**10. ¿La IA debería aprender de las decisiones del equipo?**
> Sí, a través de la memoria en 3 capas: Layer 1 (datos estructurados del cliente en Supabase), Layer 2 (memoria específica del agente), Layer 3 (patrones globales del sistema). Las correcciones del equipo alimentan Layer 3 para que los errores no se repitan.

**11. ¿Qué tan importante es que la IA explique POR QUÉ recomienda algo?**
> Muy importante. El Tenant Portal muestra transparencia total del scoring (criterios, pesos, evaluación por criterio). Las justificaciones son parte integral del output del agente.

---

## C. Búsqueda en Lenguaje Natural

**12. ¿Qué consultas en lenguaje natural debería entender el sistema?**
> - "Busca bodegas de 5,000m² en Toluca con andenes"
> - "¿Qué opciones hay para [Cliente X] en la zona de Cuautitlán?"
> - "Compara estas 3 propiedades para [Cliente Y]"
> - "Dame el inventario de locales comerciales en Naucalpan"

**13. ¿El equipo preferiría escribir consultas o hablarlas por voz?**
> Escribir. Multi-canal: Slack, Claude Desktop/MCP, Internal App (cuando exista), API.

**14. ¿Qué pasa cuando la IA no entiende una consulta?**
> Debe pedir clarificación en lenguaje natural. Si no hay suficiente contexto, preguntar específicamente qué falta.

**15. ¿La IA debería recordar el contexto de conversaciones anteriores?**
> Sí. Memoria por cliente es crítica: qué dijo, qué le gustó, qué no le gustó, visitas, outcomes. También memoria de la conversación actual (para refinamientos tipo "dame más como la #1").

**16. ¿Qué idioma(s) debería entender la IA?**
> Español (México) es el idioma principal. Inglés para términos técnicos inmobiliarios (LEED, BTS, NNN, CAM).

---

## D. Generación de Contenido

**17. ¿Qué comunicaciones debería resumir la IA?**
> Transcripts de llamadas vía Circleback (fuente principal para Score Discovery Agent). No emails ni WhatsApp por ahora.

**18. ¿La IA debería generar reportes de mercado?**
> Sí. Market Intelligence Agent genera: reportes periódicos automáticos (precio/m², inventario, tendencias), comparativos on-demand ("compara Toluca vs Cuautitlán"), y narrativa en lenguaje natural que explique insights.

**19. ¿Qué tipo de textos debería poder generar la IA?**
> Justificaciones de scoring por propiedad, narrativas de mercado, resúmenes de zona (GIS Analysis). NO PowerPoints ni PDFs — el Tenant Portal reemplaza esos formatos.

**20. ¿El equipo necesita que la IA traduzca contenido?**
> No como función primaria. Español es el idioma de trabajo.

**21. ¿La IA debería poder generar presentaciones?**
> No. El Tenant Portal es la interfaz de presentación al cliente. No se generan PPT ni PDF.

**22. ¿Qué tono y estilo?**
> Profesional, directo, con datos. Evitar lenguaje marketero. El tono es de asesoría, no de venta.

---

## E. Autonomía y Confianza

**23. ¿Cuánto debería decidir la IA por sí misma?**
> Depende del agente. Tier 1 (Data Pipeline) puede ser altamente autónomo — solo escala cuando confidence es bajo. Tier 2 (Client Intelligence) requiere revisión del equipo antes de presentar al cliente. El agente NUNCA comunica directamente con el cliente.

**24. ¿Qué haría que el equipo confíe en las recomendaciones?**
> Confidence scores transparentes, justificaciones claras por criterio, capacidad de ver el reasoning del agente, y un historial demostrable de mejora con el tiempo.

**25. ¿Qué acciones NUNCA debería tomar la IA sin aprobación humana?**
> - Comunicar directamente con un cliente
> - Modificar el scoring aprobado por el cliente
> - Eliminar propiedades del golden record
> - Enviar shortlists sin revisión del equipo

**26. ¿Cómo debería manejar la incertidumbre?**
> Con un mecanismo formal de HITL: detalla el caso ambiguo, explica el error potencial, manda notificación, almacena en review queue, y espera resolución. Las resoluciones se guardan como patrones de aprendizaje.

**27. ¿El equipo necesita poder "corregir" a la IA?**
> Sí, y es crítico que la IA aprenda de las correcciones. La Internal App (futuro) tendrá secciones de tracking, revisión, y feedback. Las correcciones alimentan System Memory (Layer 3).

**28. ¿Qué nivel de error es aceptable?**
> - Address Enrichment: >80% accuracy
> - Data Normalization: >95% campos correctos
> - Deduplication: >85% precisión, <2% falsos positivos
> - Scoring/Match: >75% concordancia con evaluación humana

---

## F. Alertas y Proactividad

**29. ¿Debería la IA enviar alertas proactivas?**
> Sí, es crítico. Es un diferenciador de negocio. Cuando aparece una propiedad nueva que matchea el scoring de un cliente activo, el equipo debe enterarse inmediatamente.

**30. ¿Qué tipo de alertas serían más valiosas?**
> "Nueva bodega en Toluca de 3,000m² a $85/m² — match 87% con scoring de [Cliente X]". Incluir: propiedad, score, cliente, top 3 criterios que matchean.

**31. ¿Con qué frecuencia?**
> Inmediata post-scrape (no batch). Con el volumen actual (~30K propiedades totales, ~5-15 clientes) no hay riesgo de fatiga de alertas.

**32. ¿Por qué canal?**
> Slack inicialmente, Internal App cuando exista.

**33. ¿La IA debería sugerir acciones basadas en patrones?**
> Futuro. Primero las alertas de match, después patrones más sofisticados.

---

## G. Calidad de Datos

**34. ¿Qué rol juega la IA en la calidad de datos?**
> Central. Tres agentes de Tier 1 (Address Enrichment, Data Normalization, Deduplication) existen específicamente para resolver problemas de calidad de datos. El 50%+ de direcciones de I24/Pincali son incorrectas.

**35. ¿Detectar datos incorrectos automáticamente?**
> Sí. Confidence scores para direcciones, validación de rangos para precios/superficies, detección de duplicados cross-portal.

**36. ¿Sugerir correcciones o completar datos faltantes?**
> Sí. El agente propone correcciones. Si confidence es alto (>80), aplica automáticamente. Si es bajo (<50), va a review queue para corrección humana.

**37. ¿Datos contradictorios de diferentes fuentes?**
> Data Normalization escala al equipo vía HITL. El agente detalla qué dice cada fuente y cuál recomienda.

**38. ¿Enriquecer datos automáticamente?**
> Sí: geocoding, reverse geocoding, feature extraction de descripciones, PDF extraction como fallback cuando el scraper no capturó datos completos.

---

## H. Integración con Otros Módulos

**39. ¿Cómo interactúa la IA con el módulo geoespacial?**
> GIS Analysis Agent consume 10+ fuentes (INEGI, Google, ArcGIS, Catastro, foot traffic) para generar zone quality scores diferenciados por tipo de propiedad. H3/AGEB se calculan por triggers en Supabase, no por agente. Ver [Research/GIS-Analysis-Strategy.md](Research/GIS-Analysis-Strategy.md).

**40. ¿Análisis de mercado automáticamente?**
> Sí, Market Intelligence Agent genera reportes periódicos + on-demand + narrativa.

**41. ¿Integración con HubSpot?**
> No directamente desde Mastra. La sincronización HubSpot ↔ Supabase la maneja Trigger.dev (determinístico). Mastra consume datos del cliente que ya están en Supabase.

**42. ¿IA en el portal de clientes?**
> No como chatbot. El cliente ve los resultados de los agentes (scoring, shortlists, scores por criterio, datos de mercado) pero no interactúa directamente con la IA.

**43. ¿Leer y procesar documentos?**
> Sí, PDFs de fichas técnicas como fallback. Si datos no están en las staging tables, el agente busca en los PDFs almacenados en Supabase Storage (buckets: cbre-pdfs, colliers-pdfs, finsa-flyers).

---

## I. Interfaz de Interacción

**44. ¿Chat, interfaz visual, o ambas?**
> Ambas. Modo interactivo del Property Search & Match Agent es chat. Los resultados se presentan como interfaz visual en Tenant Portal e Internal App.

**45. ¿Personalidad o neutral?**
> Neutral y funcional. Profesional, con datos.

**46. ¿Ver el razonamiento de la IA?**
> Sí. Transparencia total: confidence scores, señales usadas, justificación por criterio, breakdown del scoring.

**47. ¿Siempre visible o bajo demanda?**
> Bajo demanda en modo interactivo. Proactivo solo para alertas de match.

**48. ¿Desde móvil?**
> No es prioridad. El equipo (Pablo + Jerónimo) son los usuarios principales del modo interactivo, usan desktop.

---

## J. Aprendizaje y Mejora

**49. ¿Mejorar con el tiempo basándose en el uso?**
> Sí, a través de la memoria en 3 capas. Las correcciones del equipo alimentan System Memory. El feedback del cliente alimenta Client Memory.

**50. ¿El equipo daría feedback sobre recomendaciones?**
> Sí, a través de: correcciones en HITL, aprobación/rechazo de shortlists, evaluación de outputs de scoring.

**51. ¿Qué datos usar para mejorar la IA?**
> Correcciones de direcciones, pares de duplicados confirmados/rechazados, feedback del cliente, scorings aprobados, outcomes de visitas.

**52. ¿Preocupaciones sobre privacidad?**
> Datos de clientes son confidenciales. Aislamiento estricto por tenant (RLS en Supabase). La IA no debe mezclar información entre clientes.

---

## K. Casos de Uso Específicos

**53. Escenario ideal de IA acelerando un deal:**
> 1. Cliente tiene llamada con Beiqa → Score Discovery procesa transcript → genera scoring draft en 5 min
> 2. Equipo revisa, aprueba scoring
> 3. Property Search & Match evalúa ~30K propiedades contra scoring → genera shortlist rankeada
> 4. Cliente revisa en Tenant Portal, da feedback
> 5. Agente aprende del feedback → siguiente shortlist es mejor
> 6. Entre rondas, propiedad nueva llega y matchea 90% → alerta inmediata al equipo → se agrega a shortlist
> Total: ciclo de días → ciclo de horas.

**54. ¿La IA durante una llamada con cliente?**
> Futuro. Hoy el transcript se procesa post-llamada vía Circleback.

**55. ¿Preparar antes de una reunión?**
> Futuro. Con scoring + memoria del cliente, el agente podría generar un briefing pre-reunión.

**56. ¿Responder preguntas de clientes directamente?**
> No. El agente nunca comunica directamente con clientes. El equipo Beiqa es siempre el intermediario.

---

## L. Riesgos y Limitaciones

**57. ¿Si la IA da una recomendación incorrecta a un cliente?**
> El equipo siempre revisa antes de compartir con el cliente. No hay riesgo de recomendación incorrecta directa porque el flujo incluye revisión humana.

**58. ¿Situaciones donde la IA NO debería usarse?**
> Comunicación directa con clientes, decisiones de pricing, negociación de contratos, cualquier acción que comprometa a Beiqa legalmente.

**59. ¿Respuestas verificables?**
> Muy importante. Transparency: source citations, confidence scores, signals used.

**60. ¿Poder "apagar" la IA?**
> No es un toggle — cada agente puede correrse o no. Si hay problemas, se desactiva el trigger que lo ejecuta.

---

## M. Priorización y MVP

**61. Si la IA solo pudiera hacer UNA cosa bien, ¿qué sería?**
> Corregir direcciones (Address Enrichment). Es el bloqueador de todo lo demás.

**62. ¿Capacidades necesarias para MVP?**
> - **MUST**: Address Enrichment (P0), Data Normalization (P0)
> - **SHOULD**: Score Discovery (P1), Property Search & Match autónomo + alertas (P1), Deduplication (P1)
> - **COULD**: Property Search & Match interactivo, GIS Analysis, Market Intelligence

**63. ¿Qué problema resuelto cambiaría más el día a día?**
> Datos limpios y confiables (Tier 1 completo) + alertas proactivas de match (Tier 2). Juntos eliminan las horas de búsqueda manual y verificación.

**64. ¿La IA es diferenciador clave o nice-to-have?**
> Diferenciador clave. Los alertas proactivas, el scoring inteligente, y la memoria por cliente son lo que distingue a Beiqa de brokers tradicionales que hacen todo manualmente.

---

*Entrevista realizada: 2026-03-11 | Entrevistado: Pablo (CEO)*
