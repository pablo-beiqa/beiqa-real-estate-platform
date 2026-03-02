# Tenant Portal — Product Questions

> Cuestionario de discovery del modulo. Las respuestas provienen de entrevista con Pablo (CEO) el 2026-03-02.
>
> **Estado**: ✅ Respondido

---

## 1. General

**¿Quien es el usuario del portal?**
> Clientes corporativos de Beiqa (tenants) — empresas que buscan espacios comerciales o industriales. Perfil mixto: a veces el CEO/C-level toma la decision final, a veces es el equipo de facilities/real estate. El portal debe servir a ambos perfiles.

**¿Cuantos clientes activos hay?**
> 5-15 clientes corporativos activos simultaneamente.

**¿Desde que dispositivos acceden?**
> Mobile y desktop con importancia igual desde dia 1. Los clientes necesitan poder revisar propiedades tanto desde la oficina como en campo.

**¿Que ve el cliente?**
> Solo su propia informacion. Aislamiento estricto — ningun cliente puede ver datos de otro cliente. El scoring se muestra completo (transparencia total).

**¿Cual es la calidad de los datos de propiedades?**
> Parcial. Algunas propiedades tienen datos completos (fotos, precio, m2, ubicacion, descripcion), pero muchas les faltan fotos o precio. El portal debe manejar campos vacios gracefully.

---

## 2. Autenticacion y Acceso

**¿Como accede el cliente al portal?**
> Aun no decidido al momento de la entrevista, pero la inclinacion es **magic link** como metodo primario (click en link del email → entra directo). Password como fallback porque algunos clientes solo tienen Gmail y prefieren credenciales tradicionales.

**¿Que proveedor de auth?**
> Supabase Auth — ya configurado en beiqa-frontend con `@supabase/ssr`.

**¿Hay roles o permisos?**
> Un solo rol: "cliente/tenant". El equipo Beiqa usa la Internal App (herramienta separada). RLS (Row Level Security) para aislamiento por cliente.

---

## 3. Scoring

**¿Que es un scoring?**
> Documento estructurado con los criterios de busqueda del cliente: tipo de propiedad, subtipo, operacion, filtros de elegibilidad, criterios con pesos (suman 100 puntos), especificaciones tecnicas, condiciones comerciales, y zonas objetivo. Se genera a partir de llamadas con el cliente (via Circleback).

**¿Quien construye el scoring?**
> El equipo Beiqa, basado en reuniones con el cliente. Se genera via Claude agent (score-discovery) que procesa transcripts de Circleback.

**¿El cliente ve el scoring completo?**
> Si, transparencia total. El cliente ve criterios, pesos, rubrica, zonas, especificaciones — todo el detalle.

**¿El cliente puede modificar el scoring?**
> No en Phase 0. En el futuro, podria aprobar/comentar el scoring desde el portal.

---

## 4. Shortlist

**¿Que contiene una shortlist?**
> Propiedades recomendadas + score por criterio del scoring del cliente. Cada propiedad tiene una evaluacion basada en los criterios definidos en el scoring (zona ✓, precio ✓, tamano ✗, etc.).

**¿Quien selecciona las propiedades?**
> Seleccion manual por el equipo Beiqa inicialmente. El plan es evolucionar hacia semi-automatico (filtrar por criterios + revision humana) y eventualmente automatico (AI matching).

**¿Cuantas propiedades por shortlist?**
> No definido formalmente. Tipicamente 5-15 propiedades por ronda de presentacion.

---

## 5. Feedback

**¿Que tipo de feedback captura el portal?**
> Feedback estructurado por propiedad:
> - **Evaluacion**: Si / No / Quiza (me interesa / no me interesa / necesito mas informacion)
> - **Razon**: Seleccion de opciones predefinidas (precio, ubicacion, tamano, condiciones, infraestructura, etc.)
> - **Comentario**: Campo de texto libre opcional para detalles adicionales

**¿El feedback alimenta las busquedas futuras?**
> Si — el equipo usa el feedback para refinar los criterios de busqueda y mejorar las siguientes rondas de shortlist.

---

## 6. Mapas e Inteligencia de Mercado

**¿Que vision hay para los mapas?**
> Estrategia progresiva:
> 1. **Fase basica**: Markers de propiedades del shortlist sobre mapa
> 2. **Fase contexto**: Capas de informacion — POIs, transporte, competidores, datos socioeconomicos
> 3. **Fase analisis**: Heatmaps de precio/m2, densidad de oferta, comparativos por zona

**¿Que proveedor de mapas?**
> Atlas.co — decidido por API y capacidad de embed. Pendiente evaluacion de pricing.

**¿Que datos de mercado ve el cliente?**
> Analisis completo (vision a futuro):
> - Precio/m2 promedio por zona
> - Comparativos de zonas (Zona A vs Zona B vs Zona C)
> - Tendencias de precio historicas
> - Datos socioeconomicos de la zona (INEGI)
> - Exportar analisis como PDF descargable

---

## 7. Integraciones

**¿Que integraciones son criticas?**
> - **Supabase** (core): DB, Auth, Storage, REST API — ya configurado
> - **Atlas.co** (mapas): API + embed — por implementar
> - **Email/Slack** (notificaciones): Avisar al equipo cuando cliente da feedback, avisar al cliente cuando hay nuevo shortlist
> - **HubSpot** (sync): Datos de scoring via pipeline de Circleback — ya configurado parcialmente

---

## 8. Diseno y UX

**¿Hay wireframes o mockups?**
> No. Functionality first. Se construye directamente en codigo con shadcn/ui + Tailwind.

**¿Hay design system?**
> En construccion (separado). El Tenant Portal usa shadcn/ui como base de componentes.

**¿Hay branding definido?**
> En proceso. Se tomaran referencias del design system conforme avance.

---

## 9. Proceso de Negocio Actual

**¿Como funciona hoy el proceso de compartir propiedades?**
> 1. Reunion con cliente para entender necesidades ("¿donde buscan?", "¿que necesitan?")
> 2. Se arma un scoring con criterios, parametros y caracteristicas
> 3. Se envia scoring al cliente para aprobacion
> 4. Si el cliente aprueba, se buscan propiedades
> 5. Se construye una presentacion (manual en PowerPoint o parcialmente con Claude)
> 6. Se comparte por email
> 7. Feedback llega por multiples canales (email, llamada, WhatsApp)
> 8. Se itera hasta seleccionar propiedades para visitas

**¿Que quiere automatizar el Tenant Portal?**
> Pasos 3-7: que el cliente vea scoring y propiedades directamente en el portal, de feedback estructurado, y el equipo reciba notificaciones. Eliminar la necesidad de PowerPoint y el ida y vuelta por email.

---

## 10. Gestion de Issues

**¿Donde se gestionan las tareas?**
> Issues centralizados en [beiqa-real-estate-platform](https://github.com/pablo-beiqa/beiqa-real-estate-platform/issues) con labels que diferencian destino (`frontend`, `data`, `docs`, etc.). Mismo backlog para todos los repos.

---

*Entrevista realizada: 2026-03-02 | Entrevistado: Pablo (CEO)*
