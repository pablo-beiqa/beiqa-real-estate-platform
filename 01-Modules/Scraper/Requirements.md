# Requerimientos: Scraper

**Estado**: 🟡 En desarrollo parcial

> **Prioridades**: **MUST** = Imprescindible | **SHOULD** = Importante pero no bloqueante | **COULD** = Deseable si hay tiempo

---

## Capacidades

### Extraccion de Datos

- **MUST** Extraer listings de 4+ portales inmobiliarios
  - [x] Inmuebles24 (via Apify — migrando a Trigger.dev+Firecrawl Sprint 1-2)
  - [x] CBRE Mexico (Trigger.dev + Firecrawl, RSC parsing — ✅ producción)
  - [x] Colliers Mexico (Trigger.dev + Firecrawl + Browserbase — ✅ producción)
  - [x] FinSA (Trigger.dev + API directa — ✅ producción)
  - [x] Pincali / pincali.com (Trigger.dev + Firecrawl + Browserbase — ✅ producción, 1,761 props)
- **MUST** Extraer campos obligatorios de cada listing: titulo, precio (+ moneda), tipo de operacion (renta/venta), direccion, coordenadas (si disponible), superficie (m²), tipo de inmueble, descripcion, URL original, ID original, fecha de scraping
- **MUST** Extraer telefono del anunciante y guardarlo directamente en el registro de la propiedad
- **SHOULD** Extraer campos opcionales: imagenes, caracteristicas/amenidades, fecha de publicacion

### Extraccion de Datos del Anunciante

- **MUST** Extraer datos del anunciante diferenciado por tipo de portal:
  - **Inmuebles24 / Pincali** (portales multi-broker):
    - [ ] Nombre de la empresa/inmobiliaria anunciante
    - [ ] Telefono (se guarda en la propiedad + se crea Company en HubSpot)
    - [ ] **NO se crean Contacts individuales** — solo Company
  - **CBRE / Colliers** (portales corporativos):
    - [ ] Nombre del agente/contacto individual
    - [ ] Telefono y datos de contacto (se guarda en propiedad + Contact en HubSpot)
    - [ ] Asociar Contact a Company fija (CBRE o Colliers)

### Parsing de Descripcion con LLM

- **MUST** Extraer campos estructurados desde texto libre de la descripcion usando LLM costo-eficiente (Haiku o GPT-4o-mini)
  - [ ] Superficie en m² (cuando no esta en campos estructurados)
  - [ ] Altura de techo
  - [ ] Seguridad (vigilancia, CCTV, etc.)
  - [ ] Antiguedad del inmueble
  - [ ] Amenidades y caracteristicas especiales
  - [ ] Tipo de anunciante (persona fisica vs empresa)
- **SHOULD** Evaluar costo/calidad del modelo LLM y optimizar prompt

### Filtros de Busqueda

- **MUST** Filtrar por tipo de inmueble (industrial, comercial, oficinas)
  - [ ] Tipo de operacion (renta, venta)
  - [ ] Zona geografica (estado, ciudad, colonia)
  - [ ] Rango de precio
  - [ ] Rango de superficie

### Normalizacion Basica (en pipeline)

- **MUST** Normalizar datos dentro del pipeline antes de guardar
  - [ ] Precios a moneda base (MXN)
  - [ ] Superficies a metros cuadrados
  - [ ] Tipos de inmueble a catalogo interno
  - [ ] Coordenadas a WGS84

> **Nota**: La normalizacion profunda, deduplicacion cross-portal y geocodificacion batch son responsabilidad del modulo [Data / Normalization](../Data/Normalization/).

### Geocodificacion (en pipeline)

- **MUST** Geocodificar propiedades sin coordenadas dentro del pipeline de TriggerDev
  - [ ] Integrar Google Geocoding API
  - [ ] Cache de resultados para evitar llamadas duplicadas
  - [ ] Marcar nivel de confianza del geocoding
- **SHOULD** Asignar H3 hexagono y AGEB a cada propiedad geocodificada

### Deteccion de Anomalias

- **MUST** Validar datos antes de guardar y detectar anomalias
  - [ ] Coordenadas fuera de rango de Mexico
  - [ ] Precios fuera de rangos logicos por tipo de propiedad
  - [ ] Superficie inconsistente (valor imposible o muy diferente a descripcion)
  - [ ] Datos del anunciante inconsistentes (persona vs empresa)
- **MUST** Flaggear anomalias para revision humana
- **SHOULD** Enviar alerta a Slack cuando se detectan anomalias criticas

### Deteccion de Cambios

- **MUST** Detectar propiedades nuevas, modificadas y no encontradas entre ejecuciones
  - [ ] Identifica propiedades nuevas
  - [ ] Identifica propiedades con datos modificados
  - [ ] Identifica propiedades que desaparecieron — marcar como no disponible de inmediato

### Escritura Dual (HubSpot + Supabase)

- **MUST** Escribir propiedades a Supabase y HubSpot
  - [ ] Verificar existencia en ambos destinos antes de crear
  - [ ] Crear propiedad nueva en ambos con asociaciones
  - [ ] Actualizar propiedad existente en ambos si hay cambios
  - [ ] Marcar como no disponible en ambos si no se encontro
- **MUST** Escribir inmobiliaria/broker segun tipo de portal
  - [ ] I24/Pincali: crear/asociar Company en HubSpot
  - [ ] CBRE/Colliers: crear/asociar Contact + Company en HubSpot
- **SHOULD** Definir orden de escritura y manejo de fallos parciales

### Imagenes

- **MUST** Descargar imagenes de cada propiedad
  - [x] Upload a Supabase Storage (CBRE: cbre-images/cbre-pdfs, Colliers: colliers-images/colliers-pdfs, FINSA: finsa-flyers)
  - [x] Asociar URLs de storage a la propiedad en Supabase (image_storage_urls, pdf_storage_urls, flyer_url)
- **SHOULD** Mantener referencia a URL original como fallback

### Ejecucion y Programacion

- **MUST** Ejecutarse automaticamente via TriggerDev cron
  - [x] CBRE: cron martes 6am UTC (semanal)
  - [x] Colliers: cron lunes 6am UTC (semanal)
  - [x] FINSA: cron día 1 y 15, 5am UTC (bimensual)
  - [x] Pincali: cron lunes 7am UTC (semanal) — ✅ con persist
  - [ ] Inmuebles24: migrando de Apify a Trigger.dev (Sprint 1-2)
- **SHOULD** Permitir ejecucion manual bajo demanda desde TriggerDev
- **MUST** Registrar log de cada ejecucion en el repo (EXECUTION-LOG.md)

### Notificaciones

- **MUST** Enviar notificaciones a Slack (canal existente) en caso de:
  - [ ] Error critico que detiene la ejecucion
  - [ ] Anomalias que requieren revision humana
  - [ ] Portal inaccesible o cambio de estructura detectado
- **SHOULD** Enviar resumen de ejecucion al finalizar (propiedades nuevas, actualizadas, errores)

### Resiliencia y Rate Limiting

- **MUST** Manejar errores sin detener la ejecucion completa
  - [ ] Si falla un listing, continua con los demas
  - [ ] Registra errores con detalle suficiente para debug
- **MUST** TriggerDev maneja retries con backoff exponencial (✅ configurado: max 3 intentos, 1-10s con jitter. FINSA: 10-60s)
- **MUST** Pausar ejecucion y solicitar intervencion humana cuando hay cambios de estructura
- **SHOULD** Respetar limites de velocidad de los portales
  - [ ] Delay configurable entre requests
  - [ ] Backoff exponencial ante errores 429

### Rendimiento

- **SHOULD** Procesar portales grandes (30-40k en Pincali/I24) en lotes manejables
- **SHOULD** Completar portales pequenos (CBRE/Colliers, cientos) en menos de 30 minutos
- **MUST** Poder re-ejecutarse sin generar duplicados si falla a mitad (✅ idempotencyKey basado en fecha: `{slug}-{YYYY-MM-DD}`)

---

## Preguntas Abiertas

1. ~~¿Almacenamos las imagenes o solo las URLs?~~ → **Decidido**: descargar y almacenar en Supabase Storage
2. ~~¿Que hacer con propiedades que desaparecen?~~ → **Decidido**: marcar como no disponible de inmediato
3. ¿Que pasa si el portal cambia su estructura HTML? → TriggerDev pausa y notifica a Slack para intervencion humana
4. ¿Orden de dual-write (Supabase primero o HubSpot primero)? → Por definir
5. ¿Que Google API usar para geocodificacion? → Por investigar (Geocoding API vs Places API vs alternativas)
