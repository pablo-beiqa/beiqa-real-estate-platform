# Contexto extraído del transcript de entrevista (Research Phase Strategy)

**Origen**: Transcript `07fdf293-ae94-4fb4-90fc-ce682623d9ca.txt`  
**Propósito**: Mantener como contexto persistente para conversaciones futuras y para seguir construyendo sobre las decisiones y hallazgos de la fase de investigación.

---

## Resumen ejecutivo

Se realizó una entrevista en varias rondas (1–8) para definir la estrategia de investigación de la plataforma BEIQA. El documento de arquitectura del sistema se reconoció como **visión**, no como diseño validado. Toda suposición debe verificarse con evidencia antes de comprometer recursos.

---

## Hechos clave (del proceso de entrevista)

| Área | Hecho |
|------|--------|
| **Timeline** | Urgente — resultados en 1–2 meses |
| **Presupuesto** | $50K–$100K USD |
| **Equipo** | No contratado aún — modelo híbrido (Pablo como PM/decision-maker + outsource + in-house) |
| **Alcance MVP** | UN portal (EasyBroker), UNA ciudad (CDMX), TODOS los tipos de propiedad |
| **Investigación hecha** | Cero — todos los docs actuales son plantillas sin hallazgos |
| **Mayor miedo** | Tomar decisiones tecnológicas equivocadas |
| **IA** | Diferida para matching/procesamiento; deduplicación con IA puede ser necesaria desde día uno |
| **HubSpot** | Se mantiene como CRM — BEIQA maneja propiedades/investigación y se sincroniza con HubSpot |
| **Construcción** | Decidido construir custom (no documentado por qué) |
| **Arquitectura** | Es VISIÓN — suposiciones no validadas |

---

## Corrección crítica sobre la arquitectura

El [System Architecture](../../04-Architecture/System-Architecture.md) **no** está 80–90 % cerrado. Es una lista de deseos. Supuestos **no** decididos:

- **Método de adquisición de datos**: Podría ser scraping automatizado, extensiones de navegador, APIs, importación manual, herramientas Chrome o híbrido. La arquitectura asume scraping automatizado — no validado.
- **Programación**: Cron automático, disparo manual o híbrido — no decidido.
- **Interfaz**: Podría ser dashboard web, chatbot IA (Claude), agente conversacional, herramienta tipo Airtable o híbrido. La arquitectura asume app web React/Next.js — no validado. El equipo es **no técnico**, la UX es crítica.
- **Deduplicación**: La misma propiedad aparece 7+ veces en UN portal (diferentes agentes). Es un problema de calidad de datos que requiere fuzzy matching / resolución de entidades con IA. La arquitectura apenas lo menciona.
- **REST vs GraphQL**: No decidido; posible complejidad innecesaria para MVP.
- **Almacenamiento de imágenes**: Solo URLs vs almacenamiento local — no decidido; impacta coste y confiabilidad.

---

## Las 5 preguntas centrales de investigación

Todo el trabajo de investigación se ordena en estas 5 preguntas:

1. **¿Cómo entra la información de propiedades al sistema?**  
   (Scraping, API, extensión, manual, híbrido — define la forma del resto del sistema.)

2. **¿Cómo manejamos la deduplicación de propiedades?**  
   (Misma propiedad, varios agentes, distintos precios/descripciones — fuzzy matching / IA.)

3. **¿Qué stack tecnológico usamos?**  
   (Incluye 6 ADRs abiertos + decisiones no documentadas; es el mayor miedo de Pablo.)

4. **¿La arquitectura propuesta es válida?**  
   (Validar o corregir cada componente y flujo según las respuestas a Q1–Q3.)

5. **¿Cuánto cuesta y es factible?**  
   (Números reales de coste y GO/NO-GO.)

---

## Respuestas de la entrevista (por ronda)

### Ronda 1 — Fundamentos
- **Quién construye**: Híbrido (Pablo + outsourced + in-house).
- **Presupuesto real**: $50K–$100K.
- **Urgencia**: Urgente — se pierden deals, se necesita algo en 1–2 meses.
- **Investigación hecha**: Nada — todo teórico.
- **Valor principal**: Scraper — dejar de buscar manualmente.

### Ronda 2 — Producto y técnica
- **Nivel del equipo**: Aún no contratado.
- **HubSpot**: Integrar (BEIQA propiedades/investigación; CRM en HubSpot).
- **Alcance scraper**: UN portal ya sería un cambio grande.
- **Tipos de propiedad**: Todos por igual — sistema flexible.
- **Foco geográfico**: Una ciudad/área metropolitana primero.

### Ronda 3 — Entregables y preocupaciones
- **Formato de entrega**: Mix según cliente/etapa del proyecto.
- **Ciudad clave**: CDMX / Estado de México.
- **Mayor preocupación**: Decisiones tecnológicas equivocadas.
- **Autoridad de decisión**: Pablo + socio.
- **Datos existentes**: Algunos Excel con datos de propiedades de proyectos pasados.

### Ronda 4 — Trade-offs y build vs buy
- **Build vs buy**: Ya decidido construir custom.
- **Portal prioritario**: EasyBroker (brokers comerciales/profesionales).
- **IA**: Fase posterior — primero recolección de datos.
- **Legal scraping**: No preocupa — dato público, uso interno, actuar con responsabilidad.
- **Métrica de éxito a 3 meses**: Mezcla de (A) scraper funcionando, (B) plan técnico validado, (C) MVP en uso.

### Rondas 5–6 — Stack y gaps de arquitectura
- **Python vs Node**: Necesitan que la investigación produzca la respuesta con evidencia.
- **Scheduling, frescura de datos, deduplicación, imágenes, auth, monitoreo**: Todos temas a investigar, no preferencias fijas.

### Ronda 7 — Principios (lo que realmente hay que investigar)
- **Adquisición de datos**: Investigar TODAS las opciones (scraping, extensión, manual, APIs, Chrome, híbrido).
- **Deduplicación**: Entender qué hace “igual” a una propiedad (dirección, coords, superficie, fotos, combinaciones).
- **Interfaz MVP**: ¿App web completa, spreadsheet+, panel simple, o algo más? — debe salir de la investigación.
- **Flujo actual**: Tiempo desde requerimiento del cliente hasta shortlist; cantidad de propiedades revisadas.
- **Ventaja competitiva**: Velocidad, cobertura, inteligencia, experiencia — la investigación debe priorizar cómo ganar en todas.

### Ronda 8 — IA, volumen, ownership, formato de investigación
- **Interfaz IA**: Muy en consideración (poder “preguntar” en lenguaje natural y obtener resultados).
- **Volumen de datos**: Por cuantificar en la investigación.
- **Quién ejecuta la investigación**: Pablo + asistencia de IA antes de involucrar humanos.
- **Formato de salida**: Tanto resumen para decisión (stakeholders) como detalle técnico para desarrolladores.

---

## Hallazgo crítico: EasyBroker

EasyBroker tiene **API pública documentada** (easybroker.com/developers). Si es el portal prioritario, podría no hacer falta construir scraper — solo integración vía API. Esto debe ser lo primero que valide la investigación.

---

## Estructura de investigación acordada (plan v2)

- **Research-Overview.md**: Índice maestro, metodología, fases, tracker.
- **Fase R-0**: Cuestionarios de producto (Product-Questions) — responder antes de investigación técnica.
- **Fase R-1**: Adquisición de datos (Q1) + Deduplicación (Q2).
- **Fase R-2**: Decisiones de tecnología (Q3), incl. interfaz.
- **Fase R-3**: Validación de arquitectura (Q4) + costos y factibilidad (Q5).
- **Fase R-4**: Diferido (más portales, INEGI, geo avanzado, IA, portal tenants, más ciudades).

---

## ADRs abiertos (recordatorio)

| ADR   | Tema                    | Opciones                          |
|-------|-------------------------|-----------------------------------|
| ADR-001 | Lenguaje backend        | Python vs Node.js                 |
| ADR-002 | Framework web           | FastAPI vs Express vs NestJS      |
| ADR-003 | Framework frontend      | Next.js vs Remix (y tipo de interfaz) |
| ADR-004 | Cloud provider          | AWS vs GCP vs Azure               |
| ADR-005 | Hosting BD              | Managed vs self-hosted            |
| ADR-006 | CI/CD                   | GitHub Actions vs GitLab CI       |

Más decisiones no documentadas: auth, object storage, cache, monitoreo, REST vs GraphQL, tecnología de scraper, scheduling.

---

## Uso de este documento

- **En conversaciones siguientes**: Usar este archivo como contexto compartido para no repetir suposiciones y alinear con lo ya decidido o abierto.
- **Para investigación**: Las 5 preguntas centrales y las fases R-0 a R-4 guían qué responder y en qué orden.
- **Para Product-Questions**: Las respuestas de las rondas 1–8 indican qué preguntas de producto aún no están cubiertas (p. ej. interfaz interna, método de adquisición aceptable para MVP, criterios de go/no-go).

Si se usa **agent-memory-mcp**, se pueden guardar también como memorias clave: “Arquitectura es visión no validada”, “MVP = EasyBroker + CDMX + todos los tipos”, “Deduplicación es problema central”, “Investigación debe evaluar todas las opciones de adquisición de datos”.
