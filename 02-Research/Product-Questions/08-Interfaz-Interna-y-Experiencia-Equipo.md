# Cuestionario de Producto: Interfaz Interna y Experiencia del Equipo

**Módulo**: Cómo el equipo Beiqa interactúa con el sistema (app interna)
**Fase**: R-0 (Fundación)
**Estado**: 🔴 Sin responder
**Respondido por**: _______________
**Fecha**: _______________

---

## Contexto

> La arquitectura actual asume una "Internal App Web" (React/Next.js), pero **no está decidido** cómo debe ser la experiencia del equipo. El equipo es mayormente no técnico; la forma en que interactúen con la Plataforma puede ser:
>
> - Un **dashboard web** clásico (búsquedas, filtros, tablas, mapas)
> - Una **interfaz conversacional** (hablar o escribir en lenguaje natural, tipo chatbot o agente IA)
> - Una herramienta tipo **spreadsheet / Airtable** (tablas editables, vistas, filtros simples)
> - Un **híbrido** (por ejemplo: dashboard para explorar + asistente IA para preguntas en lenguaje natural)
>
> **Nomenclatura importante:**
> - **Beiqa** = La empresa, el negocio de brokerage inmobiliario industrial
> - **La Plataforma** = El producto de software que estamos construyendo para Beiqa
>
> Este cuestionario busca definir qué experiencia debe tener el equipo interno antes de que la investigación técnica elija tecnologías (dashboard vs IA vs low-code).
>
> **Ya documentado en:**
> - [Current-Process.md](../../01-Discovery/Current-Process.md) — Cómo trabaja el equipo hoy
> - [Pain-Points.md](../../01-Discovery/Pain-Points.md) — Dolor de búsqueda manual
> - [System-Architecture.md](../../04-Architecture/System-Architecture.md) — Diagrama que muestra "Internal App Web" (supuesto no validado)
>
> **Las preguntas buscan lo que FALTA sobre la forma de trabajo deseada.**

---

## Preguntas

### A. Forma de Trabajo Actual

1. Cuando un miembro del equipo busca propiedades hoy, ¿qué herramientas usa además del navegador? (Excel, notas, WhatsApp, otro.)

   > _Respuesta:_

2. ¿El equipo prefiere trabajar con **listas y tablas** (como Excel) o con **pantallas con filtros y mapas** (como un portal inmobiliario)?

   > _Respuesta:_

3. ¿Alguno del equipo ha usado herramientas tipo Airtable, Notion o bases de datos "visibles" (filas/columnas)? ¿Les resultaría natural algo así?

   > _Respuesta:_

4. ¿Cuántas pestañas/ventanas tiene abiertas típicamente un miembro del equipo cuando trabaja?

   > _Respuesta:_

5. ¿Qué porcentaje del tiempo del equipo se dedica a tareas administrativas vs. tareas de valor (hablar con clientes, negociar)?

   > _Respuesta:_

6. ¿Qué herramientas usa el equipo que ama? ¿Cuáles odia?

   > _Respuesta:_

---

### B. Perfil del Usuario Interno

7. ¿Quiénes exactamente usarán la Plataforma? (Brokers, asistentes, directores, todos)

   > _Respuesta:_

8. ¿Qué tan técnico es cada tipo de usuario? ¿Qué tan cómodos están con tecnología nueva?

   > _Respuesta:_

9. ¿Hay diferencias generacionales en el equipo que afecten la adopción de tecnología?

   > _Respuesta:_

10. ¿Quién en el equipo sería el "early adopter" que prueba primero? ¿Quién sería el más resistente?

    > _Respuesta:_

11. ¿El equipo tiene tiempo para aprender una herramienta nueva, o necesita algo que funcione desde el día uno?

    > _Respuesta:_

---

### C. Búsqueda y Consulta

12. ¿Cómo imaginas la búsqueda ideal? Ejemplo A: "Entro, pongo filtros (zona, m², precio) y veo resultados en tabla y mapa." Ejemplo B: "Escribo o digo: 'bodegas de 2000 m² en Toluca bajo 50 pesos el m²' y el sistema me devuelve opciones." ¿Te identificas más con A, con B, o con ambos según el momento?

    > _Respuesta:_

13. Si pudieras **preguntar en lenguaje natural** ("dame bodegas en el norte de CDMX que lleven más de 30 días publicadas"), ¿lo usarías como método principal o como complemento a filtros y tablas?

    > _Respuesta:_

14. ¿Qué es más importante para el MVP: **velocidad para encontrar propiedades** (aunque la interfaz sea sencilla) o **una interfaz muy clara y visual** (aunque tome más tiempo construirlo)?

    > _Respuesta:_

15. ¿Cuánto tiempo debería tomar encontrar propiedades relevantes? ¿Segundos? ¿Minutos?

    > _Respuesta:_

16. ¿El equipo necesita poder guardar búsquedas frecuentes?

    > _Respuesta:_

17. ¿Qué filtros son los más usados? ¿Cuáles son "nice to have"?

    > _Respuesta:_

---

### D. Dashboard vs Chat / IA

18. Para el primer año, ¿qué te parece más útil: un **dashboard** con mapas, filtros y tablas, o un **chat / asistente** donde escribes lo que necesitas y te responde con propiedades y resúmenes?

    > _Respuesta:_

19. Si el sistema tuviera un **asistente tipo chatbot**: ¿debería solo responder consultas, o también **sugerir cosas proactivamente** (ej.: "Hay 3 propiedades nuevas que coinciden con la búsqueda del cliente X")?

    > _Respuesta:_

20. ¿El equipo estaría dispuesto a **cambiar su flujo** (por ejemplo, pasar de buscar en portales a preguntarle a un asistente) o prefieren que el sistema se parezca al flujo actual (buscar y filtrar) y que la IA sea opcional?

    > _Respuesta:_

21. ¿El equipo confiaría en recomendaciones de IA, o necesita ver todos los datos para decidir?

    > _Respuesta:_

22. ¿Qué combinación de interfaz sería ideal? (Solo dashboard, solo chat, dashboard + chat, otro)

    > _Respuesta:_

---

### E. Dispositivos y Contexto de Uso

23. ¿Dónde se usa principalmente la búsqueda de propiedades? (Oficina con laptop, en movimiento con celular, ambos.)

    > _Respuesta:_

24. ¿Qué tan importante es poder **guardar búsquedas**, **listas** o **shortlists** y volver a ellas después? ¿O suele ser una búsqueda puntual por proyecto?

    > _Respuesta:_

25. ¿El equipo necesita acceso desde el celular? ¿Para qué tareas específicas?

    > _Respuesta:_

26. ¿El equipo trabaja desde casa, oficina, o en campo? ¿Cómo afecta esto el uso de la Plataforma?

    > _Respuesta:_

27. ¿La Plataforma necesita funcionar offline o con conexión lenta?

    > _Respuesta:_

28. ¿El equipo usa la Plataforma durante llamadas con clientes? ¿Necesita ser rápida para consultas en vivo?

    > _Respuesta:_

---

### F. Integración con el Flujo de Trabajo

29. En el flujo típico (cliente pide opciones → equipo busca → arma shortlist → presenta al cliente), ¿en qué momento exacto usarían la Plataforma? (Solo para buscar, para buscar y armar shortlist, para buscar + shortlist + generar el documento para el cliente, otro.)

    > _Respuesta:_

30. ¿Qué entregable debe poder producir el equipo **desde la Plataforma** sin salir a Excel o PowerPoint? (Solo lista de propiedades, lista + mapa, comparativo, reporte de mercado, todo lo anterior.)

    > _Respuesta:_

31. ¿Cómo debería integrarse la Plataforma con HubSpot? ¿Qué datos deberían fluir entre ambos?

    > _Respuesta:_

32. ¿El equipo necesita poder copiar/pegar datos de la Plataforma a otras herramientas?

    > _Respuesta:_

33. ¿Qué tareas hace el equipo en Excel que deberían poder hacerse en la Plataforma?

    > _Respuesta:_

34. ¿El equipo necesita poder importar datos a la Plataforma? ¿De dónde?

    > _Respuesta:_

---

### G. Gestión de Propiedades

35. ¿El equipo necesita poder agregar/editar propiedades manualmente en la Plataforma?

    > _Respuesta:_

36. ¿Quién debería poder editar datos de propiedades? ¿Todos o solo ciertos roles?

    > _Respuesta:_

37. ¿El equipo necesita poder marcar propiedades como "favoritas", "descartadas", "en seguimiento"?

    > _Respuesta:_

38. ¿Cómo debería el equipo agregar notas o comentarios a una propiedad?

    > _Respuesta:_

39. ¿El equipo necesita ver quién más del equipo ha visto o trabajado con una propiedad?

    > _Respuesta:_

---

### H. Gestión de Clientes y Proyectos

40. ¿Cómo debería la Plataforma organizar el trabajo por cliente/proyecto?

    > _Respuesta:_

41. ¿El equipo necesita ver todas las búsquedas activas de todos los clientes, o solo las propias?

    > _Respuesta:_

42. ¿Cómo se asignan clientes/proyectos a miembros del equipo?

    > _Respuesta:_

43. ¿El equipo necesita colaborar en el mismo proyecto? ¿Cómo?

    > _Respuesta:_

44. ¿Qué información del cliente debería estar visible mientras se buscan propiedades?

    > _Respuesta:_

---

### I. Shortlists y Presentaciones

45. ¿Cómo arma el equipo una shortlist hoy? ¿Qué pasos involucra?

    > _Respuesta:_

46. ¿Qué información debe incluir una shortlist?

    > _Respuesta:_

47. ¿El equipo necesita poder ordenar/priorizar propiedades en una shortlist?

    > _Respuesta:_

48. ¿Cómo debería verse la presentación final para el cliente? ¿Qué formato?

    > _Respuesta:_

49. ¿El equipo necesita poder personalizar las presentaciones por cliente?

    > _Respuesta:_

50. ¿Cuánto tiempo debería tomar generar una presentación desde la Plataforma?

    > _Respuesta:_

---

### J. Notificaciones y Alertas

51. ¿Qué notificaciones serían útiles para el equipo? (Nuevas propiedades, cambios de precio, actividad de clientes)

    > _Respuesta:_

52. ¿Por qué canal deberían llegar las notificaciones? (Email, app, WhatsApp, SMS)

    > _Respuesta:_

53. ¿Con qué frecuencia deberían llegar las notificaciones? ¿Hay riesgo de "fatiga"?

    > _Respuesta:_

54. ¿El equipo necesita poder configurar qué notificaciones recibe?

    > _Respuesta:_

---

### K. Reportes y Análisis

55. ¿Qué reportes necesita el equipo para su trabajo diario?

    > _Respuesta:_

56. ¿El equipo necesita ver métricas de su propio desempeño? (Propiedades revisadas, clientes atendidos, deals cerrados)

    > _Respuesta:_

57. ¿La dirección necesita reportes agregados del equipo?

    > _Respuesta:_

58. ¿Qué dashboards o vistas resumen serían más útiles?

    > _Respuesta:_

---

### L. Personalización y Configuración

59. ¿El equipo necesita poder personalizar su vista de la Plataforma? (Columnas, filtros por defecto, tema)

    > _Respuesta:_

60. ¿Diferentes miembros del equipo necesitan ver cosas diferentes?

    > _Respuesta:_

61. ¿El equipo necesita poder crear vistas o filtros guardados?

    > _Respuesta:_

62. ¿Qué configuraciones deberían ser globales vs. por usuario?

    > _Respuesta:_

---

### M. Riesgos y Adopción

63. ¿Qué haría que el equipo **no use** la Plataforma? (Muy lenta, muy complicada, no confían en los datos, prefieren su Excel, otro.)

    > _Respuesta:_

64. ¿Qué **una cosa** debería hacer la interfaz excepcionalmente bien para que todos la adopten?

    > _Respuesta:_

65. ¿Qué entrenamiento necesitaría el equipo para usar la Plataforma?

    > _Respuesta:_

66. ¿Quién daría soporte al equipo cuando tengan dudas o problemas?

    > _Respuesta:_

67. ¿Cómo se mediría si el equipo está usando la Plataforma efectivamente?

    > _Respuesta:_

---

### N. Experiencia de Usuario

68. ¿Qué tan importante es que la Plataforma sea "bonita" vs. "funcional"?

    > _Respuesta:_

69. ¿El equipo prefiere muchas opciones y flexibilidad, o una interfaz simple con menos decisiones?

    > _Respuesta:_

70. ¿Qué aplicaciones usa el equipo que tienen una buena experiencia de usuario? ¿Qué les gusta de ellas?

    > _Respuesta:_

71. ¿Qué frustraciones tiene el equipo con las herramientas actuales que la Plataforma debería evitar?

    > _Respuesta:_

---

### O. Priorización y MVP

72. ¿Qué funcionalidad de la interfaz es imprescindible para el MVP?

    > _Respuesta:_

73. ¿Qué funcionalidad puede esperar a versiones futuras?

    > _Respuesta:_

74. ¿Qué problema de la interfaz, si se resolviera, cambiaría más el día a día del equipo?

    > _Respuesta:_

75. ¿Es más importante tener muchas funcionalidades básicas o pocas funcionalidades muy bien hechas?

    > _Respuesta:_

---

## Notas Adicionales

> _Notas:_
