# Data Ingestion

**Fase del proyecto**: Fase 2 — Post-MVP
**Estado**: 🔴 Por iniciar
**Owner**: Por definir

---

## Descripcion

El modulo de Data Ingestion se encarga de integrar fuentes de datos externos estructurados que enriquecen la informacion base de propiedades capturada por el Scraper. Estas fuentes incluyen datos demograficos del INEGI (poblacion, nivel socioeconomico por AGEB), puntos de interes de Google Maps (restaurantes, bancos, transporte), datos catastrales de gobiernos municipales, e indicadores economicos de proveedores comerciales. El objetivo es que cada propiedad en la base de datos de BEIQA no solo tenga sus atributos inmobiliarios, sino tambien el contexto completo de su entorno.

Para un equipo que asesora a empresas en la seleccion de ubicaciones comerciales e industriales, la decision no depende solo del inmueble sino de factores como acceso a mano de obra, cercania a vias de transporte, densidad comercial de la zona y tendencias economicas locales. Este modulo automatiza la recopilacion de esos datos contextuales que hoy el equipo investiga manualmente en multiples sitios web y reportes gubernamentales, consolidandolos en un solo punto de acceso dentro de la plataforma.

---

## Objetivos

1. Enriquecer los registros de propiedades con datos demograficos, economicos y de puntos de interes, logrando que al menos el 70% de las propiedades tengan perfil de zona completo.
2. Integrar al menos 5 fuentes de datos externas (INEGI, Google Maps/Places, catastro, indicadores economicos, transporte) con pipelines automatizados.
3. Reducir el tiempo que el equipo dedica a investigacion contextual de zonas de mas de 2 horas a menos de 5 minutos por propiedad.

---

## Metricas de Exito / KPIs

| Metrica | Target | Como se mide |
|---------|--------|--------------|
| Fuentes de datos integradas | >=5 fuentes activas en produccion | Conteo de conectores con datos actualizados en ultimos 30 dias |
| Cobertura de datos por propiedad | >=70% de propiedades con perfil de zona completo | (Propiedades con >=4 campos de enriquecimiento) / (total propiedades) |
| Completitud de enriquecimiento | >=85% de campos de enriquecimiento poblados donde hay datos disponibles | Campos poblados / campos esperados por zona geografica cubierta |
| Tiempo de investigacion de zona | <5 minutos por propiedad | Medicion con equipo de operaciones antes/despues |
| Costo mensual de APIs externas | <$500 USD/mes | Monitoreo de consumo de APIs (Google, proveedores comerciales) |

---

## Entregables Clave

| Entregable | Descripcion | Estado |
|-----------|-------------|--------|
| Conector INEGI | Pipeline para descargar y procesar datos del Censo Economico, DENUE y datos por AGEB (demografia, empleo, NSE) | 🔴 |
| Conector Google Maps/Places | Integracion con Google Places API para obtener POIs cercanos a cada propiedad (comercios, transporte, servicios) | 🔴 |
| Conector de datos catastrales | Pipeline para integrar datos de valor catastral y uso de suelo de fuentes municipales/estatales | 🔴 |
| Conector de indicadores economicos | Integracion de datos macroeconomicos y de actividad industrial por region (INEGI, Banxico, IMSS) | 🔴 |
| Workflows de enriquecimiento | Procesos automatizados que, al ingresar una propiedad nueva, ejecutan el enriquecimiento con todas las fuentes disponibles | 🔴 |
| Monitor de costos de API | Dashboard para rastrear consumo y costos de APIs de terceros con alertas de umbral | 🔴 |

---

## Dependencias

### Necesita (upstream)
- **Base de datos (Arquitectura)** → Esquema extendido para datos de enriquecimiento, tablas de zonas, POIs y demograficos
- **Scraper** → Proporciona los registros base de propiedades con ubicacion (lat/lng) que sirven como ancla para el enriquecimiento

### Depende de este (downstream)
- **Market Intelligence** → Utiliza datos demograficos y economicos para generar reportes y analisis de mercado
- **Geospatial** → Consume datos de zonas INEGI, POIs y limites catastrales para visualizacion en mapa

---

## Riesgos Clave

| # | Riesgo | Impacto | Probabilidad | Mitigacion |
|---|--------|---------|--------------|------------|
| 1 | Costos de API de Google Maps/Places que excedan el presupuesto conforme crece la base de datos | Alto | Media | Implementar cache agresivo, limitar llamadas por zona (no por propiedad individual), monitoreo de costos con alertas |
| 2 | Calidad irregular de datos gubernamentales (INEGI desactualizado, catastros incompletos) | Medio | Alta | Implementar validacion de calidad por fuente, indicadores de confianza, y fechas de ultima actualizacion visibles al usuario |
| 3 | Rate limits en APIs externas que impidan enriquecimiento en tiempo real | Medio | Media | Procesar enriquecimiento en batch nocturno, implementar colas con reintentos y backoff exponencial |
| 4 | Cambios en formatos de datos del INEGI o APIs gubernamentales sin previo aviso | Medio | Media | Versionamiento de conectores, tests automatizados de formato, alertas de fallo de parsing |
| 5 | Dependencia de un solo proveedor para datos criticos (e.g., Google para POIs) | Bajo | Baja | Evaluar fuentes alternativas (OpenStreetMap, Foursquare) como respaldo |

---

## Documentos del Modulo

- [Product Questions](./Product-Questions.md) — Cuestionario de discovery
- [Requirements](./Requirements.md) — Capacidades y criterios de aceptacion
- [Research/](./Research/) — Investigacion tecnica
