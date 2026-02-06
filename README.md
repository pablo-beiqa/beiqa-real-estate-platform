# BEIQA Real Estate Platform

Documentación del proyecto de plataforma tecnológica para Beiqa - Representación de Tenants Corporativos.

**Fase actual**: 🟢 Discovery / Investigación

---

## Quick Links

### Por dónde empezar
- [Visión y Objetivos](./00-Overview/Vision-and-Goals.md)
- [Contexto de Negocio](./01-Discovery/Business-Context.md)
- [Arquitectura del Sistema](./04-Architecture/System-Architecture.md)
- [Fase 0: Investigación](./06-Roadmap/Phase-0-Investigation.md)

### Para el equipo técnico
- [Catálogo de Fuentes de Datos](./02-Research/Data-Sources-Research/Source-Catalog.md)
- [Opciones de Base de Datos](./02-Research/Technology-Research/Database-Options.md)
- [Modelo de Datos](./03-Requirements/Data-Requirements/Data-Model.md)

### Para stakeholders
- [Resumen Ejecutivo](./09-Communication/Stakeholder-Presentations/Executive-Summary.md)
- [Estimación de Presupuesto](./02-Research/Cost-Estimates/Total-Budget.md)

---

## Estructura de la Documentación

```
📁 beiqa-real-estate-platform/
├── 📁 00-Overview/          # Visión, stakeholders, métricas
├── 📁 01-Discovery/         # Contexto de negocio, pain points
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
│   └── 📁 Wireframes/
├── 📁 06-Roadmap/           # Fases del proyecto
├── 📁 07-Decisions/         # ADRs (Architecture Decision Records)
├── 📁 08-Data-Sources/      # Documentación de fuentes específicas
│   ├── 📁 INEGI/
│   ├── 📁 Google/
│   ├── 📁 Government/
│   ├── 📁 Commercial/
│   └── 📁 Scraping/
└── 📁 09-Communication/     # Presentaciones y materiales
    ├── 📁 Stakeholder-Presentations/
    └── 📁 Demo-Scripts/
```

---

## Módulos de la Plataforma

| #   | Módulo                                               | Descripción                     | Estado                                |                  |
| --- | ---------------------------------------------------- | ------------------------------- | ------------------------------------- | ---------------- |
| 1   | [Scraper](./03-Requirements/Functional-Requirements/01-Scraper.md) | Extracción de propiedades de portales | 📝 Especificando |
| 2   | Data Ingestion                                       | Integración de fuentes externas | 📋 Por especificar                    |                  |
| 3   | Base de Datos                                        | Almacenamiento centralizado     | 📝 Diseñando                          |                  |
| 4   | Market Intelligence                                  | Análisis de mercado             | 📋 Por especificar                    |                  |
| 5   | Geospatial                                           | Análisis geoespacial            | 📋 Por especificar                    |                  |
| 6   | AI Brain                                             | Matching y automatización       | 📋 Por especificar                    |                  |
| 7   | Tenant Portal                                        | Portal para clientes            | 📋 Por especificar                    |                  |

---

## Estado del Proyecto

### Fase 0: Investigación (Actual)
- [x] Definición de módulos
- [x] Estructura de documentación
- [ ] Investigación de fuentes de datos
- [ ] Pruebas de concepto
- [ ] Decisiones arquitectónicas

### Próximos Hitos
- **Completar Fase 0**: ~2-4 semanas
- **Iniciar Fase 1 (MVP)**: Post validación

---

## Documentos Clave

### Investigación Activa
- [Comparativo de Portales](./02-Research/Scraper-Research/Portal-Comparison.md)
- [APIs de INEGI](./02-Research/Data-Sources-Research/INEGI-APIs.md)
- [Google Maps Platform](./02-Research/Data-Sources-Research/Google-Maps-Platform.md)
- [Datos Catastrales](./08-Data-Sources/Government/Catastro-Research.md)
- [Proveedores Comerciales](./08-Data-Sources/Commercial/Providers-Evaluation.md)

### Diseño
- [Modelo de Datos](./03-Requirements/Data-Requirements/Data-Model.md)
- [Arquitectura del Sistema](./04-Architecture/System-Architecture.md)
- [User Personas](./05-User-Experience/User-Personas.md)

### Decisiones
- [ADR-001: Base de Datos](./07-Decisions/ADR-001-Database-Choice.md)
- [Template para ADRs](./07-Decisions/ADR-Template.md)

---

## Equipo y Contactos

| Rol | Nombre | Contacto |
|-----|--------|----------|
| Product Owner | [Por definir] | |
| Tech Lead | [Por definir] | |
| Business Stakeholder | [Por definir] | |

---

## Convenciones de la Documentación

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

## Changelog del Proyecto

| Fecha | Cambio |
|-------|--------|
| 2026-02-04 | Creación de estructura inicial de documentación |
| 2026-02-04 | Definición de módulos y arquitectura de alto nivel |
| | |

---

*Última actualización: 2026-02-04*
