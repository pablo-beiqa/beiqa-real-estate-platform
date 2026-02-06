# Cuestionario de Producto: Base de Datos

**Módulo**: Base de datos central y estructura de información
**Fase**: R-0 (Fundación)
**Estado**: 🔴 Sin responder
**Respondido por**: _______________
**Fecha**: _______________

---

## Contexto

> La base de datos es el corazón de BEIQA. Necesitamos entender qué información almacenar, cómo se busca y cómo se relaciona para diseñar la estructura correcta.
>
> **Ya documentado en:**
> - [Data-Model.md](../../03-Requirements/Data-Requirements/Data-Model.md) -- Modelo de datos propuesto (PROPERTY, CLIENT, ZONE, etc.)
> - [System-Architecture.md](../../04-Architecture/System-Architecture.md) -- PostgreSQL + PostGIS como decisión preliminar
>
> **Las preguntas buscan validar y profundizar el modelo desde la perspectiva del usuario.**

---

## Preguntas

### Búsquedas y Consultas

1. ¿Cuáles son las búsquedas más comunes que el equipo realiza? (¿Por zona? ¿Por tamaño? ¿Por precio? ¿Por tipo?) Ordena de más frecuente a menos frecuente.

   > _Respuesta:_

2. ¿Cómo combina el equipo filtros cuando busca? Ejemplo: "bodega de 1000-2000m2 en corredor Toluca-Lerma, renta menor a $X/m2"

   > _Respuesta:_

3. ¿El equipo busca por cercanía a puntos específicos? (Ej: "propiedades a menos de 30 min del aeropuerto", "bodegas cerca de la autopista X")

   > _Respuesta:_

### Información de Propiedades

4. ¿Qué información de una propiedad es OBLIGATORIA almacenar vs. qué es opcional?

   > _Respuesta:_

5. ¿Cómo trackea el equipo actualmente el historial de una propiedad? (Cambios de precio, cambios de estatus, cuándo se rentó/vendió)

   > _Respuesta:_

6. ¿Qué campos son específicos de propiedades industriales vs. oficinas vs. retail vs. terrenos?

   > _Respuesta:_

### Zonas y Geografía

7. ¿Cómo categoriza el equipo las zonas? (¿Por ciudad? ¿Por corredor industrial? ¿Por colonia? ¿Por áreas custom?)

   > _Respuesta:_

8. ¿Hay zonas que el equipo ha definido informalmente que no corresponden a divisiones administrativas oficiales? (Ej: "zona norte de CDMX", "corredor Querétaro")

   > _Respuesta:_

### Relaciones y Contexto

9. ¿Cuántas propiedades trackea el equipo activamente en un momento dado?

   > _Respuesta:_

10. ¿Qué datos de HubSpot necesitan ser accesibles junto con los datos de propiedades? (¿Nombre del cliente? ¿Etapa del deal? ¿Notas?)

    > _Respuesta:_

11. ¿Cómo vincula el equipo los requerimientos de un cliente con las propiedades que le corresponden hoy?

    > _Respuesta:_

### Comparaciones y Análisis

12. ¿Qué comparaciones hace el equipo entre propiedades? ¿Qué campos son los más importantes para comparar?

    > _Respuesta:_

13. ¿Se necesita guardar versiones históricas de los datos? (Ej: el precio hace 3 meses vs. hoy)

    > _Respuesta:_

14. ¿Qué reportes o vistas agregadas son más útiles? (Ej: "total de propiedades por zona", "precio promedio por tipo", "nuevos listings esta semana")

    > _Respuesta:_

---

## Notas Adicionales

> _Notas:_
