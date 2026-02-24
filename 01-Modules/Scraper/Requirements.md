# Requerimientos: Scraper

**Estado**: 🟡 En diseño

> **Prioridades**: **MUST** = Imprescindible | **SHOULD** = Importante pero no bloqueante | **COULD** = Deseable si hay tiempo

---

## Capacidades

### Extracción de Datos

- **MUST** Extraer listings de múltiples portales inmobiliarios configurables
  - [ ] Soporta al menos 3 portales diferentes
  - [ ] Cada portal es configurable de forma independiente
  - [ ] Permite agregar nuevos portales sin cambiar código core
- **MUST** Extraer campos obligatorios de cada listing: título, precio (+ moneda), tipo de operación (renta/venta), dirección, coordenadas (si disponible), superficie (m²), tipo de inmueble, descripción, URL original, ID original, fecha de scraping
- **SHOULD** Extraer campos opcionales: imágenes, contacto del broker (nombre, teléfono, email), características/amenidades, fecha de publicación

### Filtros de Búsqueda

- **MUST** Filtrar por tipo de inmueble (industrial, comercial, oficinas)
  - [ ] Tipo de operación (renta, venta)
  - [ ] Zona geográfica (estado, ciudad, colonia)
  - [ ] Rango de precio
  - [ ] Rango de superficie

### Normalización de Datos

- **MUST** Normalizar datos de diferentes portales a formato unificado
  - [ ] Direcciones a formato estándar
  - [ ] Precios a moneda base (MXN)
  - [ ] Superficies a metros cuadrados
  - [ ] Tipos de inmueble a catálogo interno
  - [ ] Coordenadas a WGS84

### Detección de Cambios

- **MUST** Detectar propiedades nuevas, modificadas y eliminadas entre ejecuciones
  - [ ] Identifica propiedades nuevas (no existían en la corrida anterior)
  - [ ] Identifica propiedades modificadas (datos diferentes)
  - [ ] Identifica propiedades que desaparecieron (posiblemente vendidas/rentadas)
  - [ ] Registra historial de cambios

### Ejecución y Programación

- **SHOULD** Ejecutarse automáticamente según programación configurable
  - [ ] Permite configurar frecuencia (diaria, semanal, etc.)
  - [ ] Permite configurar hora de ejecución
  - [ ] Permite ejecución manual bajo demanda
  - [ ] Registra log de cada ejecución

### Deduplicación

- **SHOULD** Detectar y unificar propiedades duplicadas entre portales
  - [ ] Detecta misma propiedad publicada en múltiples portales
  - [ ] Unifica registros preservando la mejor información de cada fuente
  - [ ] Mantiene referencia a todas las fuentes originales

### Geocodificación

- **SHOULD** Geocodificar propiedades sin coordenadas
  - [ ] Geocodifica direcciones que no tienen lat/long
  - [ ] Marca nivel de confianza del geocoding
  - [ ] Permite corrección manual de coordenadas incorrectas

### Resiliencia y Rate Limiting

- **SHOULD** Manejar errores sin detener la ejecución completa
  - [ ] Si falla un portal, continúa con los demás
  - [ ] Si falla un listing, continúa con los demás
  - [ ] Registra errores con detalle suficiente para debug
- **SHOULD** Respetar límites de velocidad de los portales
  - [ ] Delay configurable entre requests
  - [ ] Respeto de robots.txt (configurable)
  - [ ] Límite máximo de requests por minuto/hora
  - [ ] Backoff exponencial ante errores 429
- **COULD** Notificar errores críticos (portal completo inaccesible)

### Rendimiento

- **SHOULD** Procesar al menos 10,000 listings por ejecución
- **SHOULD** Completar ejecución en máximo 4 horas
- **SHOULD** Poder re-ejecutarse sin generar duplicados si falla a mitad
- **COULD** Ejecutarse en horarios de bajo tráfico (noche)

---

## Preguntas Abiertas

1. ¿Qué tan estrictos debemos ser con robots.txt?
2. ¿Qué hacer con propiedades que desaparecen? ¿Marcar como vendidas?
3. ¿Almacenamos las imágenes o solo las URLs?
4. ¿Qué pasa si el portal cambia su estructura HTML?
