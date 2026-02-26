# Estrategia de Adquisicion de Datos — Decisiones Tomadas

**Fase**: Fase 1 — MVP
**Estado**: 🟢 Definido
**Ultima actualizacion**: 2026-02-25

---

## Resumen Ejecutivo

Se han seleccionado 4 portales para el MVP del scraper, cada uno con su herramienta y frecuencia definida. Las decisiones estan tomadas con base en pruebas reales y la experiencia operativa del equipo.

---

## Portales Seleccionados

### 1. Inmuebles24 (inmuebles24.com)

| Aspecto | Detalle |
|---------|---------|
| **Tipo** | Portal inmobiliario multi-broker (el mas grande de Mexico) |
| **Herramienta** | Apify (actor pre-construido) |
| **Procesamiento** | Clay (transitorio → migrara a TriggerDev) |
| **Frecuencia** | Mensual |
| **Volumen estimado** | ~30-40k propiedades comerciales/industriales |
| **Estado** | ✅ En produccion |
| **Datos de broker** | Empresa/inmobiliaria anunciante + telefono (NO contactos individuales) |

**Por que Apify**: Existe un actor pre-construido confiable para Inmuebles24. El volumen es alto (~30-40k), y Apify maneja la complejidad del scraping (anti-bot, paginacion, rendering JS) de forma robusta. Ya esta en produccion y validado.

**Futuro**: Migrara de Clay a TriggerDev para el procesamiento post-extraccion. Apify seguira como motor de extraccion.

---

### 2. Pincali (pincali.com)

| Aspecto | Detalle |
|---------|---------|
| **Tipo** | Portal inmobiliario multi-broker |
| **Herramienta** | TriggerDev (scraper custom en TypeScript) |
| **Procesamiento** | TriggerDev (pipeline completo) |
| **Frecuencia** | Mensual |
| **Volumen estimado** | ~30-40k propiedades |
| **Estado** | 🔴 Por desarrollar |
| **Datos de broker** | Empresa/inmobiliaria anunciante + telefono (NO contactos individuales) |

**Por que TriggerDev custom**: Pincali es un portal similar a Inmuebles24 con listados publicos, filtros, paginacion y paginas de detalle. No hay actor pre-construido en Apify, por lo que se desarrollara un scraper custom. TriggerDev permite control total sobre la logica de errores, retries y human-in-the-loop.

**Estructura del sitio**: Portal clasico con busqueda por filtros, paginacion de resultados, y paginas de detalle individuales por propiedad.

---

### 3. CBRE (colliers.com — seccion Mexico)

| Aspecto | Detalle |
|---------|---------|
| **Tipo** | Sitio corporativo de broker internacional |
| **Herramienta** | TriggerDev (scraper custom en TypeScript) |
| **Procesamiento** | TriggerDev (pipeline completo) |
| **Frecuencia** | Semanal |
| **Volumen estimado** | ~cientos de propiedades |
| **Estado** | 🔴 Por desarrollar |
| **Datos de broker** | Agentes individuales + telefono (Contact en HubSpot asociado a Company "CBRE") |

**Por que semanal**: Volumen bajo (cientos) permite ejecucion frecuente sin impacto en costos. Las propiedades corporativas cambian con frecuencia.

**Particularidad**: CBRE es la inmobiliaria — siempre es la misma. Se extraen contactos individuales (agentes) dentro de la empresa.

---

### 4. Colliers (colliers.com/es-mx — seccion Mexico)

| Aspecto | Detalle |
|---------|---------|
| **Tipo** | Sitio corporativo de broker internacional |
| **Herramienta** | TriggerDev (scraper custom en TypeScript) |
| **Procesamiento** | TriggerDev (pipeline completo) |
| **Frecuencia** | Semanal |
| **Volumen estimado** | ~cientos de propiedades |
| **Estado** | 🔴 Por desarrollar |
| **Datos de broker** | Agentes individuales + telefono (Contact en HubSpot asociado a Company "Colliers") |

**Por que semanal**: Mismo razonamiento que CBRE — bajo volumen, cambios frecuentes.

**Particularidad**: Colliers es la inmobiliaria — siempre es la misma. Se extraen contactos individuales (agentes) dentro de la empresa.

---

## Justificacion de Herramientas

### Por que Apify (para Inmuebles24)
- Actor pre-construido validado en produccion
- Maneja anti-bot, rendering JS, y paginacion compleja
- Volumen alto (~30-40k) requiere herramienta robusta
- Costo razonable para el volumen

### Por que TriggerDev (para Pincali, CBRE, Colliers)
- **TypeScript nativo**: desarrollo rapido con tipado, integracion directa con APIs de HubSpot y Supabase
- **Control de errores**: retries automaticos con backoff exponencial, pausas para intervencion humana
- **Human-in-the-loop**: cuando se detecta un cambio de estructura HTML, TriggerDev pausa la ejecucion y notifica al equipo via Slack
- **Cron nativo**: scheduling semanal/mensual integrado
- **Logs y observabilidad**: dashboard de ejecuciones, historico de runs
- Ya se tiene una instancia configurada

### Por que NO N8N
- N8N es bueno para workflows simples de integracion
- La logica del scraper requiere control mas fino: manejo de errores por listing, parsing HTML complejo, logica condicional por portal
- TriggerDev permite escribir logica arbitraria en TypeScript, lo que da flexibilidad total

### Por que Clay es transitorio
- Clay funciona bien hoy para el flujo de Inmuebles24 (enriquecimiento, LLM, geocoding, escritura a HubSpot)
- Pero genera dependencia de un servicio externo con costo por ejecucion
- La logica de Clay se puede replicar en TriggerDev con mas control y menor costo
- Plan: primero sacar los 3 nuevos portales con TriggerDev, despues migrar I24 de Clay a TriggerDev

---

## Comparativo de Portales

| Caracteristica | Inmuebles24 | Pincali | CBRE | Colliers |
|---------------|-------------|---------|------|----------|
| Tipo de sitio | Portal multi-broker | Portal multi-broker | Corporativo | Corporativo |
| Volumen | ~30-40k | ~30-40k | ~cientos | ~cientos |
| Frecuencia | Mensual | Mensual | Semanal | Semanal |
| Complejidad scraping | Alta (anti-bot, JS) | Media (portal clasico) | Baja (sitio corporativo) | Baja (sitio corporativo) |
| API disponible | No (scraping) | No (scraping) | No (scraping) | No (scraping) |
| Broker = empresa fija | No | No | Si (CBRE) | Si (Colliers) |
| Contactos individuales | No | No | Si | Si |
| Herramienta | Apify | TriggerDev | TriggerDev | TriggerDev |
| Destino | HS + Supabase | HS + Supabase | HS + Supabase | HS + Supabase |

---

## Portales Descartados (no incluidos en MVP)

Los siguientes portales se evaluaron pero no se incluyen en esta fase:

| Portal | Razon de exclusion |
|--------|--------------------|
| EasyBroker | Evaluado inicialmente como prioridad por tener API publica. Descartado en favor de los 4 portales actuales que cubren mejor el mercado target |
| Vivanuncios | Bajo volumen de propiedades comerciales/industriales |
| Lamudi | Enfoque mas residencial, poca cobertura industrial |
| Metros Cubicos | Cobertura limitada del segmento target |
| Propiedades.com | No prioritario para MVP |
| Solili (solili.mx) | Portal especializado industrial, posible adicion post-MVP |
| LoopNet Mexico | Cobertura muy baja en Mexico |

---

## Consideraciones Legales

| Aspecto | Postura |
|---------|---------|
| robots.txt | Respetar donde sea razonable, implementar delays conservadores |
| Terminos de uso | Revisar por portal antes de iniciar scraping |
| Rate limiting | Delays aleatorios entre requests, backoff exponencial |
| Datos personales (LFPDPPP) | Solo almacenar datos publicos del listing, no datos personales sensibles |
| Uso de datos | Interno exclusivamente, no reventa |
| Imagenes | Descargar para referencia interna, no redistribucion |

---

## Riesgos por Portal

| Portal | Riesgo principal | Mitigacion |
|--------|-----------------|------------|
| Inmuebles24 | Anti-bot agresivo, estructura cambiante | Apify maneja esto; backup con actualizacion de actor |
| Pincali | Desconocemos anti-bot measures | Investigar en fase de analisis; TriggerDev permite ajustar |
| CBRE | Sitio corporativo puede reestructurarse | Volumen bajo permite verificacion manual rapida; alerta Slack |
| Colliers | Similar a CBRE | Similar mitigacion |

---

## Documentos Relacionados

- [README del Scraper](../README.md) — Estado actual y roadmap
- [Requirements](../Requirements.md) — Capacidades detalladas
- [Data Model](../../../02-Architecture/Database/Data-Model.md) — Esquema de base de datos
- [Deduplication Strategy](../../../02-Architecture/Deduplication-Strategy.md) — Estrategia de deduplicacion
