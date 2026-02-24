# Tenant Portal

**Fase del proyecto**: Fase 4+ — Post-MVP
**Estado**: 🔴 Por iniciar
**Owner**: Por definir

---

## Descripcion

El Tenant Portal es la aplicacion web orientada a los clientes corporativos de BEIQA — las empresas que buscan espacios comerciales o industriales en renta o compra. A traves de este portal, los clientes pueden revisar las propiedades recomendadas por su asesor de BEIQA, comparar opciones lado a lado, proporcionar feedback estructurado sobre cada propiedad (me interesa, no me interesa, necesito mas informacion) y mantener un registro de la comunicacion con su asesor a lo largo del proceso de busqueda.

Actualmente la interaccion con clientes se realiza a traves de correos electronicos con PDFs adjuntos, presentaciones de PowerPoint y llamadas telefonicas. Este proceso dificulta el seguimiento, genera versiones desactualizadas de informacion y no captura de forma estructurada las preferencias del cliente. El Tenant Portal profesionaliza esta interaccion, permite al cliente revisar opciones a su propio ritmo, genera feedback que el equipo de BEIQA puede usar para refinar las busquedas, y posiciona a BEIQA como una empresa con tecnologia diferenciada frente a competidores que aun operan con metodos tradicionales.

---

## Objetivos

1. Mejorar la experiencia del cliente corporativo proporcionando un portal profesional donde pueda revisar y evaluar propiedades recomendadas de forma autonoma, apuntando a un NPS de cliente superior a 50.
2. Capturar feedback estructurado de clientes sobre cada propiedad (criterios de aceptacion/rechazo) para alimentar el motor de recomendaciones y acelerar el proceso de busqueda.
3. Reducir el ciclo de presentacion-feedback de 5-7 dias (via email) a menos de 48 horas mediante acceso inmediato a recomendaciones y herramientas de evaluacion en linea.

---

## Metricas de Exito / KPIs

| Metrica | Target | Como se mide |
|---------|--------|--------------|
| Engagement de clientes | >=70% de clientes activos acceden al portal al menos 1 vez por semana | (Clientes con sesion en ultimos 7 dias) / (clientes con busqueda activa) |
| Tasa de respuesta de feedback | >=80% de propiedades compartidas reciben feedback del cliente dentro de 48 horas | (Propiedades con feedback) / (propiedades compartidas) filtrado por tiempo |
| NPS de cliente | >50 | Encuesta NPS enviada al finalizar cada proceso de busqueda |
| Tiempo de ciclo presentacion-feedback | <48 horas (vs 5-7 dias baseline) | Mediana de tiempo entre compartir shortlist y recibir feedback completo |
| Adopcion por clientes | >=60% de nuevos clientes usan el portal durante su proceso de busqueda | (Clientes que usan portal) / (total clientes nuevos) por trimestre |

---

## Entregables Clave

| Entregable | Descripcion | Estado |
|-----------|-------------|--------|
| Visor de propiedades | Interfaz donde el cliente ve propiedades recomendadas con fotos, atributos, mapa y datos de zona en formato profesional | 🔴 |
| Herramienta de comparacion | Tabla comparativa lado a lado de 2-5 propiedades con atributos clave, mapa comparativo y resumen de pros/contras | 🔴 |
| Sistema de feedback | Mecanismo para que el cliente evalue cada propiedad (interes, rechazo, condicional), con campos para comentarios y criterios especificos | 🔴 |
| Log de comunicacion | Historial de interacciones entre el cliente y su asesor de BEIQA (mensajes, documentos compartidos, decisiones) | 🔴 |
| Autenticacion y acceso seguro | Sistema de login seguro para clientes con enlaces de acceso por proyecto, sin necesidad de registro complejo | 🔴 |

---

## Dependencias

### Necesita (upstream)
- **Base de datos** → Datos de propiedades, shortlists y perfiles de cliente
- **Internal App (API)** → Comparte la API del backend para acceder a propiedades, shortlists y gestion de feedback
- **AI Brain** → Recomendaciones personalizadas y propiedades sugeridas basadas en perfil y feedback del cliente

### Depende de este (downstream)
- Ninguno — el Tenant Portal es el punto final de la cadena de valor de la plataforma

---

## Riesgos Clave

| # | Riesgo | Impacto | Probabilidad | Mitigacion |
|---|--------|---------|--------------|------------|
| 1 | Baja adopcion por parte de clientes que prefieran el proceso tradicional (email/telefono) | Alto | Media | Hacer el portal tan simple como revisar un email, onboarding guiado, no eliminar canales tradicionales sino complementarlos |
| 2 | Requisitos de seguridad y privacidad para datos de clientes corporativos | Alto | Media | Autenticacion robusta, datos encriptados, permisos granulares, cumplimiento con LFPDPPP (ley de proteccion de datos mexicana) |
| 3 | Inflacion de alcance: clientes piden features que convierten el portal en un CRM completo | Medio | Alta | Definir alcance claro del portal vs la app interna, roadmap publico, priorizar features con impacto medible |
| 4 | Mantenimiento de dos aplicaciones frontend (Internal App + Tenant Portal) con equipo pequeno | Medio | Media | Compartir componentes UI (design system), misma API backend, mono-repo o libreria de componentes compartidos |
| 5 | Expectativas de clientes que excedan las capacidades del portal en version inicial | Medio | Media | Comunicar claramente que es una v1, recopilar feedback para iterar, mostrar roadmap de mejoras planificadas |

---

## Documentos del Modulo

- [Product Questions](./Product-Questions.md) — Cuestionario de discovery
- [Requirements](./Requirements.md) — Capacidades y criterios de aceptacion
- [Research/](./Research/) — Investigacion tecnica
