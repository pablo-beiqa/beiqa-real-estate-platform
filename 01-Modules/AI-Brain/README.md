# AI Brain

**Fase del proyecto**: Fase 3+ — Post-MVP
**Estado**: 🔴 Por iniciar
**Owner**: Por definir

---

## Descripcion

El modulo AI Brain es la capa de inteligencia artificial de la plataforma BEIQA. Su funcion principal es el matching inteligente entre los requerimientos de clientes corporativos y las propiedades disponibles en la base de datos. Utiliza procesamiento de lenguaje natural (NLP) para analizar descripciones de propiedades, extraer atributos no estructurados y enriquecer los registros. Ademas, implementa un motor de recomendaciones que aprende de las preferencias del equipo y el feedback de clientes para mejorar progresivamente la calidad de las sugerencias.

En la operacion actual de BEIQA, el proceso de encontrar propiedades que cumplan con los criterios de un cliente corporativo requiere experiencia y tiempo significativo del equipo. Un asesor debe mentalmente cruzar decenas de variables — superficie, precio, ubicacion, accesibilidad, tipo de inmueble, condiciones de contrato — contra un inventario de cientos o miles de opciones. Este modulo automatiza ese proceso de matching, genera shortlists ordenados por relevancia, y detecta propiedades duplicadas publicadas en multiples portales. Con el tiempo, el sistema aprende que combinaciones de factores son mas relevantes para cada tipo de cliente, mejorando la eficiencia del equipo.

---

## Objetivos

1. Implementar un motor de matching que genere shortlists de propiedades relevantes para un requerimiento de cliente con una precision de al menos 80% (medida como propiedades aceptadas / propiedades sugeridas).
2. Automatizar la extraccion de atributos de descripciones de propiedades en texto libre mediante NLP, reduciendo la entrada manual de datos en un 60%.
3. Detectar y consolidar propiedades duplicadas entre portales con una precision de al menos 90%, eliminando redundancia en la base de datos.

---

## Metricas de Exito / KPIs

| Metrica | Target | Como se mide |
|---------|--------|--------------|
| Precision de matching | >=80% de propiedades sugeridas son relevantes para el cliente | (Propiedades aceptadas en shortlist) / (propiedades sugeridas por el modelo) |
| Tasa de aceptacion de recomendaciones | >=60% de recomendaciones del sistema son incluidas en shortlists finales | Tracking de recomendaciones aceptadas vs rechazadas por el equipo |
| Tiempo para generar shortlist | <5 minutos (vs 2-4 horas manual) | Medicion de tiempo desde ingreso de requerimiento hasta shortlist generada |
| Precision de deduplicacion | >=90% de duplicados detectados correctamente | Muestreo manual de pares marcados como duplicados y verificacion |
| Atributos extraidos por NLP | >=85% de campos clave extraidos correctamente de descripciones | Comparacion de extraccion automatica vs etiquetado manual en muestra |

---

## Entregables Clave

| Entregable | Descripcion | Estado |
|-----------|-------------|--------|
| Motor de matching propiedad-cliente | Algoritmo que cruza requerimientos de clientes contra inventario de propiedades, ponderando multiples criterios | 🔴 |
| Pipeline de NLP | Procesamiento de descripciones de propiedades para extraer superficie, amenidades, condiciones de contrato y atributos cualitativos | 🔴 |
| API de recomendaciones | Endpoint que recibe un perfil de requerimiento y retorna propiedades rankeadas por relevancia con score explicable | 🔴 |
| Sistema de deduplicacion con AI | Modelo que identifica la misma propiedad publicada en diferentes portales usando similitud de texto, ubicacion y atributos | 🔴 |
| Feedback loop | Mecanismo para capturar aceptacion/rechazo de recomendaciones por el equipo y reentrenar el modelo periodicamente | 🔴 |

---

## Dependencias

### Necesita (upstream)
- **Base de datos** → Datos historicos de propiedades, requerimientos de clientes y feedback de shortlists anteriores
- **Scraper** → Descripciones de propiedades en texto libre y atributos estructurados para entrenamiento y ejecucion
- **Market Intelligence** → Tendencias de mercado y contexto de precios como features adicionales para el modelo
- **Geospatial** → Datos de proximidad, zona y contexto geografico como variables de matching

### Depende de este (downstream)
- **Internal App** → Muestra recomendaciones, shortlists generados por AI y propiedades deduplicadas al equipo
- **Tenant Portal** → Presenta recomendaciones personalizadas a clientes corporativos

---

## Riesgos Clave

| # | Riesgo | Impacto | Probabilidad | Mitigacion |
|---|--------|---------|--------------|------------|
| 1 | Escasez de datos de entrenamiento (pocas transacciones historicas con feedback estructurado) | Alto | Alta | Comenzar con reglas heuristicas + pesos configurables, capturar feedback desde el dia uno, migrar gradualmente a ML |
| 2 | Precision insuficiente del modelo que genere desconfianza del equipo | Alto | Media | Implementar explicabilidad (por que se recomienda cada propiedad), permitir ajuste manual de pesos, revision humana siempre |
| 3 | Costos de LLM/API de OpenAI para procesamiento de NLP a escala | Medio | Media | Evaluar modelos open-source (Llama, Mistral) para tareas de NLP, cache de resultados, procesamiento batch |
| 4 | Descripciones de propiedades en portales mexicanos con lenguaje inconsistente y errores | Medio | Alta | Pipeline de limpieza de texto pre-NLP, entrenamiento con datos reales del mercado mexicano, diccionario de terminos inmobiliarios |
| 5 | Complejidad de mantener modelos actualizados conforme cambian las condiciones del mercado | Medio | Media | Reentrenamiento periodico automatizado, monitoreo de drift en metricas de precision |

---

## Documentos del Modulo

- [Product Questions](./Product-Questions.md) — Cuestionario de discovery
- [Requirements](./Requirements.md) — Capacidades y criterios de aceptacion
- [Research/](./Research/) — Investigacion tecnica
