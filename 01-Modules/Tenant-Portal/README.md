# Tenant Portal

**Fase del proyecto**: Fase 2 — Portal Web + Data
**Estado**: 🟡 En diseño (Phase 0 completado en beiqa-frontend)
**Owner**: Pamela (Frontend)

**Repositorio de codigo**: [beiqa-frontend](https://github.com/pablo-beiqa/beiqa-frontend)

---

## Descripcion

El Tenant Portal es la aplicacion web orientada a los **clientes corporativos** de BEIQA — las empresas que buscan espacios comerciales o industriales en renta o compra. A traves de este portal, los clientes pueden:

1. **Revisar su scoring** completo (criterios, pesos, zonas, especificaciones tecnicas, condiciones comerciales)
2. **Ver propiedades recomendadas** (shortlist) con score por criterio de su scoring
3. **Dar feedback estructurado** sobre cada propiedad (Si/No/Quiza + razon + comentario)
4. **Visualizar en mapa** la ubicacion de propiedades y datos contextuales de zona

Actualmente la interaccion con clientes se realiza a traves de correos electronicos con PDFs adjuntos, presentaciones de PowerPoint y llamadas telefonicas. El Tenant Portal profesionaliza esta interaccion, captura feedback estructurado para refinar busquedas, y posiciona a BEIQA como empresa con tecnologia diferenciada.

> **Nota**: El scoring dashboard (generado desde llamadas via Circleback) es parte del Tenant Portal, NO de la Internal App. La Internal App es la herramienta interna del equipo Beiqa para gestion de operaciones, data y GIS.

---

## Estado Actual

### En produccion (beiqa-frontend — Phase 0)

| Componente | Estado | Detalle |
|-----------|--------|---------|
| **Framework** | ✅ | Next.js 15 + App Router + TypeScript strict |
| **Styling** | ✅ | Tailwind CSS + shadcn/ui (Card, Badge, Table) |
| **Supabase client** | ✅ | SSR + Browser clients configurados (`@supabase/ssr`) |
| **HubSpot client** | ✅ | API wrapper para deals y contactos |
| **Scoring Dashboard** | ✅ | 3 paginas: home con stats, lista de scorings, detalle con 5 secciones |
| **ScoringDocument schema** | ✅ | TypeScript completo (147 lineas) con todos los tipos |
| **Claude agent** | ✅ | score-discovery: Circleback transcript → JSON scoring → dashboard |

### Por desarrollar (Fase 2 — Sprint actual)

| Componente | Estado | Prioridad |
|-----------|--------|-----------|
| Autenticacion magic link + password | 🔴 | MUST |
| Shortlist con scores por criterio | 🔴 | MUST |
| Feedback estructurado | 🔴 | MUST |
| Mapa de propiedades (Atlas.co) | 🔴 | SHOULD |
| Notificaciones (email/Slack) | 🔴 | SHOULD |

---

## Journey del Cliente

```
1. Reunion inicial → Entender necesidades del cliente
2. Scoring → Construir criterios, pesos, zonas, specs, condiciones comerciales
3. Cliente revisa scoring → Aprueba/comenta en el portal
4. Busqueda → Filtrar propiedades que matchean scoring
5. Shortlist → Presentar propiedades con score por criterio
6. Feedback → Cliente evalua: Si/No/Quiza + razon + comentario
7. Iteracion → Refinar busqueda basada en feedback
8. Visitas → Agendar tours a propiedades seleccionadas
```

**Hoy**: Pasos 1-3 digitalizados en el portal (scoring dashboard). Pasos 4-8 son manuales (email, PowerPoint, llamadas).
**Objetivo Fase 2**: Digitalizar pasos 4-7 en el portal.

---

## Objetivos

1. **Capturar feedback estructurado** de clientes sobre propiedades (Si/No/Quiza + razon) para acelerar el ciclo de presentacion-feedback de 5-7 dias a menos de 48 horas.
2. **Profesionalizar la experiencia del cliente** con un portal donde pueda revisar scoring, propiedades y dar feedback de forma autonoma, apuntando a NPS > 50.
3. **Reducir trabajo manual** del equipo al eliminar presentaciones en PowerPoint y centralizar la comunicacion en el portal.

---

## Metricas de Exito / KPIs

| Metrica | Target | Como se mide |
|---------|--------|--------------|
| Tasa de feedback | >=80% de propiedades compartidas reciben feedback dentro de 48h | (Propiedades con feedback) / (propiedades compartidas) |
| Tiempo de ciclo | <48 horas entre compartir shortlist y recibir feedback | Mediana de tiempo (timestamp feedback - timestamp compartido) |
| NPS de cliente | >50 | Encuesta NPS al finalizar cada proceso de busqueda |
| Adopcion | >=60% de nuevos clientes usan el portal | (Clientes en portal) / (total clientes nuevos) por trimestre |

> **Escala actual**: 5-15 clientes corporativos activos simultaneamente. Metricas calibradas para este volumen.

---

## Entregables Clave

| Entregable | Descripcion | Estado |
|-----------|-------------|--------|
| Scoring Dashboard | Dashboard donde el cliente ve su scoring completo: criterios, pesos, zonas, specs, condiciones, escala de resultado | ✅ Phase 0 |
| Autenticacion de clientes | Magic link (primario) + password (fallback) via Supabase Auth. Aislamiento estricto por cliente (RLS). | 🔴 |
| Shortlist con scores | Lista de propiedades recomendadas con score por criterio del scoring. Datos de propiedad: tipo, precio, m2, ubicacion, fotos. | 🔴 |
| Feedback estructurado | Si/No/Quiza por propiedad + razon predefinida (precio, ubicacion, tamano, etc.) + comentario libre. | 🔴 |
| Mapa de propiedades | Mapa interactivo con ubicaciones de propiedades del shortlist. Proveedor: Atlas.co (API + embed). | 🔴 |

---

## Dependencias

### Necesita (upstream)
- **Supabase** → Base de datos (propiedades, shortlists, feedback) + Auth (magic link, RLS) + Storage
- **Scraper** → Datos de propiedades en Supabase (~60K+ listings)
- **HubSpot** → Datos de scoring via pipeline de Circleback

### Depende de este (downstream)
- **Market Intelligence** → Consumira datos de feedback para refinar recomendaciones

---

## Riesgos Clave

| # | Riesgo | Impacto | Probabilidad | Mitigacion |
|---|--------|---------|--------------|------------|
| 1 | Baja adopcion por clientes que prefieran email/telefono | Alto | Media | Portal tan simple como revisar un email, no eliminar canales tradicionales sino complementarlos |
| 2 | Datos parciales de propiedades (faltan fotos, precios, m2 en algunos listings) | Medio | Alta | Manejo graceful de campos vacios, indicadores visuales claros, priorizar propiedades con data completa |
| 3 | Mobile + desktop con importancia igual desde dia 1 | Medio | Media | Responsive-first con shadcn/ui + Tailwind, testing en ambos dispositivos |
| 4 | Seguridad y privacidad de datos corporativos | Alto | Media | RLS en Supabase, auth robusto, aislamiento estricto por cliente, LFPDPPP compliance |
| 5 | Scope creep: clientes piden features que convierten el portal en CRM | Medio | Alta | Alcance claro (scoring + shortlist + feedback), roadmap publico, priorizar con MoSCoW |

---

## Stack Tecnico

| Componente | Tecnologia | Estado |
|-----------|-----------|--------|
| Framework | Next.js 15 (App Router) | ✅ Implementado |
| Lenguaje | TypeScript (strict mode) | ✅ Implementado |
| Styling | Tailwind CSS + shadcn/ui | ✅ Implementado |
| Base de datos / Auth | Supabase (PostgreSQL + PostGIS + Auth + Storage) | ✅ Configurado |
| CRM | HubSpot (deals, contactos) | ✅ Configurado |
| Mapas | Atlas.co (API + embed) | 🔴 Por implementar |
| Notificaciones | Email + Slack | 🔴 Por implementar |
| Deploy | Vercel | 🟡 Por configurar |

---

## Documentos del Modulo

- [Product Questions](./Product-Questions.md) — Cuestionario de discovery con respuestas
- [Requirements](./Requirements.md) — Capacidades MUST / SHOULD / COULD
- [Research/](./Research/) — Investigacion tecnica
  - [Auth-Strategy.md](./Research/Auth-Strategy.md) — Estrategia de autenticacion
