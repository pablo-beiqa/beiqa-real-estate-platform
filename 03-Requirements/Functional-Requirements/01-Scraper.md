# Requerimientos Funcionales: Módulo Scraper

## Descripción del Módulo

El Scraper es responsable de la recolección automatizada de propiedades desde portales inmobiliarios externos.

**Estado**: 🔴 Por completar

---

## Objetivos

1. Recolectar propiedades comerciales e industriales de portales inmobiliarios
2. Mantener datos actualizados con ejecuciones periódicas
3. Detectar propiedades nuevas, modificadas y eliminadas
4. Normalizar datos heterogéneos en un formato unificado

---

## Requerimientos Funcionales

### RF-SCR-001: Extracción de Portales

| ID | Descripción |
|----|-------------|
| RF-SCR-001 | El sistema debe poder extraer listings de múltiples portales inmobiliarios configurables |

**Criterios de aceptación**:
- [ ] Soporta al menos 3 portales diferentes
- [ ] Cada portal es configurable de forma independiente
- [ ] Permite agregar nuevos portales sin cambiar código core

**Prioridad**: Alta

---

### RF-SCR-002: Datos a Extraer

| ID | Descripción |
|----|-------------|
| RF-SCR-002 | El scraper debe extraer los siguientes campos de cada listing |

**Campos obligatorios**:
| Campo | Tipo | Descripción |
|-------|------|-------------|
| Título | String | Nombre del listing |
| Precio | Number | Precio de renta/venta |
| Moneda | Enum | MXN, USD |
| Tipo de operación | Enum | Renta, Venta, Ambos |
| Dirección | String | Ubicación textual |
| Coordenadas | Point | Lat/Long (si disponible) |
| Superficie | Number | Metros cuadrados |
| Tipo de inmueble | Enum | Bodega, Oficina, Local, etc. |
| Descripción | Text | Descripción completa |
| URL original | URL | Link al listing |
| ID original | String | ID único en el portal |
| Fecha de scraping | Datetime | Cuándo se extrajo |

**Campos opcionales**:
| Campo | Tipo | Descripción |
|-------|------|-------------|
| Imágenes | Array[URL] | Fotos del inmueble |
| Contacto | String | Nombre del broker |
| Teléfono | String | Teléfono de contacto |
| Email | String | Email de contacto |
| Características | Array | Amenidades, features |
| Fecha publicación | Date | Cuándo se publicó |

**Prioridad**: Alta

---

### RF-SCR-003: Filtros de Búsqueda

| ID | Descripción |
|----|-------------|
| RF-SCR-003 | El scraper debe poder filtrar por tipo de inmueble y zona |

**Filtros soportados**:
- [ ] Tipo de inmueble (industrial, comercial, oficinas)
- [ ] Tipo de operación (renta, venta)
- [ ] Zona geográfica (estado, ciudad, colonia)
- [ ] Rango de precio
- [ ] Rango de superficie

**Prioridad**: Alta

---

### RF-SCR-004: Ejecución Programada

| ID | Descripción |
|----|-------------|
| RF-SCR-004 | El scraper debe ejecutarse automáticamente según programación |

**Criterios de aceptación**:
- [ ] Permite configurar frecuencia (diaria, semanal, etc.)
- [ ] Permite configurar hora de ejecución
- [ ] Permite ejecución manual bajo demanda
- [ ] Registra log de cada ejecución

**Prioridad**: Media

---

### RF-SCR-005: Detección de Cambios

| ID | Descripción |
|----|-------------|
| RF-SCR-005 | El sistema debe detectar propiedades nuevas, modificadas y eliminadas |

**Criterios de aceptación**:
- [ ] Identifica propiedades que no existían en la corrida anterior (nuevas)
- [ ] Identifica propiedades con datos diferentes (modificadas)
- [ ] Identifica propiedades que ya no aparecen (posiblemente vendidas/rentadas)
- [ ] Registra historial de cambios

**Prioridad**: Alta

---

### RF-SCR-006: Normalización de Datos

| ID | Descripción |
|----|-------------|
| RF-SCR-006 | Los datos de diferentes portales deben normalizarse a un formato unificado |

**Normalizaciones requeridas**:
- [ ] Direcciones a formato estándar
- [ ] Precios a moneda base (MXN)
- [ ] Superficies a metros cuadrados
- [ ] Tipos de inmueble a catálogo interno
- [ ] Coordenadas a WGS84

**Prioridad**: Alta

---

### RF-SCR-007: Manejo de Errores

| ID | Descripción |
|----|-------------|
| RF-SCR-007 | El scraper debe manejar errores sin detener la ejecución completa |

**Criterios de aceptación**:
- [ ] Si falla un portal, continúa con los demás
- [ ] Si falla un listing, continúa con los demás
- [ ] Registra errores con detalle suficiente para debug
- [ ] Notifica errores críticos (portal completo inaccesible)

**Prioridad**: Media

---

### RF-SCR-008: Rate Limiting

| ID | Descripción |
|----|-------------|
| RF-SCR-008 | El scraper debe respetar límites de velocidad para no sobrecargar portales |

**Criterios de aceptación**:
- [ ] Delay configurable entre requests
- [ ] Respeto de robots.txt (configurable)
- [ ] Límite máximo de requests por minuto/hora
- [ ] Backoff exponencial ante errores 429

**Prioridad**: Media

---

### RF-SCR-009: Deduplicación

| ID | Descripción |
|----|-------------|
| RF-SCR-009 | El sistema debe detectar y unificar propiedades duplicadas entre portales |

**Criterios de aceptación**:
- [ ] Detecta misma propiedad publicada en múltiples portales
- [ ] Unifica registros preservando la mejor información de cada fuente
- [ ] Mantiene referencia a todas las fuentes originales

**Prioridad**: Media

---

### RF-SCR-010: Geocodificación

| ID | Descripción |
|----|-------------|
| RF-SCR-010 | Las propiedades sin coordenadas deben geocodificarse |

**Criterios de aceptación**:
- [ ] Geocodifica direcciones que no tienen lat/long
- [ ] Marca nivel de confianza del geocoding
- [ ] Permite corrección manual de coordenadas incorrectas

**Prioridad**: Media

---

## Requerimientos No Funcionales

### RNF-SCR-001: Rendimiento

- Debe poder procesar al menos 10,000 listings por ejecución
- Tiempo máximo de ejecución: 4 horas

### RNF-SCR-002: Disponibilidad

- El scraper puede ejecutarse en horarios de bajo tráfico (noche)
- Si falla una ejecución, debe poder re-ejecutarse sin duplicados

### RNF-SCR-003: Logs y Auditoría

- Toda ejecución debe quedar registrada
- Logs suficientes para debugging

---

## Inputs

| Input | Fuente | Formato |
|-------|--------|---------|
| Configuración de portales | Archivo config / DB | JSON |
| Filtros de búsqueda | Configuración | JSON |
| Programación | Cron / Scheduler | Cron expression |

---

## Outputs

| Output | Destino | Formato |
|--------|---------|---------|
| Propiedades | Base de datos | Registros normalizados |
| Log de ejecución | Archivo / DB | Texto estructurado |
| Métricas | Monitoring | Conteos, tiempos |

---

## Dependencias

- Base de datos central (para almacenar resultados)
- Servicio de geocodificación (para propiedades sin coordenadas)
- Sistema de notificaciones (para alertas de errores)

---

## Preguntas Abiertas

1. ¿Qué tan estrictos debemos ser con robots.txt?
2. ¿Qué hacer con propiedades que desaparecen? ¿Marcar como vendidas?
3. ¿Almacenamos las imágenes o solo las URLs?
4. ¿Qué pasa si el portal cambia su estructura HTML?

---

## Casos de Uso

### UC-SCR-001: Ejecución Programada

```
Actor: Sistema (cron)
Precondición: Configuración de portales definida
Flujo:
1. El scheduler dispara el scraper
2. El scraper itera sobre cada portal configurado
3. Para cada portal, extrae listings según filtros
4. Normaliza datos y almacena en BD
5. Genera reporte de ejecución
Postcondición: Datos actualizados en BD, log generado
```

### UC-SCR-002: Ejecución Manual

```
Actor: Usuario interno de Beiqa
Precondición: Usuario tiene permisos de admin
Flujo:
1. Usuario accede a la interfaz de admin
2. Selecciona portales a scrapear
3. Opcionalmente ajusta filtros
4. Inicia ejecución
5. Visualiza progreso en tiempo real
6. Recibe notificación al completar
Postcondición: Datos actualizados, usuario notificado
```

---

## Criterios de Aceptación del Módulo

- [ ] Extrae datos de al menos 3 portales
- [ ] Datos normalizados correctamente
- [ ] Detección de nuevos y modificados funciona
- [ ] Ejecución programada funcional
- [ ] Logs completos y útiles
- [ ] Sin errores fatales en operación normal
