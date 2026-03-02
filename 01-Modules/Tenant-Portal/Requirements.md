# Requerimientos: Tenant Portal

**Estado**: 🟡 En diseño (Phase 0 completado)

> **Prioridades**: **MUST** = Imprescindible | **SHOULD** = Importante pero no bloqueante | **COULD** = Deseable si hay tiempo

---

## Capacidades

### Autenticacion y Acceso

- **MUST** Magic link como metodo primario de login via Supabase Auth (`signInWithOtp`)
  - [ ] Envio de magic link al email del cliente
  - [ ] Compatible con cualquier proveedor de email (Gmail, Outlook, corporativo)
  - [ ] Redireccion al dashboard del cliente tras autenticacion
- **MUST** Password como metodo secundario via Supabase Auth (`signInWithPassword`)
  - [ ] El equipo Beiqa crea la cuenta del cliente
  - [ ] Flujo de reset de password
- **MUST** Aislamiento estricto por cliente via RLS (Row Level Security)
  - [ ] Cliente solo ve sus propios scorings
  - [ ] Cliente solo ve sus propios shortlists
  - [ ] Cliente solo ve su propio feedback
  - [ ] Ningun dato de otros clientes accesible
- **MUST** Funcionar en mobile y desktop con importancia igual desde dia 1
- **SHOULD** Sesion persistente (no pedir login cada vez que el cliente regresa)
- **SHOULD** Pagina de login limpia con branding de Beiqa

### Scoring (Vista del Cliente)

- **MUST** Mostrar scoring completo del cliente — transparencia total
  - [x] Filtros de elegibilidad (criterio + resultado pasa/no pasa)
  - [x] Criterios de scoring con pesos (suman 100 puntos) + rubrica
  - [x] Especificaciones tecnicas (especificacion + requerimiento)
  - [x] Condiciones comerciales (renta, deposito, plazo, ajuste anual, etc.)
  - [x] Zonas objetivo (prioridad + zona + alcaldias + contexto)
  - [x] Escala de resultado (calificacion final)
- **MUST** Manejar datos parciales: mostrar campos disponibles, ocultar campos vacios
  - [x] Implementado en Phase 0 (scoring detail page)
- **SHOULD** Permitir al cliente aprobar/comentar el scoring
  - [ ] Boton de aprobacion
  - [ ] Campo de comentarios sobre el scoring

### Shortlist de Propiedades

- **MUST** Mostrar lista de propiedades recomendadas para el cliente
  - [ ] Datos por propiedad: tipo, precio, superficie m2, ubicacion, fotos (cuando disponibles)
  - [ ] Score por criterio del scoring del cliente (zona ✓, precio ✓, tamano ✗, etc.)
  - [ ] Estado de cada propiedad (nueva, vista, con feedback)
- **MUST** Manejar datos incompletos gracefully
  - [ ] Indicadores visuales para campos faltantes (sin foto, sin precio, etc.)
  - [ ] No romper UI si faltan campos
  - [ ] Priorizar propiedades con data completa en el orden de la lista
- **SHOULD** Comparacion lado a lado de 2-5 propiedades
  - [ ] Tabla comparativa con atributos clave
  - [ ] Comparacion visual de scores por criterio
- **SHOULD** Filtros y ordenamiento dentro del shortlist
  - [ ] Ordenar por score total, precio, superficie
  - [ ] Filtrar por tipo, zona, rango de precio
- **COULD** Galeria de fotos por propiedad
  - [ ] Carousel de imagenes
  - [ ] Foto principal como thumbnail en la lista

### Feedback Estructurado

- **MUST** Evaluacion por propiedad: Si / No / Quiza
  - [ ] Botones claros y accesibles (mobile-friendly)
  - [ ] Feedback guardado inmediatamente en Supabase
- **MUST** Razon del feedback: opciones predefinidas
  - [ ] Precio (muy caro / muy barato)
  - [ ] Ubicacion (lejos / zona insegura / mala conectividad)
  - [ ] Tamano (muy grande / muy chico)
  - [ ] Condiciones comerciales (plazo / deposito / ajuste)
  - [ ] Infraestructura (no cumple specs tecnicas)
  - [ ] Otra (campo libre)
- **MUST** Campo de comentario opcional (texto libre)
  - [ ] Maximo ~500 caracteres
  - [ ] Placeholder con ejemplo de comentario util
- **SHOULD** Timestamp de feedback para medir velocidad de respuesta
  - [ ] Registrar cuando se compartio el shortlist y cuando llego el feedback
  - [ ] Calcular delta para KPI de <48 horas

### Mapas

- **MUST** Mostrar ubicacion de propiedades del shortlist en mapa interactivo
  - [ ] Markers clickeables con info basica de la propiedad
  - [ ] Centrar mapa en la zona objetivo del scoring
- **SHOULD** Proveedor: Atlas.co (API + embed)
  - [ ] Integrar Atlas.co embed en pagina de shortlist
  - [ ] Markers dinamicos basados en propiedades del shortlist
- **COULD** Capas contextuales (Fase posterior)
  - [ ] POIs cercanos (transporte, comercios, servicios)
  - [ ] Datos socioeconomicos por zona (INEGI)
  - [ ] Zonas H3 visualizadas
- **COULD** Heatmaps (Fase posterior)
  - [ ] Precio/m2 por zona
  - [ ] Densidad de oferta
  - [ ] Comparativos visuales entre zonas

### Market Intelligence (Vista del Cliente)

- **SHOULD** Precio/m2 promedio de la zona de interes del cliente
  - [ ] Rango min-max y promedio
  - [ ] Comparativo con zonas similares
- **SHOULD** Comparativo de zonas
  - [ ] Tabla: Zona A vs Zona B vs Zona C
  - [ ] Metricas: precio, oferta disponible, infraestructura
- **COULD** Tendencias de precio historico
  - [ ] Grafica de precio/m2 ultimos 6-12 meses
  - [ ] Indicador de tendencia (subiendo/bajando/estable)
- **COULD** Datos socioeconomicos de la zona
  - [ ] Poblacion, ingreso promedio, actividad economica (INEGI)
- **COULD** Exportar analisis como PDF descargable
  - [ ] Reporte de mercado por zona
  - [ ] Incluir scoring + propiedades + comparativos

### Notificaciones

- **SHOULD** Email al cliente cuando se comparte un nuevo shortlist
  - [ ] Template profesional con resumen de propiedades
  - [ ] Link directo al portal (magic link o login)
- **SHOULD** Slack notification al equipo Beiqa cuando cliente da feedback
  - [ ] Canal dedicado o thread por cliente
  - [ ] Resumen: que propiedad, que evaluacion, que razon
- **COULD** Email digest al cliente con resumen semanal de actividad
- **COULD** Push notifications mobile (cuando haya PWA o app nativa)

### Integraciones

- **MUST** Supabase (DB + Auth + Storage + REST API)
  - [x] Browser client configurado
  - [x] Server client configurado (SSR)
  - [ ] RLS policies por cliente
  - [ ] Tablas de shortlists y feedback
- **MUST** HubSpot (scoring data via Circleback pipeline)
  - [x] API client configurado (deals, contactos)
  - [ ] Sync de estado de deal al portal
- **SHOULD** Atlas.co (mapas)
  - [ ] API key configurada
  - [ ] Embed component integrado
- **SHOULD** Email (notificaciones)
  - [ ] Proveedor: por definir (Resend, SendGrid, o Supabase email)
- **SHOULD** Slack (notificaciones internas)
  - [ ] Webhook configurado

### UI/UX

- **MUST** Responsive: mobile + desktop con importancia igual
  - [x] Tailwind CSS configurado
  - [x] shadcn/ui components (Card, Badge, Table)
  - [ ] Testing en dispositivos moviles
- **MUST** Manejo de estados vacios (sin scorings, sin shortlists, sin feedback)
  - [x] Empty state en scoring dashboard (Phase 0)
  - [ ] Empty states en shortlist y feedback
- **SHOULD** Design system consistente con branding Beiqa
  - [ ] Colores corporativos
  - [ ] Tipografia consistente
  - [ ] Iconografia uniforme
- **COULD** Modo oscuro

---

## Preguntas Abiertas

1. **Tablas en Supabase**: ¿Cual es el schema para `shortlists` y `feedback`? Necesita diseño antes de implementar.
2. **Compartir shortlist**: ¿El equipo comparte via link directo, notificacion por email, o ambos?
3. **Journey en DB**: ¿Como se relaciona scoring → shortlist → visitas en el modelo de datos?
4. **Scoring en DB vs filesystem**: Actualmente los scorings se leen del filesystem (JSON). ¿Migrar a Supabase?
5. **Atlas.co pricing**: ¿Cual es el costo mensual? ¿Hay free tier suficiente para 5-15 clientes?
6. **Feedback categories**: ¿Las opciones predefinidas de razon de rechazo son suficientes o necesitan refinamiento con Jeronimo (ops)?

---

*Ultima actualizacion: 2026-03-02*
