# Factibilidad del MVP -- Decisión GO / NO-GO

**Fase**: R-3 (Validación y Factibilidad)
**Estado**: 🔴 Por completar (requiere hallazgos de todas las fases anteriores)
**Depende de**: Architecture Validation, Total Budget, todos los hallazgos de R-1 y R-2

---

## Objetivo

Consolidar todos los hallazgos de investigación en una decisión clara de GO / NO-GO para el MVP de BEIQA, con condiciones, riesgos, y plan de mitigación.

---

## Criterios de Decisión

| # | Criterio | Peso | Estado | Evaluación |
|---|---------|------|--------|------------|
| 1 | **Presupuesto viable** | Alto | 🔴 | ¿El costo total cabe en $50K-$100K? |
| 2 | **Timeline viable** | Alto | 🔴 | ¿Se puede entregar MVP en 1-2 meses post-contratación? |
| 3 | **Datos accesibles** | Alto | 🔴 | ¿EasyBroker API provee datos suficientes para MVP? |
| 4 | **Deduplicación factible** | Alto | 🔴 | ¿Se pueden deduplicar propiedades con precisión aceptable? |
| 5 | **Stack decidido** | Medio | 🔴 | ¿Se tomaron todas las decisiones tecnológicas? |
| 6 | **Equipo contratable** | Medio | 🔴 | ¿Se puede contratar el equipo necesario? |
| 7 | **Diferenciación clara** | Medio | 🔴 | ¿BEIQA ofrece algo que no exista? |
| 8 | **Riesgos manejables** | Alto | 🔴 | ¿Los riesgos identificados tienen mitigación? |

---

## Evaluación por Criterio

### 1. Presupuesto Viable

| Aspecto | Hallazgo |
|---------|----------|
| Costo estimado total año 1 | 🔴 _Ver [Total-Budget.md](./Total-Budget.md)_ |
| ¿Cabe en $50K-$100K? | 🔴 _Preliminar: Sí ($36K-$72K estimado)_ |
| Riesgo | Scope creep, costos de desarrollo mayores a estimados |
| Mitigación | MVP estricto, equipo con precio fijo por entregable |

### 2. Timeline Viable

| Aspecto | Hallazgo |
|---------|----------|
| Tiempo estimado de desarrollo MVP | 🔴 _2-4 meses post-contratación_ |
| Tiempo de contratación | 🔴 _2-4 semanas para outsource_ |
| Timeline total | 🔴 _3-5 meses desde decisión GO_ |
| Riesgo | Puede exceder el target de 1-2 meses |
| Mitigación | Empezar con scope mínimo absoluto |

### 3. Datos Accesibles

| Aspecto | Hallazgo |
|---------|----------|
| EasyBroker API | 🔴 _Por validar cobertura y calidad_ |
| INEGI DENUE | 🔴 _API gratuita disponible, por probar_ |
| INEGI Cartografía | 🔴 _Shapefiles descargables, por probar_ |
| Riesgo | EasyBroker API podría no tener suficientes datos para CDMX C/I |
| Mitigación | Extensión Chrome como plan B |

### 4. Deduplicación Factible

| Aspecto | Hallazgo |
|---------|----------|
| Enfoque propuesto | Multi-field scoring + LLM para ambiguos |
| ¿Hay herramientas? | Sí (dedupe, splink) |
| ¿El costo de LLM es viable? | 🔴 _Estimado < $1/mes_ |
| Riesgo | Precisión insuficiente sin datos reales de prueba |
| Mitigación | PoC con muestra real antes de construir |

### 5. Stack Decidido

| Decisión | Estado |
|----------|--------|
| ADR-001 Backend Language | 🔴 |
| ADR-002 API Framework | 🔴 |
| ADR-003 Interface | 🔴 |
| ADR-004 Cloud Provider | 🔴 |
| ADR-005 DB Hosting | 🔴 |
| ADR-006 Scheduling | 🔴 |

_Referencia: [Tech-Stack-Decision.md](../02-Architecture/Tech-Stack-Decision.md)_

### 6. Equipo Contratable

| Aspecto | Hallazgo |
|---------|----------|
| Roles necesarios | Backend dev (Python o Node), Frontend dev |
| Mercado LatAm | 🔴 _Por explorar (Toptal, UpWork, referencias)_ |
| Rango de costos | $40-70/hr outsource LatAm |
| Riesgo | Dificultad para encontrar experiencia en GIS/PostGIS |
| Mitigación | Buscar devs con experiencia en data, GIS se aprende |

### 7. Diferenciación Clara

| Aspecto | Hallazgo |
|---------|----------|
| ¿Qué ofrece BEIQA que no existe? | 🔴 _Depende de Competitive Landscape findings_ |
| Diferenciación potencial | Plataforma interna + dedup + geoespacial + market intel |
| Riesgo | Solili/SiiLA podrían cubrir parte de la necesidad |
| Mitigación | Enfocarse en lo que es ÚNICO para Beiqa |

### 8. Riesgos Manejables

| Riesgo | Probabilidad | Impacto | Mitigación |
|--------|-------------|---------|------------|
| EasyBroker API insuficiente | Media | Alto | Plan B: Chrome extension |
| Dedup precision baja | Media | Alto | PoC antes de build |
| Scope creep | Alta | Alto | MVP estricto, sprints cortos |
| Equipo inadecuado | Media | Alto | Período de prueba, contrato por entregable |
| Costos exceden presupuesto | Baja | Alto | Buffer 20%, scope flexible |
| Tecnología incorrecta | Baja | Muy alto | Research-first (este proceso) |
| Equipo no adopta la plataforma | Media | Muy alto | UX testing temprano, involucrar equipo |

---

## Definición del MVP Mínimo Absoluto

Si se decide GO, el MVP mínimo incluiría:

| Componente | Descripción | ¿Imprescindible? |
|------------|-------------|-------------------|
| Data acquisition | Ingestión de EasyBroker (API o extensión) | ✅ |
| Deduplication | Multi-field scoring (puede ser sin IA para v1) | ✅ |
| Database | PostgreSQL + PostGIS con propiedades, zonas | ✅ |
| Mapa | Visualización de propiedades en mapa | ✅ |
| Búsqueda | Filtrar por tipo, zona, precio, superficie | ✅ |
| Interfaz | Mínimo viable (low-code o web simple) | ✅ |
| INEGI zonas | Polígonos CDMX cargados en PostGIS | ✅ |
| Market metrics | Precio/m2 por zona (básico) | ⚠️ Nice-to-have |
| HubSpot sync | Sincronización con CRM | ❌ Post-MVP |
| Tenant portal | Portal para clientes | ❌ Post-MVP |
| AI chatbot | Interfaz conversacional | ❌ Post-MVP |
| Multi-portal | Scraping de otros portales | ❌ Post-MVP |

---

## Decisión

### GO / NO-GO

🔴 **PENDIENTE** -- Requiere completar la investigación de R-1 y R-2 para tomar esta decisión con evidencia.

### Condiciones para GO

1. EasyBroker API provee datos suficientes para CDMX comercial/industrial
2. Stack tecnológico decidido y cotizado
3. Equipo de desarrollo identificado
4. Presupuesto total confirmado dentro de rango
5. Stakeholders alineados en scope del MVP

### Condiciones para NO-GO o PAUSE

1. EasyBroker API no funciona y no hay plan B viable
2. Costos exceden $100K significativamente
3. No se puede contratar equipo adecuado en timeline
4. Competitive analysis revela que una herramienta existente cubre >80% de necesidades

---

## Próximos Pasos (Después de Completar Investigación)

1. [ ] Completar evaluación de cada criterio con datos reales
2. [ ] Presentar hallazgos a stakeholders
3. [ ] Tomar decisión GO / NO-GO
4. [ ] Si GO: iniciar contratación de equipo
5. [ ] Si GO: crear plan de desarrollo detallado (sprints)
6. [ ] Si PAUSE: definir qué investigación adicional se necesita
