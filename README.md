# BEIQA Real Estate Platform

> Plataforma tecnológica interna que centraliza la búsqueda de inmuebles comerciales/industriales, inteligencia de mercado y gestión de clientes para el equipo de representación de tenants corporativos de Beiqa.

**Fase actual**: 🟢 Discovery / Investigación
**Última actualización**: 2026-02-24

---

## 🎯 ¿Qué es BEIQA Platform?

BEIQA Platform es una herramienta interna diseñada para **reducir el tiempo de búsqueda de propiedades en 50%** y **aumentar la calidad de las recomendaciones** mediante:

- 🤖 **Scraping automatizado** de portales inmobiliarios
- 📊 **Inteligencia de mercado** consolidada de múltiples fuentes
- 🗺️ **Análisis geoespacial** avanzado
- 🤝 **Portal para clientes** con seguimiento de opciones

**Objetivo principal**: Diferenciar a Beiqa de brokers tradicionales mediante tecnología y datos superiores.

> 📖 **[Leer más sobre la visión y objetivos →](./00-Project/Vision-and-Goals.md)**

---

## 🚀 Quick Links

### Para entender el proyecto
- [Visión y Objetivos](./00-Project/Vision-and-Goals.md) - ¿Qué queremos lograr?
- [Contexto de Negocio](./00-Project/Business-Context.md) - ¿Por qué existe este proyecto?
- [Resumen Ejecutivo](./05-Communication/Executive-Summary.md) - Overview para stakeholders

### Para el equipo técnico
- [Arquitectura del Sistema](./02-Architecture/System-Architecture.md) - Diseño técnico
- [Modelo de Datos](./02-Architecture/Database/Data-Model.md) - Estructura de datos
- [Decisiones Técnicas (ADRs)](./02-Architecture/ADRs/) - Decisiones arquitectónicas

### Para planificación
- [Mapa de Módulos](./01-Modules/) - Todos los módulos con dependencias y fases
- [Fase 0: Investigación](./03-Roadmap/Phase-0-Investigation.md) - Estado actual
- [Fase 1: MVP](./03-Roadmap/Phase-1-MVP.md) - Próximos pasos
- [Presupuesto](./04-Validation/Total-Budget.md) - Costos proyectados

---

## 📊 Estado del Proyecto

### Fase Actual: Investigación y Diseño (Fase 0)

**Duración estimada**: 2-4 semanas | **Estado**: 🟢 En progreso

#### Progreso de Fase 0
- ✅ Definición de módulos y estructura de documentación
- ✅ Identificación de pain points y requerimientos iniciales
- ✅ Reestructuración a modelo module-centric
- 🔄 Investigación de fuentes de datos (en curso)
- ⏳ Pruebas de concepto técnicas
- ⏳ Decisiones arquitectónicas (ADRs)

> 📋 **[Ver detalles completos de Fase 0 →](./03-Roadmap/Phase-0-Investigation.md)**

---

## 🧩 Módulos de la Plataforma

| Módulo | Descripción | Fase | Estado |
|--------|-------------|------|--------|
| [Scraper](./01-Modules/Scraper/) | Extracción automatizada de propiedades de portales | Fase 1 — MVP | 🟡 En diseño |
| [Internal App](./01-Modules/Internal-App/) | Aplicación web para el equipo Beiqa | Fase 1 — MVP | 🟡 En diseño |
| [Data Ingestion](./01-Modules/Data-Ingestion/) | Integración de fuentes externas (INEGI, APIs) | Fase 2 | 🔴 Por iniciar |
| [Market Intelligence](./01-Modules/Market-Intelligence/) | Análisis y reportes de mercado automatizados | Fase 2+ | 🔴 Por iniciar |
| [Geospatial](./01-Modules/Geospatial/) | Análisis geoespacial y visualización en mapas | Fase 2+ | 🔴 Por iniciar |
| [AI Brain](./01-Modules/AI-Brain/) | Matching inteligente y automatización | Fase 3+ | 🔴 Por iniciar |
| [Tenant Portal](./01-Modules/Tenant-Portal/) | Portal web para clientes (shortlists, feedback) | Fase 4+ | 🔴 Por iniciar |

> **Base de Datos** vive en [Arquitectura](./02-Architecture/Database/) como infraestructura compartida.

> 📋 **[Ver mapa completo de módulos con dependencias →](./01-Modules/)**

---

## 📚 Documentos Clave

### 🎯 Estratégicos
- [Visión y Objetivos](./00-Project/Vision-and-Goals.md) - Visión del producto y objetivos medibles
- [Pain Points](./00-Project/Pain-Points.md) - Problemas que resuelve la plataforma
- [User Personas](./00-Project/User-Personas.md) - Usuarios y sus necesidades

### 🏗️ Diseño Técnico
- [Arquitectura del Sistema](./02-Architecture/System-Architecture.md) - Diseño de alto nivel
- [Modelo de Datos](./02-Architecture/Database/Data-Model.md) - Estructura de datos

### 🔍 Investigación Crítica (En curso)
- [Comparativo de Portales](./01-Modules/Scraper/Research/Portal-Comparison.md) - Factibilidad de scraping
- [Opciones de Base de Datos](./02-Architecture/Database/Database-Options.md) - Evaluación técnica

### ✅ Decisiones Tomadas
- [ADR-001: Base de Datos](./02-Architecture/ADRs/ADR-001-Database-Choice.md) - PostgreSQL + PostGIS

---

## 📁 Estructura de la Documentación

```
📁 beiqa-real-estate-platform/
├── 📁 00-Project/          # Visión, contexto, stakeholders, métricas, discovery
├── 📁 01-Modules/          # Módulos funcionales (auto-contenidos)
│   ├── 📁 Scraper/         # Extracción de datos de portales
│   ├── 📁 Data-Ingestion/  # Integración de fuentes externas
│   ├── 📁 Market-Intelligence/  # Análisis de mercado
│   ├── 📁 Geospatial/      # Mapas y análisis geoespacial
│   ├── 📁 AI-Brain/        # IA, matching, NLP
│   ├── 📁 Internal-App/    # App web del equipo
│   └── 📁 Tenant-Portal/   # Portal para clientes
├── 📁 02-Architecture/     # Infraestructura: DB, ADRs, tech stack
├── 📁 03-Roadmap/          # Fases del proyecto con mapeo a módulos
├── 📁 04-Validation/       # Presupuesto, feasibility, GO/NO-GO
└── 📁 05-Communication/    # Materiales para stakeholders
```

Cada módulo contiene:
- `README.md` — Descripción, objetivos, métricas, entregables, dependencias, riesgos
- `Product-Questions.md` — Cuestionario de discovery
- `Requirements.md` — Capacidades con priorización Must/Should/Could
- `Research/` — Investigación técnica específica

---

## 👥 Equipo y Contactos

| Rol | Nombre | Contacto |
|-----|--------|----------|
| Product Owner | [Por definir] | |
| Tech Lead | [Por definir] | |
| Business Stakeholder | [Por definir] | |

> 📝 **Nota**: Ver [Stakeholders](./00-Project/Stakeholders.md) para roles y responsabilidades.

---

## 📝 Convenciones de la Documentación

### Estados

| Emoji | Significado |
|-------|-------------|
| 🟢 | Activo / En progreso |
| 🟡 | En revisión / Draft |
| 🔴 | Por iniciar / Bloqueado |
| ✅ | Completado |

### Prioridades de Requerimientos

| Tag | Significado |
|-----|-------------|
| **MUST** | Imprescindible para que funcione |
| **SHOULD** | Importante pero no bloqueante |
| **COULD** | Deseable si hay tiempo/recursos |

---

## 📋 Changelog del Proyecto

| Fecha | Cambio |
|-------|--------|
| 2026-02-24 | Reestructuración completa a modelo module-centric: 6 carpetas, 7 módulos auto-contenidos, DB a arquitectura |
| 2026-02-06 | Expansión masiva de cuestionarios de producto: de 118 a 463 preguntas en 9 módulos |
| 2026-02-05 | Reestructuración del README: mejor orientación a stakeholders |
| 2026-02-04 | Creación de estructura inicial de documentación |
| 2026-02-04 | Definición de módulos y arquitectura de alto nivel |

---

*Última actualización: 2026-02-24*
