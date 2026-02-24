# Cuestionario de Producto: Base de Datos

**Módulo**: Base de datos central y estructura de información
**Fase**: R-0 (Fundación)
**Estado**: 🔴 Sin responder
**Respondido por**: _______________
**Fecha**: _______________

---

## Contexto

> La base de datos es el corazón de la Plataforma. Necesitamos entender qué información almacenar, cómo se busca y cómo se relaciona para diseñar la estructura correcta.
>
> **Nomenclatura importante:**
> - **Beiqa** = La empresa, el negocio de brokerage inmobiliario industrial
> - **La Plataforma** = El producto de software que estamos construyendo para Beiqa
>
> **Ya documentado en:**
> - [Data-Model.md](./Data-Model.md) — Modelo de datos propuesto (PROPERTY, CLIENT, ZONE, etc.)
> - [System-Architecture.md](../System-Architecture.md) — PostgreSQL + PostGIS como decisión preliminar
>
> **Las preguntas buscan validar y profundizar el modelo desde la perspectiva del usuario.**

---

## Preguntas

### A. Búsquedas y Consultas

1. ¿Cuáles son las búsquedas más comunes que el equipo realiza? (¿Por zona? ¿Por tamaño? ¿Por precio? ¿Por tipo?) Ordena de más frecuente a menos frecuente.

   > _Respuesta:_

2. ¿Cómo combina el equipo filtros cuando busca? Ejemplo: "bodega de 1000-2000m2 en corredor Toluca-Lerma, renta menor a $X/m2"

   > _Respuesta:_

3. ¿El equipo busca por cercanía a puntos específicos? (Ej: "propiedades a menos de 30 min del aeropuerto", "bodegas cerca de la autopista X")

   > _Respuesta:_

4. ¿Qué búsquedas hace el equipo hoy que son difíciles o imposibles? ¿Qué les gustaría poder buscar?

   > _Respuesta:_

5. ¿Qué tan importante es la velocidad de búsqueda? ¿Segundos? ¿Milisegundos? ¿O puede tardar un poco si es más completa?

   > _Respuesta:_

6. ¿El equipo necesita guardar búsquedas frecuentes para reutilizarlas?

   > _Respuesta:_

---

### B. Información de Propiedades

7. ¿Qué información de una propiedad es OBLIGATORIA almacenar vs. qué es opcional?

   > _Respuesta:_

8. ¿Cómo trackea el equipo actualmente el historial de una propiedad? (Cambios de precio, cambios de estatus, cuándo se rentó/vendió)

   > _Respuesta:_

9. ¿Qué campos son específicos de propiedades industriales vs. oficinas vs. retail vs. terrenos?

   > _Respuesta:_

10. ¿Qué información de contacto se guarda por propiedad? (Broker, propietario, administrador)

    > _Respuesta:_

11. ¿Cómo se maneja cuando una propiedad tiene múltiples espacios disponibles? (Ej: un edificio con 3 pisos en renta)

    > _Respuesta:_

12. ¿Qué datos de la propiedad cambian frecuentemente vs. cuáles son estáticos?

    > _Respuesta:_

13. ¿Cómo se maneja la información de amenidades, características especiales, o restricciones?

    > _Respuesta:_

---

### C. Zonas y Geografía

14. ¿Cómo categoriza el equipo las zonas? (¿Por ciudad? ¿Por corredor industrial? ¿Por colonia? ¿Por áreas custom?)

    > _Respuesta:_

15. ¿Hay zonas que el equipo ha definido informalmente que no corresponden a divisiones administrativas oficiales? (Ej: "zona norte de CDMX", "corredor Querétaro")

    > _Respuesta:_

16. ¿Las zonas tienen jerarquía? (País > Estado > Ciudad > Corredor > Subzona)

    > _Respuesta:_

17. ¿Quién debería poder crear o modificar zonas en la Plataforma?

    > _Respuesta:_

18. ¿Las zonas cambian con el tiempo? ¿Se expanden los corredores industriales?

    > _Respuesta:_

---

### D. Relaciones y Contexto

19. ¿Cuántas propiedades trackea el equipo activamente en un momento dado?

    > _Respuesta:_

20. ¿Qué datos de HubSpot necesitan ser accesibles junto con los datos de propiedades? (¿Nombre del cliente? ¿Etapa del deal? ¿Notas?)

    > _Respuesta:_

21. ¿Cómo vincula el equipo los requerimientos de un cliente con las propiedades que le corresponden hoy?

    > _Respuesta:_

22. ¿Una propiedad puede estar asociada a múltiples clientes simultáneamente? ¿Cómo se maneja?

    > _Respuesta:_

23. ¿Cómo se relacionan las propiedades con los parques industriales o desarrollos donde están ubicadas?

    > _Respuesta:_

24. ¿El equipo necesita trackear relaciones entre propietarios, desarrolladores, y propiedades?

    > _Respuesta:_

---

### E. Historial y Versiones

25. ¿Se necesita guardar versiones históricas de los datos? (Ej: el precio hace 3 meses vs. hoy)

    > _Respuesta:_

26. ¿Qué tan atrás necesita ir el historial? ¿Meses? ¿Años?

    > _Respuesta:_

27. ¿Quién necesita ver el historial? ¿Solo el equipo interno o también los clientes?

    > _Respuesta:_

28. ¿Qué eventos importantes de una propiedad deberían quedar registrados? (Cambio de precio, visita, oferta, cierre)

    > _Respuesta:_

29. ¿El equipo necesita poder "deshacer" cambios o volver a versiones anteriores?

    > _Respuesta:_

---

### F. Comparaciones y Análisis

30. ¿Qué comparaciones hace el equipo entre propiedades? ¿Qué campos son los más importantes para comparar?

    > _Respuesta:_

31. ¿Qué reportes o vistas agregadas son más útiles? (Ej: "total de propiedades por zona", "precio promedio por tipo", "nuevos listings esta semana")

    > _Respuesta:_

32. ¿El equipo necesita comparar propiedades con "comparables" (comps) históricos?

    > _Respuesta:_

33. ¿Qué métricas calculadas son útiles? (Precio por m², días en mercado, descuento vs. precio original)

    > _Respuesta:_

---

### G. Clientes y Requerimientos

34. ¿Qué información de un cliente/proyecto se necesita almacenar?

    > _Respuesta:_

35. ¿Cómo se definen los requerimientos de un cliente? ¿Qué campos tiene un "requerimiento"?

    > _Respuesta:_

36. ¿Un cliente puede tener múltiples búsquedas activas simultáneamente?

    > _Respuesta:_

37. ¿Cómo se trackea el progreso de un cliente? (Búsqueda > Shortlist > Visitas > Negociación > Cierre)

    > _Respuesta:_

38. ¿Qué información del cliente viene de HubSpot vs. qué se captura en la Plataforma?

    > _Respuesta:_

---

### H. Contactos y Relaciones Comerciales

39. ¿Qué tipos de contactos necesita almacenar la Plataforma? (Propietarios, brokers, administradores, desarrolladores)

    > _Respuesta:_

40. ¿Cómo se relacionan los contactos con las propiedades? ¿Un contacto puede tener múltiples propiedades?

    > _Respuesta:_

41. ¿El equipo necesita trackear el historial de interacciones con contactos?

    > _Respuesta:_

42. ¿Hay información sensible de contactos que requiere permisos especiales para ver?

    > _Respuesta:_

---

### I. Documentos y Archivos

43. ¿Qué documentos se asocian a una propiedad? (Fotos, planos, fichas técnicas, contratos)

    > _Respuesta:_

44. ¿Dónde viven estos documentos hoy? ¿Deberían migrarse a la Plataforma?

    > _Respuesta:_

45. ¿Qué documentos son confidenciales vs. cuáles se pueden compartir con clientes?

    > _Respuesta:_

46. ¿El equipo necesita versionar documentos? (Ej: ficha técnica v1, v2, v3)

    > _Respuesta:_

---

### J. Permisos y Acceso

47. ¿Todos los miembros del equipo deberían ver todos los datos, o hay información restringida?

    > _Respuesta:_

48. ¿Hay datos que solo ciertos roles deberían poder editar?

    > _Respuesta:_

49. ¿Los clientes (en el portal de tenants) deberían ver un subconjunto de los datos?

    > _Respuesta:_

50. ¿Se necesita auditoría de quién vio o modificó qué datos?

    > _Respuesta:_

---

### K. Calidad de Datos

51. ¿Cómo se asegura hoy que los datos estén correctos y actualizados?

    > _Respuesta:_

52. ¿Qué pasa cuando se detecta un error en los datos? ¿Quién lo corrige?

    > _Respuesta:_

53. ¿Debería la Plataforma validar datos automáticamente? (Ej: precio fuera de rango, dirección inválida)

    > _Respuesta:_

54. ¿Qué datos son más propensos a errores o desactualización?

    > _Respuesta:_

---

### L. Priorización y MVP

55. ¿Qué entidades son imprescindibles para el MVP? (Propiedades, Clientes, Zonas, Contactos, etc.)

    > _Respuesta:_

56. ¿Qué funcionalidad de búsqueda es crítica desde el día uno?

    > _Respuesta:_

57. ¿Qué datos existentes necesitan migrarse a la Plataforma? ¿De dónde vienen?

    > _Respuesta:_

58. ¿Qué complejidad de datos puede esperar a fases posteriores?

    > _Respuesta:_

---

## Notas Adicionales

> _Notas:_
