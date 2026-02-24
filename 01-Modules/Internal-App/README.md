# Internal App

**Fase del proyecto**: Fase 1 — MVP
**Estado**: 🟡 En diseno
**Owner**: Por definir

---

## Descripcion

La Internal App es la aplicacion web principal que utiliza el equipo de BEIQA en su operacion diaria. Es la interfaz donde los asesores inmobiliarios buscan propiedades, aplican filtros avanzados (zona, superficie, precio, tipo de inmueble), visualizan resultados en mapa y lista, gestionan los requerimientos de sus clientes corporativos, y construyen shortlists de propiedades recomendadas. Tambien incluye dashboards basicos de operacion y la gestion del ciclo de vida de cada busqueda de cliente.

Actualmente el equipo trabaja con una combinacion de portales inmobiliarios, hojas de Excel compartidas, correos electronicos y archivos de Word para gestionar su operacion. Esta fragmentacion genera ineficiencias, duplicacion de trabajo, perdida de informacion y dificultad para colaborar entre asesores. La Internal App centraliza toda la operacion en una sola herramienta disenada especificamente para el flujo de trabajo de BEIQA: desde que un cliente expresa un requerimiento hasta que se le presenta un shortlist de opciones con toda la informacion necesaria para tomar una decision.

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
- **Base de datos** → Esquema completo de propiedades, clientes, requerimientos, shortlists y usuarios
- **Scraper** → Datos de propiedades actualizados que alimentan las busquedas y listados
- **API layer (Arquitectura)** → Backend API REST/GraphQL que sirve datos a la aplicacion frontend

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
