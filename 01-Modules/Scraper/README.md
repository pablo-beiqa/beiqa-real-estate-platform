# Scraper

**Fase del proyecto**: Fase 1 — MVP
**Estado**: 🟡 En diseño
**Owner**: Por definir

---

## Descripcion

El modulo de Scraper es el motor de captura automatizada de listados inmobiliarios comerciales e industriales en Mexico. Se encarga de extraer informacion de propiedades publicadas en los principales portales inmobiliarios del pais — Inmuebles24, EasyBroker, Propiedades.com, entre otros — de forma periodica y sistematica. Cada ejecucion recorre las paginas de resultados de estos portales, extrae los datos clave de cada propiedad (superficie, precio, ubicacion, tipo, contacto del broker) y los almacena en la base de datos centralizada de BEIQA.

Actualmente el equipo de BEIQA dedica horas significativas a buscar propiedades manualmente en multiples portales, copiar datos a hojas de calculo y mantener esa informacion actualizada. Este modulo elimina ese trabajo repetitivo, garantizando que la base de datos interna siempre contenga los listados mas recientes del mercado. Ademas, incluye un pipeline de normalizacion que homologa formatos de superficie, moneda, clasificacion de tipo de inmueble y ubicacion, permitiendo que los datos sean comparables sin importar el portal de origen.

---

## Objetivos

1. Automatizar la captura de propiedades comerciales/industriales desde al menos 3 portales inmobiliarios mexicanos, eliminando la busqueda manual diaria.
2. Reducir en un 50% el tiempo que el equipo dedica a la busqueda y registro de propiedades nuevas en el mercado.
3. Mantener la base de datos con informacion fresca: al menos el 80% de los listados activos actualizados en los ultimos 7 dias.

---

## Metricas de Exito / KPIs

| Metrica | Target | Como se mide |
|---------|--------|--------------|
| Propiedades capturadas | >5,000 listados activos en base de datos | Conteo en BD con filtro de status activo |
| Portales soportados | >=3 portales en produccion | Conteo de scrapers activos con ejecucion exitosa en ultimas 48h |
| Frescura de datos | >=80% de listados actualizados en ultimos 7 dias | Query: (listados con last_scraped < 7 dias) / (total listados activos) |
| Tasa de exito de ejecucion | >=95% de ejecuciones sin error critico | Logs del scheduler: ejecuciones exitosas / total ejecuciones |
| Tiempo de busqueda manual ahorrado | Reduccion del 50% vs baseline | Encuesta al equipo antes/despues de implementacion |

---

## Entregables Clave

| Entregable | Descripcion | Estado |
|-----------|-------------|--------|
| Scrapers por portal | Modulos de extraccion especificos para Inmuebles24, EasyBroker y al menos un portal adicional, con manejo de paginacion y detalle | 🔴 |
| Pipeline de normalizacion | Proceso que homologa superficies (m2), precios (MXN/USD), tipos de inmueble, y ubicaciones a un esquema unificado | 🔴 |
| Scheduler de ejecucion | Sistema de programacion de ejecuciones periodicas (cron/Celery) con reintentos y manejo de fallos | 🔴 |
| Dashboard de ejecucion | Panel donde el equipo puede ver estado de los scrapers, ultima ejecucion, errores, y estadisticas de captura | 🔴 |
| Detector de duplicados | Logica para identificar la misma propiedad publicada en multiples portales y consolidarla en un solo registro | 🔴 |

---

## Dependencias

### Necesita (upstream)
- **Base de datos (Arquitectura)** → Esquema de tablas para propiedades, historial de precios, y metadatos de ejecucion
- **Servicio de geocodificacion** → Para normalizar direcciones y obtener coordenadas lat/lng de cada propiedad

### Depende de este (downstream)
- **Internal App** → Consume los listados capturados para mostrarlos al equipo de BEIQA
- **Market Intelligence** → Usa datos historicos de precios y disponibilidad para generar analisis de mercado
- **AI Brain** → Utiliza las descripciones y atributos de propiedades para matching y NLP

---

## Riesgos Clave

| # | Riesgo | Impacto | Probabilidad | Mitigacion |
|---|--------|---------|--------------|------------|
| 1 | Cambios en HTML/estructura de los portales que rompan los scrapers | Alto | Alta | Implementar alertas de fallo, tests de estructura, y modularizar selectores para facilitar actualizacion rapida |
| 2 | Restricciones legales o de terminos de uso de los portales (robots.txt, cease & desist) | Alto | Media | Respetar robots.txt, implementar rate limiting conservador, evaluar APIs oficiales donde existan |
| 3 | Rate limiting o bloqueo de IP por parte de los portales | Medio | Alta | Rotacion de user-agents, delays aleatorios entre requests, uso de proxies si es necesario |
| 4 | Datos inconsistentes o incompletos entre portales | Medio | Alta | Pipeline de validacion robusto, campos obligatorios vs opcionales, logs de calidad de datos |
| 5 | Escalabilidad del scheduler conforme se agreguen portales y zonas | Bajo | Media | Disenar arquitectura de colas desde el inicio (Celery/Redis), ejecucion por lotes |

---

## Documentos del Modulo

- [Product Questions](./Product-Questions.md) — Cuestionario de discovery
- [Requirements](./Requirements.md) — Capacidades y criterios de aceptacion
- [Research/](./Research/) — Investigacion tecnica
