# Comparativo de Portales Inmobiliarios

**Fase**: R-1 (Datos Core)
**Estado**: 🔴 Por investigar
**Depende de**: [Data Acquisition Strategy](./Data-Acquisition-Strategy.md)

---

## Objetivo

Evaluar cada portal inmobiliario para determinar el **método de adquisición óptimo** (API, scraping, extensión, manual), volumen de datos, y prioridad de integración.

> **Nota**: Este documento se reestructuró para evaluar portales por método de adquisición, no solo por factibilidad de scraping. Ver [Data-Acquisition-Strategy.md](./Data-Acquisition-Strategy.md) para la estrategia general.

---

## Portales a Evaluar

### 1. EasyBroker (PRIORIDAD MVP)

| Criterio | Valor | Notas |
|----------|-------|-------|
| URL | easybroker.com | |
| Especialización | Comercial, profesional | Enfocado en brokers |
| Cobertura geográfica | Nacional | |
| **¿Tiene API oficial?** | **Sí** | REST API pública confirmada |
| Método de adquisición recomendado | **API** | Investigar cobertura y límites |
| Propiedades comerciales/industriales CDMX | 🔴 Por contar | |
| Rate limits de API | 🔴 Por investigar | |
| Campos disponibles via API | 🔴 Por investigar | |
| ¿Incluye coordenadas? | 🔴 Por verificar | |
| ¿Incluye imágenes (URLs)? | 🔴 Por verificar | |
| **Factibilidad** | 🟡 Alta (API disponible) | Pendiente validar cobertura |

**Acciones**:
- [ ] Obtener API key de prueba
- [ ] Documentar endpoints y campos disponibles
- [ ] Hacer request de prueba para CDMX comercial/industrial
- [ ] Contar volumen disponible
- [ ] Evaluar si API cubre necesidades del MVP

---

### 2. Inmuebles24

| Criterio | Valor | Notas |
|----------|-------|-------|
| URL | inmuebles24.com | |
| Tipos de inmuebles | Todos | Residencial, comercial, industrial |
| Cobertura geográfica | Nacional | |
| ¿Tiene API oficial? | 🔴 Por investigar | Probablemente no |
| Método de adquisición recomendado | 🔴 Por determinar | Scraping o extensión Chrome |
| Propiedades comerciales/industriales CDMX | 🔴 Por contar | |
| Estructura HTML | 🔴 Por analizar | |
| Protección anti-bot | 🔴 Por verificar | |
| ¿Incluye coordenadas? | 🔴 Por verificar | |
| Términos de servicio | 🔴 Por revisar | |
| **Factibilidad** | 🔴 Por determinar | |

**Acciones**:
- [ ] Inspeccionar estructura HTML de listings
- [ ] Verificar si hay API interna en Network tab
- [ ] Contar propiedades comerciales/industriales
- [ ] Revisar robots.txt y términos de servicio
- [ ] Probar request simple

---

### 3. Vivanuncios

| Criterio | Valor | Notas |
|----------|-------|-------|
| URL | vivanuncios.com.mx | |
| Tipos de inmuebles | Todos | |
| ¿Tiene API oficial? | 🔴 Por investigar | |
| Método de adquisición recomendado | 🔴 Por determinar | |
| **Factibilidad** | 🔴 Por determinar | |

---

### 4. Lamudi

| Criterio | Valor | Notas |
|----------|-------|-------|
| URL | lamudi.com.mx | |
| Tipos de inmuebles | Todos | |
| ¿Tiene API oficial? | 🔴 Por investigar | |
| Método de adquisición recomendado | 🔴 Por determinar | |
| **Factibilidad** | 🔴 Por determinar | |

---

### 5. Metros Cúbicos

| Criterio | Valor | Notas |
|----------|-------|-------|
| URL | metroscubicos.com | |
| Tipos de inmuebles | Todos | |
| Método de adquisición recomendado | 🔴 Por determinar | |
| **Factibilidad** | 🔴 Por determinar | |

---

### 6. Propiedades.com

| Criterio | Valor | Notas |
|----------|-------|-------|
| URL | propiedades.com | |
| Método de adquisición recomendado | 🔴 Por determinar | |
| **Factibilidad** | 🔴 Por determinar | |

---

### 7. Solili

| Criterio | Valor | Notas |
|----------|-------|-------|
| URL | solili.mx | |
| Especialización | Industrial | Enfoque industrial |
| Método de adquisición recomendado | 🔴 Por determinar | Posiblemente suscripción/datos, no scraping |
| **Factibilidad** | 🔴 Por determinar | Puede ser competidor más que fuente |

---

### 8. LoopNet México

| Criterio | Valor | Notas |
|----------|-------|-------|
| URL | loopnet.com (sección México) | |
| Especialización | Comercial | |
| Método de adquisición recomendado | 🔴 Por determinar | |
| **Factibilidad** | 🔴 Por determinar | Cobertura México probablemente baja |

---

## Matriz Comparativa por Método de Adquisición

| Portal | API | Scraping | Extensión | CSV | Prioridad MVP | # Props C/I CDMX |
|--------|-----|----------|-----------|-----|---------------|-------------------|
| EasyBroker | ✅ Sí | Fallback | Fallback | N/A | 🔴 **#1** | ? |
| Inmuebles24 | ? | ? | ? | ? | 🟡 #2 | ? |
| Vivanuncios | ? | ? | ? | ? | 🟡 | ? |
| Lamudi | ? | ? | ? | ? | 🟢 | ? |
| Metros Cúbicos | ? | ? | ? | ? | 🟢 | ? |
| Propiedades.com | ? | ? | ? | ? | 🟢 | ? |
| Solili | ? | N/A | N/A | ? | 🟡 | ? |
| LoopNet MX | ? | ? | ? | ? | 🟢 | ? |

---

## Campos Comunes a Capturar (Todos los Portales)

| Campo | Descripción | Importancia |
|-------|-------------|-------------|
| Título | Nombre del listing | Alta |
| Precio | Renta y/o venta | Alta |
| Dirección | Ubicación textual | Alta |
| Coordenadas | Lat/Long | Alta (si disponible) |
| Superficie (m²) | Tamaño | Alta |
| Tipo de inmueble | Industrial, comercial, etc. | Alta |
| Descripción | Texto descriptivo | Media |
| Imágenes | URLs de fotos | Media |
| Contacto | Broker/propietario | Media |
| Fecha publicación | Cuándo se publicó | Media |
| URL original | Link al listing | Alta |
| ID original | Identificador del portal | Alta |

### Campos Específicos por Tipo

**Industrial/Bodegas**: Altura de techo, puertas de andén, capacidad de carga de piso, área de maniobras

**Oficinas**: Piso, estacionamientos, edificio inteligente

**Retail**: Frente (metros), ubicación en centro comercial

---

## Consideraciones Legales

### Por Verificar en Cada Portal

- [ ] Términos de servicio - ¿prohíben scraping?
- [ ] robots.txt - ¿qué rutas bloquean?
- [ ] Requerimientos de atribución
- [ ] Datos personales en listings (LFPDPPP)

### Mitigaciones de Riesgo

- **API primero**: Siempre preferir API oficial sobre scraping
- Rate limiting (no sobrecargar servidores)
- Respetar robots.txt donde sea razonable
- No almacenar datos personales de terceros
- Uso interno, no reventa de datos

---

## Próximos Pasos

1. [ ] **EasyBroker API** -- investigar a fondo (PRIORIDAD)
2. [ ] Inmuebles24 -- análisis técnico de adquisición
3. [ ] Revisar términos de servicio de todos los portales
4. [ ] Contar propiedades comerciales/industriales por portal en CDMX
5. [ ] Definir método de adquisición por portal
6. [ ] Crear PoC con EasyBroker API
7. [ ] Decidir portales para MVP vs. post-MVP
