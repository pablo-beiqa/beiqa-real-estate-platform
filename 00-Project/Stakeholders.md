# Stakeholders — BEIQA Platform

> **Actualizado**: Febrero 2026

---

## Equipo Actual

| Nombre | Rol | Responsabilidad principal |
|--------|-----|--------------------------|
| **Pablo** | CEO / Product Owner | Visión estratégica, aprobación final, prioridades de negocio |
| **Fabrizio** | Tech Lead | Arquitectura, desarrollo backend, decisiones técnicas, DB, integraciones |
| **Jerónimo** | Ops / Llamadas | Operaciones, llamadas con clientes, trigger manual de scraping vía Slack |
| **Pamela** | Frontend Developer | Desarrollo de Internal App y Tenant Portal (Next.js) |
| **Alex** | Advisor externo | Consultoría técnica y de datos (participó en llamada de arquitectura 23 feb) |

---

## Usuarios del Sistema

### Usuarios activos (Fase 1)

| Usuario | Rol | Uso esperado | Features críticos |
|---------|-----|--------------|------------------|
| Fabrizio | Tech Lead | Admin + uso diario | Todo — es el dev |
| Jerónimo | Ops | Uso diario | Trigger scraping, ver propiedades, reportes |
| Pablo | CEO/PO | Revisión periódica | Dashboard, reportes, estado del proyecto |

### Usuarios futuros (Fase 2)

| Usuario | Rol | Uso esperado | Features críticos |
|---------|-----|--------------|------------------|
| Pamela | Frontend | Desarrollo | Internal App + Portal |
| Tenants (clientes) | Empresas buscando espacio | Semanal durante proceso | Portal con propiedades, shortlist, mapa |

---

## Matriz RACI

| Actividad | Pablo (CEO) | Fabrizio (Tech) | Jerónimo (Ops) | Pamela (FE) |
|-----------|------------|----------------|----------------|-------------|
| Definir visión del producto | A | C | C | I |
| Priorizar features | A | R | C | I |
| Aprobar presupuesto | A/R | C | I | I |
| Definir arquitectura | I | A/R | I | C |
| Desarrollo backend | I | R/A | I | I |
| Desarrollo frontend | I | A | I | R |
| Operaciones / scraping | I | A | R | I |
| Testing y validación | C | A | R | R |

**Leyenda**: R = Responsable, A = Accountable, C = Consultado, I = Informado

---

## Canales de Comunicación

| Stakeholder | Canal | Frecuencia |
|-------------|-------|-----------|
| Pablo ↔ Fabrizio | Slack / llamadas | Según necesidad |
| Equipo completo | Slack | Diario |
| Jerónimo | Slack (triggers manuales) | Diario en operaciones |
| Alex (advisor) | Llamadas | Según necesidad |

---

## Contactos para Decisiones

| Decisión | Quién aprueba |
|---------|--------------|
| Cambios de stack / arquitectura | Fabrizio (consulta Pablo) |
| Nuevas features | Pablo (consulta Fabrizio) |
| Cambios de presupuesto | Pablo |
| Accesos a herramientas | Fabrizio |
| Onboarding de nuevo portal | Fabrizio + Jerónimo |

---

*Actualizado: 2026-02-24*
