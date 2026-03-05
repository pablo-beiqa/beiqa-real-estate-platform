# Internal App

**Fase del proyecto**: Sprint 5+ (post-MVP de agentes y Tenant Portal)
**Estado**: 🟡 En diseño
**Owner**: Fabrizio (backend) + Pamela (diseño)

> **Nota**: La Internal App es **app.beiqa.com** — la herramienta interna del equipo Beiqa para gestión de operaciones, data y GIS. NO es para clientes; los clientes usan el [Tenant Portal](../Tenant-Portal/).

> **Scoring dashboard**: Existe un scoring dashboard funcional en `beiqa-frontend`, pero pertenece al Tenant Portal, no a la Internal App.

---

## Descripción

La Internal App es la aplicación web principal que utiliza el equipo de BEIQA en su operación diaria (accesible en **app.beiqa.com**). Es la interfaz donde los asesores inmobiliarios buscan propiedades, aplican filtros avanzados (zona, superficie, precio, tipo de inmueble), visualizan resultados en mapa y lista, gestionan los requerimientos de sus clientes corporativos, y construyen shortlists de propiedades recomendadas. También incluye dashboards básicos de operación y la gestión del ciclo de vida de cada búsqueda de cliente.

La Internal App lee del **golden record** (tabla `properties` en Supabase) vía Supabase REST API. No tiene su propia base de datos — consume la misma fuente de verdad que alimentan los agentes AI (Address Enrichment, Normalization, Deduplication).

Actualmente el equipo trabaja con una combinación de portales inmobiliarios, hojas de Excel compartidas, correos electrónicos y archivos de Word para gestionar su operación. Esta fragmentación genera ineficiencias, duplicación de trabajo, pérdida de información y dificultad para colaborar entre asesores. La Internal App centraliza toda la operación en una sola herramienta diseñada específicamente para el flujo de trabajo de BEIQA: desde que un cliente expresa un requerimiento hasta que se le presenta un shortlist de opciones con toda la información necesaria para tomar una decisión.

---

## Objetivos

1. Reemplazar el flujo de trabajo basado en Excel y portales externos por una aplicacion centralizada, logrando que el 100% del equipo la use como herramienta principal en los primeros 3 meses.
2. Centralizar la gestion de propiedades, clientes y shortlists en un solo sistema, eliminando la duplicacion de datos entre hojas de calculo y correos.
3. Reducir el tiempo promedio de busqueda y elaboracion de shortlists de 4 horas a menos de 1 hora por requerimiento de cliente.

---

## Metricas de Exito / KPIs

| Metrica | Target | Como se mide |
|---------|--------|--------------|
| Tasa de adopcion del equipo | 100% del equipo usando la app como herramienta principal a 3 meses del lanzamiento | Porcentaje de asesores con al menos 1 sesion diaria en los ultimos 7 dias |
| Usuarios activos diarios | >=80% del equipo activo cada dia laboral | (Usuarios con sesion en el dia) / (total asesores) promedio semanal |
| Satisfaccion del usuario | >=3.5 / 5.0 en encuesta de satisfaccion | Encuesta NPS/satisfaccion trimestral al equipo |
| Tiempo de busqueda a shortlist | <1 hora promedio (vs 4 horas baseline) | Medicion desde creacion de requerimiento hasta envio de shortlist |
| Propiedades gestionadas en la app | >=90% de propiedades revisadas se gestionan dentro de la app (no en Excel) | Encuesta mensual + auditoria de uso de Excel vs app |

---

## Entregables Clave

| Entregable | Descripcion | Estado |
|-----------|-------------|--------|
| Listado y busqueda de propiedades | Vista principal con tabla/grid de propiedades, filtros avanzados (zona, precio, superficie, tipo), ordenamiento y paginacion | 🔴 |
| Vista de mapa | Mapa interactivo con marcadores de propiedades, clusters, y sincronizacion con los filtros de busqueda | 🔴 |
| Gestion de clientes | CRUD de clientes corporativos con datos de contacto, historial de requerimientos y estado de cada busqueda | 🔴 |
| Constructor de shortlists | Herramienta para seleccionar propiedades, agregar notas del asesor, ordenar opciones y exportar/compartir el shortlist | 🔴 |
| Dashboard basico | Panel con metricas de operacion: propiedades activas, requerimientos abiertos, shortlists pendientes, actividad del equipo | 🔴 |
| Detalle de propiedad | Vista completa de cada propiedad con fotos, atributos, ubicacion en mapa, historial de precios y datos de zona | 🔴 |

---

## Dependencias

### Necesita (upstream)
- **Golden record (`properties`)** → Tabla unificada de propiedades en Supabase, poblada por agentes AI (Normalization, Deduplication)
- **Supabase REST API** → API auto-generada que sirve datos del golden record al frontend
- **Scraper** → Datos de propiedades actualizados que alimentan el golden record
- **AI Brain** → Agentes que enriquecen, normalizan y deduplicar propiedades antes de que lleguen a la Internal App

### Depende de este (downstream)
- **Tenant Portal** → Comparte la misma API y base de datos, hereda patrones de UI y componentes

---

## Riesgos Clave

| # | Riesgo | Impacto | Probabilidad | Mitigacion |
|---|--------|---------|--------------|------------|
| 1 | Resistencia del equipo a adoptar una nueva herramienta y abandonar Excel | Alto | Media | Involucrar al equipo desde el diseno (user research), implementar gradualmente, hacer la app mas facil que Excel para tareas clave |
| 2 | Complejidad de UX que haga la aplicacion dificil de usar y frustre a los usuarios | Alto | Media | Diseno centrado en el usuario, prototipos interactivos validados antes de desarrollo, iteraciones rapidas basadas en feedback |
| 3 | Scope creep: solicitudes continuas de features que retrasen el lanzamiento del MVP | Alto | Alta | Definir MVP estricto con criterios claros, backlog priorizado, decir no a features post-MVP hasta consolidar el core |
| 4 | Performance de busqueda con filtros complejos sobre una base de datos grande | Medio | Media | Indices de base de datos optimizados, paginacion del lado del servidor, cache de busquedas frecuentes |
| 5 | Integracion con el mapa que agregue latencia y complejidad al frontend | Medio | Media | Componente de mapa como modulo independiente con lazy loading, renderizado asincronico de marcadores |

---

## Documentos del Modulo

- [Product Questions](./Product-Questions.md) — Cuestionario de discovery
- [Requirements](./Requirements.md) — Capacidades y criterios de aceptacion
- [Research/](./Research/) — Investigacion tecnica
