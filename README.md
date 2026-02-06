# BEIQA Real Estate Platform

> Plataforma tecnológica interna que centraliza la búsqueda de inmuebles comerciales/industriales, inteligencia de mercado y gestión de clientes para el equipo de representación de tenants corporativos de Beiqa.

**Fase actual**: 🟢 Discovery / Investigación  
**Última actualización**: 2026-02-05

---

## 🎯 ¿Qué es BEIQA Platform?

BEIQA Platform es una herramienta interna diseñada para **reducir el tiempo de búsqueda de propiedades en 50%** y **aumentar la calidad de las recomendaciones** mediante:

- 🤖 **Scraping automatizado** de portales inmobiliarios
- 📊 **Inteligencia de mercado** consolidada de múltiples fuentes
- 🗺️ **Análisis geoespacial** avanzado
- 🤝 **Portal para clientes** con seguimiento de opciones

**Objetivo principal**: Diferenciar a Beiqa de brokers tradicionales mediante tecnología y datos superiores.

> 📖 **[Leer más sobre la visión y objetivos →](./00-Overview/Vision-and-Goals.md)**

---

## 🚀 Quick Links

### Para entender el proyecto
- [Visión y Objetivos](./00-Overview/Vision-and-Goals.md) - ¿Qué queremos lograr?
- [Contexto de Negocio](./01-Discovery/Business-Context.md) - ¿Por qué existe este proyecto?
- [Resumen Ejecutivo](./09-Communication/Stakeholder-Presentations/Executive-Summary.md) - Overview para stakeholders

### Para el equipo técnico
- [Arquitectura del Sistema](./04-Architecture/System-Architecture.md) - Diseño técnico
- [Modelo de Datos](./03-Requirements/Data-Requirements/Data-Model.md) - Estructura de datos
- [Catálogo de Fuentes de Datos](./02-Research/Data-Sources-Research/Source-Catalog.md) - Fuentes disponibles

### Para planificación
- [Fase 0: Investigación](./06-Roadmap/Phase-0-Investigation.md) - Estado actual
- [Fase 1: MVP](./06-Roadmap/Phase-1-MVP.md) - Próximos pasos
- [Estimación de Presupuesto](./02-Research/Cost-Estimates/Total-Budget.md) - Costos proyectados

---

## 📊 Estado del Proyecto

### Fase Actual: Investigación y Diseño (Fase 0)

**Duración estimada**: 2-4 semanas | **Estado**: 🟢 En progreso

#### Progreso de Fase 0
- ✅ Definición de módulos y estructura de documentación
- ✅ Identificación de pain points y requerimientos iniciales
- 🔄 Investigación de fuentes de datos (en curso)
- ⏳ Pruebas de concepto técnicas
- ⏳ Decisiones arquitectónicas (ADRs)

#### Próximos Hitos
- **Completar Fase 0**: ~2-4 semanas
- **Iniciar Fase 1 (MVP)**: Post validación técnica y de negocio

> 📋 **[Ver detalles completos de Fase 0 →](./06-Roadmap/Phase-0-Investigation.md)**

---

## 🧩 Módulos de la Plataforma

| #   | Módulo                                               | Descripción                     | Estado                                | Prioridad MVP |
| --- | ---------------------------------------------------- | ------------------------------- | ------------------------------------- | ------------- |
| 1   | [Scraper](./03-Requirements/Functional-Requirements/01-Scraper.md) | Extracción automatizada de propiedades de portales | 📝 Especificando | ✅ Incluido |
| 2   | Data Ingestion                                       | Integración de fuentes externas (INEGI, APIs) | 📋 Por especificar | ⏸️ Post-MVP |
| 3   | Base de Datos                                        | Almacenamiento centralizado con capacidades geoespaciales | 📝 Diseñando | ✅ Incluido |
| 4   | Market Intelligence                                  | Análisis y reportes de mercado automatizados | 📋 Por especificar | ⏸️ Post-MVP |
| 5   | Geospatial                                           | Análisis geoespacial y visualización en mapas | 📋 Por especificar | ⏸️ Post-MVP |
| 6   | AI Brain                                             | Matching inteligente y automatización | 📋 Por especificar | ⏸️ Post-MVP |
| 7   | Tenant Portal                                        | Portal web para clientes (shortlists, feedback) | 📋 Por especificar | ⏸️ Post-MVP |

**Leyenda de estados**: 🟢 Activo | 🟡 En diseño | 🔴 Por iniciar | ✅ Completado

---

## 📚 Documentos Clave

> Documentos críticos para entender el proyecto o tomar decisiones importantes. Solo se incluyen documentos completos o en estado avanzado.

### 🎯 Estratégicos
- [Visión y Objetivos](./00-Overview/Vision-and-Goals.md) - Visión del producto y objetivos medibles
- [Pain Points](./01-Discovery/Pain-Points.md) - Problemas que resuelve la plataforma
- [User Personas](./05-User-Experience/User-Personas.md) - Usuarios y sus necesidades

### 🏗️ Diseño Técnico
- [Arquitectura del Sistema](./04-Architecture/System-Architecture.md) - Diseño de alto nivel
- [Modelo de Datos](./03-Requirements/Data-Requirements/Data-Model.md) - Estructura de datos

### 🔍 Investigación Crítica (En curso)
- [Comparativo de Portales](./02-Research/Scraper-Research/Portal-Comparison.md) - Factibilidad de scraping
- [Opciones de Base de Datos](./02-Research/Technology-Research/Database-Options.md) - Evaluación técnica

### ✅ Decisiones Tomadas
- [ADR-001: Base de Datos](./07-Decisions/ADR-001-Database-Choice.md) - Decisión arquitectónica

> 💡 **Nota**: Documentos marcados con 🔴 (por investigar) no se incluyen aquí hasta tener resultados.

---

## 📁 Estructura de la Documentación

```
📁 beiqa-real-estate-platform/
├── 📁 00-Overview/          # Visión, stakeholders, métricas, glosario
├── 📁 01-Discovery/         # Contexto de negocio, pain points, proceso actual
├── 📁 02-Research/          # Investigación técnica y de fuentes
│   ├── 📁 Scraper-Research/
│   ├── 📁 Data-Sources-Research/
│   ├── 📁 Technology-Research/
│   └── 📁 Cost-Estimates/
├── 📁 03-Requirements/      # Requerimientos funcionales y de datos
│   ├── 📁 Functional-Requirements/
│   └── 📁 Data-Requirements/
├── 📁 04-Architecture/      # Arquitectura del sistema
├── 📁 05-User-Experience/   # Personas, wireframes
├── 📁 06-Roadmap/           # Fases del proyecto (Fase 0, MVP, etc.)
├── 📁 07-Decisions/         # ADRs (Architecture Decision Records)
├── 📁 08-Data-Sources/      # Documentación detallada de fuentes específicas
│   ├── 📁 Government/
│   └── 📁 Commercial/
└── 📁 09-Communication/     # Presentaciones y materiales para stakeholders
    └── 📁 Stakeholder-Presentations/
```

> 💡 **Tip**: Usa los Quick Links arriba para navegar rápidamente, o explora la estructura completa para documentos específicos.

---

## 👥 Equipo y Contactos

| Rol | Nombre | Contacto |
|-----|--------|----------|
| Product Owner | [Por definir] | |
| Tech Lead | [Por definir] | |
| Business Stakeholder | [Por definir] | |

> 📝 **Nota**: Esta sección se actualizará cuando se definan los roles. Ver [Stakeholders](./00-Overview/Stakeholders.md) para roles y responsabilidades.

---

## 📝 Convenciones de la Documentación

### Estados

| Emoji | Significado |
|-------|-------------|
| 🟢 | Activo / En progreso |
| 🟡 | En revisión / Draft |
| 🔴 | Por iniciar / Bloqueado |
| ✅ | Completado |

### Tags recomendados

- `#research` - Documentos de investigación
- `#decision` - Decisiones tomadas
- `#requirement` - Requerimientos
- `#blocker` - Elementos bloqueados
- `#todo` - Pendientes

---

## 📋 Changelog del Proyecto

| Fecha | Cambio |
|-------|--------|
| 2026-02-05 | Reestructuración del README: mejor orientación a stakeholders, tech stack summary, documentos clave refinados |
| 2026-02-04 | Creación de estructura inicial de documentación |
| 2026-02-04 | Definición de módulos y arquitectura de alto nivel |

---

*Última actualización: 2026-02-05*
