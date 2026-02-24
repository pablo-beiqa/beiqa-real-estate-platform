# Estrategia de IA / LLM

**Fase**: R-2 (Tecnología y Plataforma)
**Estado**: 🔴 Por investigar
**Depende de**: Cuestionario 06-Cerebro-IA (R-0), Deduplication Strategy (R-1)

---

## Objetivo

Definir dónde la IA agrega valor real vs. donde reglas simples son suficientes. Evaluar opciones de LLM, estimar costos, y determinar qué capacidades son MVP vs. post-MVP.

> **Contexto de la entrevista**: "IA diferida para procesamiento/matching -- pero deduplicación con IA puede ser necesaria desde el día uno."

---

## Mapa de Puntos de Integración de IA

| Punto de Integración | Valor | Complejidad | ¿MVP? | ¿IA necesaria o reglas suficientes? |
|---------------------|-------|-------------|--------|-------------------------------------|
| Deduplicación de propiedades | Muy alto | Media | ✅ Sí | IA para casos ambiguos, reglas para obvios |
| Matching propiedad-cliente | Alto | Media | ⚠️ Básico | Reglas para MVP, IA para post-MVP |
| Interpretación NLP de búsquedas | Alto | Media | ❌ No | IA necesaria |
| Resumen de comunicaciones | Medio | Baja | ❌ No | IA necesaria |
| Generación de reportes | Alto | Media | ❌ No | IA asistida |
| Alertas proactivas | Medio | Baja | ❌ No | Reglas suficientes |
| Interfaz conversacional (chatbot) | Alto | Alta | ❌ No (pero evaluar) | IA necesaria (RAG) |
| Clasificación de propiedades | Medio | Baja | ⚠️ Posible | Reglas para MVP |
| Detección de anomalías en datos | Medio | Media | ❌ No | IA o estadística |
| Enriquecimiento de datos | Medio | Baja | ❌ No | IA para texto libre |

---

## Análisis por Punto de Integración

### 1. Deduplicación con IA (MVP)

> Detalle completo en [Deduplication-Strategy.md](../../../02-Architecture/Deduplication-Strategy.md)

| Aspecto | Investigación |
|---------|---------------|
| Rol de IA | Resolver casos ambiguos que el scoring automático no puede decidir |
| Prompt | Presentar dos propiedades candidatas y preguntar si son la misma |
| Input estimado | ~200-500 tokens por par |
| Output | Sí/No + confianza + justificación |
| Volumen estimado | 🔴 Depende de % de casos ambiguos |
| Modelo recomendado | GPT-4o-mini o Claude Haiku (bajo costo, suficiente para clasificación) |

**Preguntas de investigación:**
- [ ] ¿Cuál es la precisión de GPT-4o-mini vs Claude Haiku para este task?
- [ ] ¿Cuántos pares ambiguos se generan por lote de importación?
- [ ] ¿El costo es aceptable a escala? (estimar tokens × precio)

### 2. Matching Propiedad-Cliente

| Aspecto | MVP (Reglas) | Post-MVP (IA) |
|---------|-------------|---------------|
| Implementación | SQL con filtros + scoring simple | LLM que interpreta requerimientos libres |
| Criterios | Tipo, zona, superficie, precio | + "buen acceso a autopista", "zona segura" |
| Resultado | Score numérico | Score + explicación en texto |
| Costo | $0 | Tokens por consulta |

### 3. Interfaz Conversacional (Evaluar para Post-MVP)

| Aspecto | Investigación |
|---------|---------------|
| Arquitectura | RAG: Embeddings de propiedades + LLM para conversación |
| Embeddings | OpenAI text-embedding-3-small o similar |
| Vector store | pgvector (extensión PostgreSQL) o Pinecone |
| LLM | Claude o GPT-4o para generación |
| Ejemplo de query | "Muéstrame bodegas de 2000m2 cerca de Querétaro con buen acceso a autopista" |

**Preguntas de investigación:**
- [ ] ¿pgvector es suficiente o se necesita vector DB dedicada?
- [ ] ¿Cuál es la latencia aceptable para el equipo?
- [ ] ¿El costo por conversación es viable?

### 4. Generación de Reportes

| Aspecto | Investigación |
|---------|---------------|
| Descripción | LLM que genera narrativa a partir de datos estructurados |
| Input | Métricas de mercado, propiedades comparables, datos de zona |
| Output | Texto para reporte (se inserta en template) |
| Modelo | GPT-4o o Claude (necesita calidad en español) |

---

## Opciones de LLM

| Modelo | Provider | Calidad Español | Costo (input/output por 1M tokens) | Latencia | Mejor para |
|--------|----------|-----------------|-------------------------------------|----------|-----------|
| GPT-4o | OpenAI | ✅ Excelente | ~$2.50 / $10.00 | Media | Reportes, NLP complejo |
| GPT-4o-mini | OpenAI | ✅ Buena | ~$0.15 / $0.60 | Rápida | Clasificación, dedup |
| Claude 3.5 Sonnet | Anthropic | ✅ Excelente | ~$3.00 / $15.00 | Media | Análisis, reportes |
| Claude 3.5 Haiku | Anthropic | ✅ Buena | ~$0.25 / $1.25 | Rápida | Clasificación, dedup |
| Llama 3.1 70B | Meta (self-hosted) | ⚠️ Buena | Costo de hosting | Variable | Sin costo por token |
| Mistral Large | Mistral | ⚠️ Media-Buena | ~$2.00 / $6.00 | Media | Alternativa europea |

### Recomendación Preliminar por Uso

| Uso | Modelo Recomendado | Justificación |
|-----|-------------------|---------------|
| Deduplicación (MVP) | GPT-4o-mini o Haiku | Bajo costo, tarea simple |
| Reportes (post-MVP) | GPT-4o o Claude Sonnet | Necesita calidad en español |
| Chatbot (post-MVP) | Claude Sonnet o GPT-4o | Conversacional, español natural |
| Embeddings | text-embedding-3-small | Estándar, buen precio |

---

## Estimación de Costos

### Escenario MVP (Solo deduplicación)

| Concepto | Volumen/mes | Tokens/unidad | Costo/mes |
|----------|------------|---------------|-----------|
| Pares de dedup ambiguos | ~200 | ~400 tokens | ~$0.05-$0.10 |
| **Total MVP** | | | **< $1/mes** |

### Escenario Post-MVP (Dedup + Matching + Reportes)

| Concepto | Volumen/mes | Tokens/unidad | Costo/mes |
|----------|------------|---------------|-----------|
| Deduplicación | ~500 pares | ~400 tokens | ~$0.10 |
| Matching queries | ~100 | ~2000 tokens | ~$0.50 |
| Reportes generados | ~20 | ~5000 tokens | ~$2.00 |
| Conversaciones chatbot | ~200 | ~3000 tokens | ~$3.00 |
| **Total Post-MVP** | | | **~$5-10/mes** |

> Los costos de LLM son sorprendentemente bajos para el volumen de BEIQA. No es un bloqueante.

---

## Decisiones Técnicas

### Embeddings y Vector Search

| Opción | Tipo | Costo | Integración con PostgreSQL |
|--------|------|-------|---------------------------|
| pgvector | Extensión PostgreSQL | $0 (incluido) | ✅ Nativa |
| Pinecone | Managed vector DB | $0-70/mes | Via API |
| Weaviate | Open source vector DB | Self-hosted | Via API |
| Qdrant | Open source vector DB | Self-hosted | Via API |

**Recomendación**: pgvector (si se usa PostgreSQL, ya lo tiene. Menos componentes.)

### SDK / Framework

| Opción | Lenguaje | Pros | Contras |
|--------|----------|------|---------|
| LangChain | Python/JS | Muchas integraciones, RAG built-in | Complejo, overhead |
| LlamaIndex | Python | Optimizado para RAG | Menos flexible |
| OpenAI SDK directo | Python/JS | Simple, sin overhead | Más código custom |
| Anthropic SDK directo | Python/JS | Simple, sin overhead | Más código custom |
| Vercel AI SDK | JS/TS | Streaming, React hooks | Solo JS |

**Recomendación preliminar**: SDK directo para MVP (simple), evaluar LangChain para post-MVP (si se necesita RAG complejo).

---

## Preguntas de Investigación

### Técnicas

- [ ] ¿GPT-4o-mini o Haiku tiene mejor F1-score para deduplicación inmobiliaria?
- [ ] ¿pgvector es suficiente para nuestro volumen de embeddings?
- [ ] ¿La calidad del español de los modelos es aceptable para reportes?
- [ ] ¿Se necesita fine-tuning para terminología inmobiliaria mexicana?

### De Producto

- [ ] ¿El equipo quiere un chatbot como interfaz principal o complementario?
- [ ] ¿Qué nivel de autonomía debe tener la IA? (Auto vs. sugerencia)
- [ ] ¿El equipo confía en recomendaciones de IA?

---

## Acciones de Investigación

1. [ ] Hacer PoC de deduplicación con GPT-4o-mini (10 pares reales)
2. [ ] Comparar con Claude Haiku (mismos 10 pares)
3. [ ] Estimar volumen real de tokens para el MVP
4. [ ] Evaluar pgvector vs. servicio externo
5. [ ] Prototipar chatbot simple con datos de prueba
6. [ ] Calcular costos mensuales reales
7. [ ] Documentar decisiones
