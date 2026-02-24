# User Personas - BEIQA Platform

## Objetivo

Definir los usuarios principales de la plataforma para guiar decisiones de diseño y funcionalidad.

**Estado**: 🟡 En desarrollo

---

## Persona 1: Analista de Beiqa (Usuario Principal)

### Perfil

| Atributo | Descripción |
|----------|-------------|
| **Nombre** | "Ana" - Analista de Representación |
| **Rol** | Analista de tenant representation en Beiqa |
| **Experiencia** | 2-5 años en bienes raíces comerciales |
| **Tech savvy** | Media-alta |
| **Frecuencia de uso** | Diario, varias horas |

### Responsabilidades

- Buscar propiedades que cumplan criterios de clientes
- Evaluar y filtrar opciones antes de presentar al cliente
- Generar reportes de mercado
- Comunicarse con clientes sobre opciones
- Negociar con brokers de propiedades

### Necesidades

| Necesidad | Prioridad | Notas |
|-----------|-----------|-------|
| Ver todas las propiedades disponibles en un mapa | Alta | Visualización geográfica |
| Filtrar rápidamente por criterios específicos | Alta | Tipo, zona, precio, superficie |
| Comparar propiedades lado a lado | Alta | Para decisiones |
| Ver datos de mercado de una zona | Alta | Precios, vacancy |
| Guardar propiedades en shortlists por cliente | Alta | Organización |
| Generar reportes para presentar | Media | PDF, presentaciones |
| Ver historial de comunicaciones | Media | Contexto del cliente |
| Recibir alertas de nuevas propiedades | Media | Matching automático |

### Frustraciones Actuales

- Buscar manualmente en múltiples portales
- Información desactualizada o duplicada
- No tener datos de mercado consolidados
- Dificultad para recordar qué se habló con cada cliente
- Crear reportes manualmente

### Escenario de Uso Típico

> Ana recibe una llamada de un cliente buscando bodega de 5,000 m² en zona norte de CDMX. Abre la plataforma, crea una búsqueda con esos criterios, ve en el mapa las opciones disponibles, revisa datos de mercado de cada subzona, filtra las mejores 10 opciones, y genera un reporte con comparativo para enviar al cliente esa misma tarde.

---

## Persona 2: Director de Beiqa (Power User)

### Perfil

| Atributo | Descripción |
|----------|-------------|
| **Nombre** | "Roberto" - Director Comercial |
| **Rol** | Director o socio de Beiqa |
| **Experiencia** | 10+ años en bienes raíces |
| **Tech savvy** | Media |
| **Frecuencia de uso** | Diario, 1-2 horas |

### Responsabilidades

- Supervisar operación del equipo
- Relación con clientes clave
- Decisiones estratégicas
- Análisis de mercado de alto nivel
- Reportes para inversionistas/clientes grandes

### Necesidades

| Necesidad | Prioridad | Notas |
|-----------|-----------|-------|
| Dashboard de KPIs del negocio | Alta | Búsquedas activas, deals, etc. |
| Vista de alto nivel de todas las búsquedas | Alta | Estado de cada cliente |
| Reportes de inteligencia de mercado | Alta | Tendencias, oportunidades |
| Visibilidad de pipeline | Alta | Qué tan cerca de cerrar |
| Exportar datos para presentaciones | Media | Para clientes grandes |

### Frustraciones Actuales

- No tener visibilidad consolidada del negocio
- Depender de que el equipo le reporte manualmente
- No tener datos de mercado para conversaciones estratégicas

---

## Persona 3: Cliente Tenant (Usuario del Portal)

### Perfil

| Atributo | Descripción |
|----------|-------------|
| **Nombre** | "Carlos" - Director de Expansión |
| **Rol** | Ejecutivo en empresa cliente de Beiqa |
| **Industria** | Logística, retail, manufactura, etc. |
| **Tech savvy** | Baja-media |
| **Frecuencia de uso** | Semanal durante búsqueda activa |

### Responsabilidades

- Decidir ubicaciones para expansión de su empresa
- Aprobar opciones presentadas por Beiqa
- Reportar a su dirección sobre opciones

### Necesidades

| Necesidad | Prioridad | Notas |
|-----------|-----------|-------|
| Ver las propiedades que Beiqa recomienda | Alta | Shortlist curada |
| Comparar opciones fácilmente | Alta | Tabla comparativa |
| Ver propiedades en mapa | Alta | Ubicación respecto a puntos clave |
| Dar feedback sobre cada opción | Alta | Me gusta/no me gusta + notas |
| Ver reporte de mercado de la zona | Media | Contexto para decisión |
| Descargar información para presentar internamente | Media | PDF, Excel |
| Comunicarse con Beiqa desde el portal | Baja | Mensajes, requests |

### Frustraciones Actuales

- Recibir información en múltiples formatos (emails, PDFs, Excel)
- No tener un lugar central para ver todas las opciones
- Dificultad para comparar propiedades
- No recordar qué feedback dio sobre cada opción

### Escenario de Uso Típico

> Carlos recibe notificación de que Beiqa agregó 3 nuevas opciones a su búsqueda. Entra al portal, ve las opciones en el mapa junto con la ubicación de sus centros de distribución actuales, revisa las fotos y datos de cada una, marca una como "muy interesante" y otra como "descartada - muy lejos", descarga un comparativo en PDF para mostrar a su director, y agenda una llamada con Beiqa para discutir las opciones.

---

## Matriz de Funcionalidades por Persona

| Funcionalidad | Ana (Analista) | Roberto (Director) | Carlos (Cliente) |
|---------------|----------------|-------------------|------------------|
| Mapa de propiedades | ✅ Completo | ✅ Vista | ✅ Solo sus opciones |
| Búsqueda avanzada | ✅ Completo | ✅ Limitado | ❌ No |
| Crear/editar propiedades | ✅ Completo | ✅ Completo | ❌ No |
| Gestión de clientes | ✅ Completo | ✅ Vista | ❌ Solo su empresa |
| Dashboard de KPIs | ✅ Operativo | ✅ Ejecutivo | ❌ No |
| Reportes de mercado | ✅ Completo | ✅ Completo | ✅ Simplificado |
| Shortlists | ✅ Crear/editar | ✅ Ver | ✅ Ver + feedback |
| Admin del scraper | ✅ Completo | ❌ No | ❌ No |
| Comparativo propiedades | ✅ Completo | ✅ Ver | ✅ Sus opciones |
| Comunicaciones | ✅ Ver/registrar | ✅ Ver | ✅ Iniciar |

---

## Jobs to be Done (JTBD)

### Para Ana (Analista)

1. "Cuando un cliente me pide opciones, necesito encontrar propiedades relevantes rápidamente para poder presentar opciones el mismo día"

2. "Cuando presento opciones, necesito información de mercado que respalde mis recomendaciones para que el cliente confíe en mi análisis"

3. "Cuando hago seguimiento, necesito recordar todo lo que hemos hablado con el cliente para no preguntar lo mismo dos veces"

### Para Roberto (Director)

1. "Cuando reviso el negocio, necesito ver el estado de todas las búsquedas activas para identificar dónde necesito intervenir"

2. "Cuando hablo con un cliente importante, necesito tener datos de mercado actualizados para sonar como experto"

### Para Carlos (Cliente)

1. "Cuando Beiqa me presenta opciones, necesito poder evaluarlas fácilmente para tomar una decisión informada"

2. "Cuando presento a mi director, necesito materiales profesionales para justificar mi recomendación"

---

## Próximos Pasos

1. [ ] Validar personas con el equipo de Beiqa
2. [ ] Priorizar funcionalidades por persona
3. [ ] Crear user journeys detallados
4. [ ] Diseñar wireframes por persona
5. [ ] Definir métricas de éxito por persona
