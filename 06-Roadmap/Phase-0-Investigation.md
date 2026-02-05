# Fase 0: Investigación y Diseño

## Objetivo

Validar factibilidad técnica, completar diseño, y tomar decisiones arquitectónicas antes de iniciar desarrollo.

**Duración estimada**: 2-4 semanas  
**Estado**: 🟢 Activa

---

## Semana 1-2: Investigación Técnica

### 1.1 Investigación de Fuentes de Datos

| Tarea | Responsable | Entregable | Estado |
|-------|-------------|------------|--------|
| Analizar estructura HTML de Inmuebles24 | Dev | Documento de análisis | 🔴 |
| Analizar estructura HTML de Vivanuncios | Dev | Documento de análisis | 🔴 |
| Revisar términos de servicio de portales | Legal/PM | Matriz de factibilidad | 🔴 |
| Probar APIs de INEGI | Dev | PoC funcional | 🔴 |
| Evaluar Google Maps Platform pricing | PM | Estimación de costos | 🔴 |
| Investigar proveedores comerciales | PM | Comparativo | 🔴 |

### 1.2 Investigación Geoespacial

| Tarea | Responsable | Entregable | Estado |
|-------|-------------|------------|--------|
| Descargar y probar datos INEGI cartografía | Dev | Datos cargados en PostGIS | 🔴 |
| Evaluar calidad de shapefiles disponibles | Dev | Reporte de calidad | 🔴 |
| Probar queries geoespaciales | Dev | Scripts de prueba | 🔴 |
| Definir capas geoespaciales necesarias | PM | Lista priorizada | 🔴 |

### 1.3 Pruebas de Concepto

| Tarea | Responsable | Entregable | Estado |
|-------|-------------|------------|--------|
| PoC scraper portal prioritario | Dev | Script funcional | 🔴 |
| PoC geocodificación | Dev | Script funcional | 🔴 |
| PoC consultas PostGIS | Dev | Queries de ejemplo | 🔴 |
| PoC integración OpenAI | Dev | Script funcional | 🔴 |

---

## Semana 2-3: Diseño y Decisiones

### 2.1 Decisiones Arquitectónicas (ADRs)

| Decisión | Opciones | Fecha límite | Estado |
|----------|----------|--------------|--------|
| Lenguaje backend | Python vs Node.js | Sem 2 | 🔴 |
| Framework web | FastAPI vs Express vs NestJS | Sem 2 | 🔴 |
| Base de datos | PostgreSQL vs otras | Sem 2 | 🔴 |
| Cloud provider | AWS vs GCP vs Azure | Sem 2 | 🔴 |
| Framework frontend | Next.js vs Remix | Sem 3 | 🔴 |
| Servicio de mapas | Google vs Mapbox vs OSM | Sem 3 | 🔴 |

### 2.2 Diseño Técnico

| Tarea | Responsable | Entregable | Estado |
|-------|-------------|------------|--------|
| Modelo de datos final | Dev | ER diagram + DDL | 🔴 |
| API contracts v1 | Dev | OpenAPI spec | 🔴 |
| Diagrama de componentes | Dev | Documento | 🔴 |
| Estrategia de deployment | Dev | Documento | 🔴 |

### 2.3 Diseño de Producto

| Tarea | Responsable | Entregable | Estado |
|-------|-------------|------------|--------|
| User flows principales | PM/UX | Diagramas | 🔴 |
| Wireframes pantallas clave | PM/UX | Figma/sketches | 🔴 |
| Priorización de features | PM | Lista ordenada | 🔴 |

---

## Semana 3-4: Preparación

### 3.1 Setup del Proyecto

| Tarea | Responsable | Entregable | Estado |
|-------|-------------|------------|--------|
| Setup repositorio(s) | Dev | Repo(s) Git | 🔴 |
| Setup CI/CD básico | Dev | Pipeline funcional | 🔴 |
| Setup ambiente desarrollo | Dev | Docker compose | 🔴 |
| Setup ambiente staging | Dev | Ambiente en cloud | 🔴 |
| Setup herramientas (PM, comunicación) | PM | Accesos configurados | 🔴 |

### 3.2 Estimación y Planificación

| Tarea | Responsable | Entregable | Estado |
|-------|-------------|------------|--------|
| Estimación detallada Fase 1 | Dev + PM | Story points/horas | 🔴 |
| Definición de sprints Fase 1 | PM | Sprint plan | 🔴 |
| Identificación de riesgos | PM | Risk register | 🔴 |
| Presupuesto final | PM | Documento aprobado | 🔴 |

---

## Entregables de Fase 0

Al finalizar esta fase, debemos tener:

### Documentación

- [ ] ADRs documentados para todas las decisiones clave
- [ ] Modelo de datos validado
- [ ] API contracts v1
- [ ] Diagrama de arquitectura actualizado
- [ ] Estimación de costos validada

### Validación Técnica

- [ ] PoC de scraping funcionando
- [ ] PoC de PostGIS funcionando
- [ ] PoC de AI funcionando
- [ ] Claridad sobre APIs disponibles y sus costos

### Preparación

- [ ] Repositorio configurado
- [ ] CI/CD básico funcionando
- [ ] Ambiente de desarrollo listo
- [ ] Equipo alineado en tech stack

---

## Criterios de Salida (Go/No-Go para Fase 1)

### Criterios de GO

| Criterio | Validación |
|----------|------------|
| PoC de scraping exitoso para al menos 1 portal | [ ] |
| Costos de APIs dentro de presupuesto | [ ] |
| Decisiones arquitectónicas tomadas | [ ] |
| Equipo alineado en tech stack | [ ] |
| Estimación de Fase 1 aprobada | [ ] |

### Criterios de NO-GO

| Criterio | Consecuencia |
|----------|--------------|
| Scraping no factible por bloqueos técnicos | Buscar alternativas (APIs, compra de datos) |
| Costos exceden presupuesto significativamente | Reducir alcance o buscar financiamiento |
| Riesgos legales identificados | Consultar con legal antes de continuar |

---

## Riesgos de Fase 0

| Riesgo | Probabilidad | Impacto | Mitigación |
|--------|--------------|---------|------------|
| Portales bloquean scraping | Media | Alto | Evaluar múltiples portales, técnicas de evasión |
| APIs de INEGI no funcionan como esperado | Baja | Medio | Tener alternativas (descarga de archivos) |
| Costos de Google Maps muy altos | Media | Medio | Evaluar alternativas open source |
| Equipo no disponible | Baja | Alto | Asegurar compromisos antes de iniciar |

---

## Notas

- Esta fase es crítica para reducir riesgo en el proyecto
- No iniciar desarrollo sin completar criterios de GO
- Documentar todo para referencia futura
- Mantener comunicación constante con stakeholders
