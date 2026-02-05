# Comparativo de Portales Inmobiliarios

## Objetivo

Evaluar cada portal inmobiliario para determinar factibilidad de scraping, volumen de datos, y prioridad de integración.

**Estado**: 🔴 Por investigar

---

## Portales a Evaluar

### 1. Inmuebles24

| Criterio | Valor | Notas |
|----------|-------|-------|
| URL | inmuebles24.com | |
| Tipos de inmuebles | Todos | Residencial, comercial, industrial |
| Cobertura geográfica | Nacional | |
| ¿Tiene API oficial? | 🔴 Por investigar | |
| Propiedades comerciales/industriales | 🔴 Por contar | |
| Estructura HTML | 🔴 Por analizar | |
| Protección anti-bot | 🔴 Por verificar | |
| ¿Incluye coordenadas? | 🔴 Por verificar | |
| Frecuencia de actualización | 🔴 Por monitorear | |
| Términos de servicio | 🔴 Por revisar | |
| **Factibilidad** | 🔴 Por determinar | |

**Acciones**:
- [ ] Inspeccionar estructura HTML de listings
- [ ] Verificar si hay API interna en Network tab
- [ ] Contar propiedades comerciales/industriales
- [ ] Revisar términos de servicio
- [ ] Probar request simple

---

### 2. Vivanuncios

| Criterio | Valor | Notas |
|----------|-------|-------|
| URL | vivanuncios.com.mx | |
| Tipos de inmuebles | Todos | |
| Cobertura geográfica | Nacional | |
| ¿Tiene API oficial? | 🔴 Por investigar | |
| Propiedades comerciales/industriales | 🔴 Por contar | |
| Estructura HTML | 🔴 Por analizar | |
| Protección anti-bot | 🔴 Por verificar | |
| ¿Incluye coordenadas? | 🔴 Por verificar | |
| **Factibilidad** | 🔴 Por determinar | |

---

### 3. Metros Cúbicos

| Criterio | Valor | Notas |
|----------|-------|-------|
| URL | metroscubicos.com | |
| Tipos de inmuebles | Todos | |
| Cobertura geográfica | Nacional | |
| **Factibilidad** | 🔴 Por determinar | |

---

### 4. Lamudi

| Criterio | Valor | Notas |
|----------|-------|-------|
| URL | lamudi.com.mx | |
| Tipos de inmuebles | Todos | |
| Cobertura geográfica | Nacional | |
| **Factibilidad** | 🔴 Por determinar | |

---

### 5. Propiedades.com

| Criterio | Valor | Notas |
|----------|-------|-------|
| URL | propiedades.com | |
| **Factibilidad** | 🔴 Por determinar | |

---

### 6. EasyBroker

| Criterio | Valor | Notas |
|----------|-------|-------|
| URL | easybroker.com | |
| Especialización | Comercial, profesional | Enfocado en brokers |
| **Factibilidad** | 🔴 Por determinar | |

---

### 7. Solili

| Criterio | Valor | Notas |
|----------|-------|-------|
| URL | solili.mx | |
| Especialización | Industrial | Enfoque industrial |
| **Factibilidad** | 🔴 Por determinar | |

---

### 8. LoopNet México

| Criterio | Valor | Notas |
|----------|-------|-------|
| URL | loopnet.com (sección México) | |
| Especialización | Comercial | |
| **Factibilidad** | 🔴 Por determinar | |

---

## Matriz Comparativa (Por Completar)

| Portal | API | # Props C/I | Anti-bot | Coords | Legal | Prioridad |
|--------|-----|-------------|----------|--------|-------|-----------|
| Inmuebles24 | ? | ? | ? | ? | ? | ? |
| Vivanuncios | ? | ? | ? | ? | ? | ? |
| Metros Cúbicos | ? | ? | ? | ? | ? | ? |
| Lamudi | ? | ? | ? | ? | ? | ? |
| Propiedades.com | ? | ? | ? | ? | ? | ? |
| EasyBroker | ? | ? | ? | ? | ? | ? |
| Solili | ? | ? | ? | ? | ? | ? |
| LoopNet MX | ? | ? | ? | ? | ? | ? |

---

## Estructura de Datos por Portal

### Campos Comunes a Capturar

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

**Industrial/Bodegas**:
- Altura de techo
- Puertas de andén
- Capacidad de carga de piso
- Área de maniobras

**Oficinas**:
- Piso
- Estacionamientos
- Edificio inteligente

**Retail**:
- Frente (metros)
- Ubicación en centro comercial

---

## Herramientas de Scraping a Evaluar

| Herramienta | Tipo | Mejor para | Notas |
|-------------|------|------------|-------|
| Scrapy | Framework Python | Alto volumen, estructurado | |
| Playwright | Browser automation | JavaScript-heavy sites | |
| Puppeteer | Browser automation | Similar a Playwright | |
| BeautifulSoup | Parser | HTML estático simple | |
| Selenium | Browser automation | Sitios complejos | |
| requests-html | Hybrid | Rendering JS simple | |

---

## Consideraciones Legales

### Por Verificar en Cada Portal

- [ ] Términos de servicio - ¿prohíben scraping?
- [ ] robots.txt - ¿qué rutas bloquean?
- [ ] Requerimientos de atribución
- [ ] Datos personales en listings (LFPDPPP)

### Mitigaciones de Riesgo

- Rate limiting (no sobrecargar servidores)
- Respetar robots.txt donde sea razonable
- No almacenar datos personales de terceros
- Uso interno, no reventa de datos

---

## Próximos Pasos

1. [ ] Hacer análisis técnico de Inmuebles24 (prioritario)
2. [ ] Hacer análisis técnico de Vivanuncios
3. [ ] Revisar términos de servicio de todos
4. [ ] Contar propiedades comerciales/industriales por portal
5. [ ] Crear PoC de scraping para el portal más factible
6. [ ] Decidir portales para MVP

---

## Hallazgos

*(Documentar aquí los resultados de la investigación)*
