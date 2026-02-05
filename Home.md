# BEIQA Real Estate Platform

Documentación del proyecto de plataforma tecnológica para Beiqa - Representación de Tenants Corporativos.

**Fase actual**: 🟢 Discovery / Investigación

---

## Quick Links

### Por dónde empezar
- [[Vision-and-Goals|Visión y Objetivos]]
- [[Business-Context|Contexto de Negocio]]
- [[System-Architecture|Arquitectura del Sistema]]
- [[Phase-0-Investigation|Fase 0: Investigación]]

### Para el equipo técnico
- [[Source-Catalog|Catálogo de Fuentes de Datos]]
- [[Database-Options|Opciones de Base de Datos]]
- [[Data-Model|Modelo de Datos]]

### Para stakeholders
- [[Executive-Summary|Resumen Ejecutivo]]
- [[Total-Budget|Estimación de Presupuesto]]

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

| # | Módulo | Descripción | Estado |
|---|--------|-------------|--------|
| 1 | [[03-Requirements/Functional-Requirements/01-Scraper|Scraper]] | Extracción de propiedades de portales | 📝 Especificando |
| 2 | Data Ingestion | Integración de fuentes externas | 📋 Por especificar |
| 3 | Base de Datos | Almacenamiento centralizado | 📝 Diseñando |
| 4 | Market Intelligence | Análisis de mercado | 📋 Por especificar |
| 5 | Geospatial | Análisis geoespacial | 📋 Por especificar |
| 6 | AI Brain | Matching y automatización | 📋 Por especificar |
| 7 | Tenant Portal | Portal para clientes | 📋 Por especificar |

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
- [[Portal-Comparison|Comparativo de Portales]]
- [[INEGI-APIs|APIs de INEGI]]
- [[Google-Maps-Platform|Google Maps Platform]]
- [[Catastro-Research|Datos Catastrales]]
- [[Providers-Evaluation|Proveedores Comerciales]]

### Diseño
- [[Data-Model|Modelo de Datos]]
- [[System-Architecture|Arquitectura del Sistema]]
- [[User-Personas|User Personas]]

### Decisiones
- [[ADR-001-Database-Choice|ADR-001: Base de Datos]]
- [[ADR-Template|Template para ADRs]]

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
