# Panorama Competitivo -- Proptech México/LatAm

**Fase**: R-1 (Datos Core)
**Estado**: 🔴 Por investigar
**Depende de**: Cuestionarios de producto (R-0)

---

## Objetivo

Antes de construir, entender qué existe. Otras empresas proptech en México/LatAm han resuelto partes de estos problemas. Entender el panorama ayuda a definir qué significa "mejor" y evitar reinventar la rueda.

---

## Preguntas de Investigación

1. ¿Qué plataformas proptech existen en México orientadas a inmuebles comerciales/industriales?
2. ¿Qué herramientas usan CBRE/JLL/Cushman internamente para inteligencia de mercado?
3. ¿Qué plataformas open-source o comerciales de datos inmobiliarios existen globalmente?
4. ¿Qué partes de la visión BEIQA ya están resueltas por herramientas existentes?
5. ¿Qué gaps justifican construir custom?
6. ¿Qué podemos aprender de su enfoque? (Fuentes de datos, patrones de UX, pricing, features)

---

## Competidores Directos (México)

### Solili

| Campo | Hallazgo |
|-------|----------|
| URL | solili.mx |
| Enfoque | Industrial y logístico |
| Cobertura | México y LatAm |
| Tipo de datos | Inventario, vacancia, absorción, precios |
| Modelo de negocio | Suscripción |
| Costo estimado | 🔴 Por investigar |
| Fortalezas | 🔴 Por investigar |
| Debilidades | 🔴 Por investigar |
| ¿Qué resuelve que BEIQA necesita? | 🔴 Por investigar |
| ¿Qué NO resuelve? | 🔴 Por investigar |

### SiiLA (Sistemas de Información Inmobiliaria Latinoamérica)

| Campo | Hallazgo |
|-------|----------|
| URL | siila.com.mx |
| Enfoque | Datos inmobiliarios comerciales |
| Cobertura | México, Brasil, LatAm |
| Tipo de datos | Inventario, transacciones, analytics |
| Modelo de negocio | 🔴 Por investigar |
| ¿Qué resuelve que BEIQA necesita? | 🔴 Por investigar |
| ¿Qué NO resuelve? | 🔴 Por investigar |

### Datoz

| Campo | Hallazgo |
|-------|----------|
| URL | datoz.com |
| Enfoque | Inteligencia inmobiliaria |
| Cobertura | México |
| Tipo de datos | Market intelligence, analytics |
| Modelo de negocio | 🔴 Por investigar |
| ¿Qué resuelve que BEIQA necesita? | 🔴 Por investigar |
| ¿Qué NO resuelve? | 🔴 Por investigar |

### 4S Real Estate

| Campo | Hallazgo |
|-------|----------|
| URL | 4srealestate.com |
| Enfoque | Consultoría y data inmobiliaria |
| Cobertura | México y LatAm |
| Tipo de datos | 🔴 Por investigar |
| ¿Qué resuelve que BEIQA necesita? | 🔴 Por investigar |
| ¿Qué NO resuelve? | 🔴 Por investigar |

### EasyBroker (como plataforma, no solo portal)

| Campo | Hallazgo |
|-------|----------|
| URL | easybroker.com |
| Enfoque | CRM y herramientas para brokers |
| Cobertura | México |
| Tipo de datos | Listings, CRM, sitio web para brokers |
| API pública | Sí -- REST API documentada |
| ¿Qué resuelve que BEIQA necesita? | 🔴 Por investigar |
| ¿Qué NO resuelve? | 🔴 Por investigar |

---

## Herramientas de Brokers Grandes

### CBRE

| Campo | Hallazgo |
|-------|----------|
| Herramientas internas conocidas | 🔴 Por investigar |
| Tipo de reportes que publican | MarketView, Quarterly reports |
| ¿Qué métricas incluyen? | 🔴 Por investigar |
| ¿Qué podemos aprender? | 🔴 Por investigar |

### JLL

| Campo | Hallazgo |
|-------|----------|
| Herramientas internas conocidas | 🔴 Por investigar |
| Tipo de reportes que publican | Market Outlook, Research reports |
| ¿Qué métricas incluyen? | 🔴 Por investigar |

### Cushman & Wakefield

| Campo | Hallazgo |
|-------|----------|
| Herramientas internas conocidas | 🔴 Por investigar |
| Tipo de reportes que publican | MarketBeat |
| ¿Qué métricas incluyen? | 🔴 Por investigar |

---

## Referentes Globales

### CoStar (EE.UU.)

| Campo | Hallazgo |
|-------|----------|
| URL | costar.com |
| Modelo | Suscripción premium para datos inmobiliarios comerciales |
| Relevancia para BEIQA | Modelo aspiracional de datos inmobiliarios |
| ¿Opera en México? | 🔴 Por verificar |
| Lecciones | 🔴 Por investigar |

### Zillow / Redfin (EE.UU. -- residencial)

| Campo | Hallazgo |
|-------|----------|
| Relevancia | Modelos de datos, UX patterns para búsqueda de propiedades |
| Diferencia clave | Enfocados en residencial, no comercial/industrial |
| Lecciones de UX | 🔴 Por investigar |

---

## Análisis de Gaps

| Capacidad de BEIQA | ¿Solili? | ¿SiiLA? | ¿Datoz? | ¿EasyBroker? | ¿Otros? | Gap = Oportunidad |
|---------------------|----------|---------|---------|--------------|---------|-------------------|
| Scraping multi-portal | 🔴 | 🔴 | 🔴 | 🔴 | 🔴 | 🔴 |
| Deduplicación IA | 🔴 | 🔴 | 🔴 | 🔴 | 🔴 | 🔴 |
| Análisis geoespacial | 🔴 | 🔴 | 🔴 | 🔴 | 🔴 | 🔴 |
| Inteligencia de mercado | 🔴 | 🔴 | 🔴 | 🔴 | 🔴 | 🔴 |
| Matching propiedad-cliente | 🔴 | 🔴 | 🔴 | 🔴 | 🔴 | 🔴 |
| Portal para tenants | 🔴 | 🔴 | 🔴 | 🔴 | 🔴 | 🔴 |
| Integración HubSpot | 🔴 | 🔴 | 🔴 | 🔴 | 🔴 | 🔴 |
| AI/NLP para búsquedas | 🔴 | 🔴 | 🔴 | 🔴 | 🔴 | 🔴 |

---

## Conclusiones (Por Completar)

### ¿Qué justifica construir custom?

🔴 _Por documentar después de la investigación_

### ¿Qué se puede comprar/suscribir en vez de construir?

🔴 _Por documentar después de la investigación_

### ¿Qué lecciones incorporar de los competidores?

🔴 _Por documentar después de la investigación_

---

## Acciones de Investigación

1. [ ] Solicitar demo/trial de Solili
2. [ ] Solicitar demo/trial de SiiLA
3. [ ] Investigar Datoz en detalle
4. [ ] Revisar reportes públicos de CBRE/JLL/Cushman México
5. [ ] Investigar si CoStar opera en México
6. [ ] Buscar startups proptech emergentes en México
7. [ ] Documentar hallazgos en este documento
8. [ ] Completar la tabla de gaps
