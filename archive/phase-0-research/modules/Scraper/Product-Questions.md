# Cuestionario de Producto: Scraper y Adquisicion de Datos

**Modulo**: Adquisicion de datos de propiedades
**Fase**: R-0 (Fundacion)
**Estado**: 🟡 Parcialmente respondido
**Respondido por**: Pablo Beitman
**Fecha**: 2026-02-25

---

## Contexto

> Este cuestionario busca entender como el equipo obtiene datos de propiedades HOY y que necesita manana.
>
> **Nomenclatura importante:**
> - **Beiqa** = La empresa, el negocio de brokerage inmobiliario industrial
> - **La Plataforma** = El producto de software que estamos construyendo para Beiqa
>
> **Ya documentado en:**
> - [Acquisition-Strategy.md](./Research/Acquisition-Strategy.md) — Estrategia de adquisicion con decisiones tomadas
> - [README.md](./README.md) — Estado actual del modulo

---

## Preguntas

### A. Uso Actual de Portales

1. ¿Cuales portales usa el equipo HOY de manera regular? Ordenalos de mas usado a menos usado.

   > **Respuesta**: Inmuebles24 (principal, en produccion via Apify), Pincali (pincali.com), CBRE (seccion Mexico), Colliers (seccion Mexico). Estos son los 4 portales del MVP.

2. ¿Cuantos listings revisa el equipo manualmente por proyecto? ¿Por semana?

   > _Respuesta: Pendiente_

3. Cuando buscas en un portal, ¿que filtros aplicas? Describe tu proceso tipico de busqueda paso a paso.

   > _Respuesta: Pendiente_

4. ¿Como decide el equipo que un listing "vale la pena guardar"? ¿Que hace relevante a una propiedad?

   > _Respuesta: Pendiente_

5. ¿Hay portales que el equipo NO usa pero deberia? ¿Por que no los usa hoy?

   > **Respuesta**: Se evaluaron EasyBroker, Vivanuncios, Lamudi, Metros Cubicos, Propiedades.com, Solili y LoopNet. Ninguno es prioridad para MVP. Solili podria sumarse post-MVP por su enfoque industrial.

6. ¿Cuanto tiempo dedica el equipo semanalmente a buscar en portales? ¿Es tiempo bien invertido o desperdiciado?

   > _Respuesta: Pendiente_

---

### B. Metodos de Captura

7. Si una extension de Chrome pudiera capturar propiedades con un clic mientras navegas, ¿seria util? ¿Como la usarias?

   > **Respuesta**: No aplica para el MVP. La captura es automatizada via Apify (Inmuebles24) y TriggerDev (los demas portales).

8. ¿Prefieres que el sistema capture TODAS las propiedades automaticamente y tu filtres despues, o que solo capture las que tu selecciones?

   > **Respuesta**: Captura automatica de todas las propiedades comerciales/industriales. El filtrado se hace post-captura.

9. ¿El equipo alguna vez descarga datos de portales en CSV/Excel? ¿O todo es copiado manual?

   > _Respuesta: Pendiente_

10. ¿Como captura el equipo las fotos de las propiedades hoy? ¿Las guarda? ¿Donde?

    > **Respuesta**: Las imagenes se descargan y almacenan en Supabase Storage. Se mantiene referencia a la URL original como fallback.

11. ¿Que informacion se pierde tipicamente cuando se copia manualmente de un portal? ¿Que datos se olvidan?

    > **Respuesta**: Datos ocultos en la descripcion (m², altura de techo, seguridad, antiguedad). El pipeline LLM se encarga de extraer estos campos automaticamente.

---

### C. Calidad y Completitud de Datos

12. ¿Que pasa cuando un listing tiene datos incompletos o incorrectos? ¿Como lo maneja el equipo?

    > **Respuesta**: El pipeline detecta anomalias (coordenadas falsas, precios atipicos, datos inconsistentes). Las anomalias se flaggean para revision humana y se notifica via Slack.

13. ¿Que tan importante es tener los datos COMPLETOS del listing vs. solo lo basico (ubicacion, precio, superficie)?

    > _Respuesta: Pendiente_

14. ¿Que datos de un listing NUNCA estan disponibles en portales pero el equipo los conoce por experiencia?

    > _Respuesta: Pendiente_

15. ¿Que tan confiables son los precios publicados en portales? ¿Reflejan la realidad del mercado?

    > _Respuesta: Pendiente_

16. ¿Que porcentaje de listings tienen informacion enganosa o incorrecta? ¿Como lo detecta el equipo?

    > **Respuesta**: Se detectan anomalias como: anunciante es persona fisica vs empresa, coordenadas que no corresponden a la ubicacion real, datos de superficie en descripcion que no coinciden con campos estructurados. Clay (hoy) y TriggerDev (futuro) usan LLM para analizar estas inconsistencias.

17. ¿Que datos son criticos verificar antes de presentar una propiedad a un cliente?

    > _Respuesta: Pendiente_

---

### D. Seguimiento y Duplicados

18. ¿Como trackea el equipo que propiedades ya reviso vs. cuales son nuevas?

    > **Respuesta**: El sistema verifica en HubSpot + Supabase si la propiedad ya existe (por ID externo / URL). Si existe, solo actualiza. Si es nueva, la crea.

19. ¿Con que frecuencia ves la MISMA propiedad publicada por diferentes agentes en el mismo portal? ¿Es un problema real?

    > _Respuesta: Pendiente_

20. ¿Cual es el ciclo de vida tipico de un listing?

    > _Respuesta: Pendiente_

21. ¿Como identificas que dos listings en diferentes portales son la misma propiedad?

    > **Respuesta**: Esto es responsabilidad del modulo Data / Normalization (deduplicacion cross-portal). No es parte del scraper individual.

22. ¿Que pasa cuando una propiedad desaparece de un portal?

    > **Respuesta**: Se marca como no disponible de inmediato en ambos destinos (HubSpot + Supabase). No se elimina, solo cambia de estado.

---

### E. Frecuencia y Volumen

23. ¿Con que frecuencia necesita actualizarse la informacion?

    > **Respuesta**: Depende del portal. CBRE y Colliers: semanal (bajo volumen, cambios frecuentes). Inmuebles24 y Pincali: mensual (alto volumen).

24. ¿Cuantas propiedades activas crees que hay en CDMX en el segmento comercial/industrial?

    > **Respuesta**: Inmuebles24 tiene ~30-40k propiedades. Pincali tiene volumen similar. CBRE y Colliers tienen cientos cada uno. Total estimado: 60-80k+ listings activos.

25. ¿Cuantas propiedades NUEVAS aparecen tipicamente por semana en los portales que usa el equipo?

    > _Respuesta: Pendiente_

26. ¿Hay temporadas o ciclos donde hay mas/menos actividad en los portales?

    > _Respuesta: Pendiente_

---

### F. Mas Alla de los Portales

27. ¿El equipo recibe informacion de propiedades por otros canales?

    > _Respuesta: Pendiente_

28. ¿Hay propiedades "off-market" que nunca aparecen en portales pero el equipo conoce?

    > _Respuesta: Pendiente_

29. ¿Que porcentaje de las propiedades que maneja el equipo vienen de portales vs. otros canales?

    > _Respuesta: Pendiente_

30. ¿El equipo tiene relaciones con otros brokers que comparten informacion?

    > _Respuesta: Pendiente_

31. ¿Hay propiedades que el equipo conoce por conocimiento local?

    > _Respuesta: Pendiente_

---

### G. Cobertura Geografica

32. ¿Que zonas geograficas son prioritarias para la captura de datos?

    > _Respuesta: Pendiente_

33. ¿Hay zonas donde los portales tienen poca cobertura?

    > _Respuesta: Pendiente_

34. ¿Deberia la Plataforma enfocarse en ciertas zonas primero, o capturar todo Mexico desde el inicio?

    > _Respuesta: Pendiente_

---

### H. Tipos de Propiedad

35. ¿Que tipos de propiedad son prioritarios?

    > **Respuesta**: Industrial y comercial son prioridad. Los 4 portales seleccionados cubren bien estos segmentos.

36. ¿Los portales que usa el equipo cubren bien todos los tipos?

    > **Respuesta**: Inmuebles24 y Pincali cubren ampliamente. CBRE y Colliers se enfocan en propiedades corporativas de mayor tamano.

37. ¿Hay tipos de propiedad que el equipo NO maneja pero podria manejar?

    > _Respuesta: Pendiente_

---

### I. Aspectos Legales y Eticos

38. ¿Hay portales que explicitamente prohiben el scraping?

    > **Respuesta**: Por investigar como parte del milestone de Investigacion y Analisis. Postura general: respetar robots.txt, rate limiting conservador, uso interno exclusivamente.

39. ¿Como deberia la Plataforma manejar datos de propiedades donde el equipo NO es el broker listador?

    > _Respuesta: Pendiente_

40. ¿Hay sensibilidad sobre que datos se pueden almacenar vs. solo consultar?

    > **Respuesta**: Solo se almacenan datos publicos del listing. Imagenes se descargan para referencia interna. No se redistribuye ni revende datos.

---

### J. Priorizacion y MVP

41. Si la Plataforma solo pudiera capturar datos de 3 portales al inicio, ¿cuales serian?

    > **Respuesta**: Se eligieron 4: Inmuebles24, Pincali, CBRE, Colliers. Inmuebles24 ya esta en produccion. Los otros 3 se desarrollan en paralelo.

42. ¿Que es mas valioso: capturar MUCHAS propiedades con datos basicos, o POCAS propiedades con datos completos?

    > **Respuesta**: Muchas propiedades con enriquecimiento automatico. El pipeline LLM extrae datos adicionales de la descripcion para tener datos lo mas completos posible.

43. ¿Que funcionalidad de adquisicion de datos es imprescindible para el MVP?

    > **Respuesta**: Extraccion automatica de los 4 portales, normalizacion basica, escritura dual a HubSpot + Supabase, deteccion de anomalias, y telefono del anunciante siempre en la propiedad.

44. ¿Que problema de adquisicion de datos, si se resolviera, cambiaria mas el dia a dia del equipo?

    > **Respuesta**: Eliminar la busqueda manual en multiples portales. Que todas las propiedades lleguen automaticamente a HubSpot y Supabase listas para trabajar.

---

## Notas Adicionales

> - Las respuestas marcadas "Pendiente" se completaran en sesiones futuras con el equipo operativo
> - Las decisiones tecnicas estan documentadas en [Acquisition-Strategy.md](./Research/Acquisition-Strategy.md)
> - La logica diferenciada de brokers (multi-broker vs corporativo) esta detallada en el [README](./README.md)
