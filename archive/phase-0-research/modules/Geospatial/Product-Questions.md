# Cuestionario de Producto: Geoespacial y GIS

**Módulo**: Mapas, polígonos, zonas, análisis geográfico
**Fase**: R-0 (Fundación)
**Estado**: 🔴 Sin responder
**Respondido por**: _______________
**Fecha**: _______________

---

## Contexto

> El mapa es central a cómo el equipo quiere trabajar. Este cuestionario busca entender exactamente cómo se usarían las capacidades geoespaciales.
>
> **Nomenclatura importante:**
> - **Beiqa** = La empresa, el negocio de brokerage inmobiliario industrial
> - **La Plataforma** = El producto de software que estamos construyendo para Beiqa
>
> **Ya documentado en:**
> - [System-Architecture.md](../../02-Architecture/System-Architecture.md) — Geospatial Analysis Engine, PostGIS
> - [Google-Maps-Platform.md](./Research/Google-Maps-Platform.md) — APIs de Google Maps evaluadas
>
> **Las preguntas buscan entender el USO REAL de mapas y análisis geográfico por el equipo.**

---

## Preguntas

### A. Uso Actual de Mapas

1. Cuando el equipo usa Google Maps hoy, ¿qué están buscando? (¿Distancia a carreteras? ¿Negocios cercanos? ¿Zonas de inundación? ¿Accesos?)

   > _Respuesta:_

2. ¿Con qué frecuencia se usan mapas en el proceso de trabajo? ¿En qué momentos del proceso de un proyecto son más útiles?

   > _Respuesta:_

3. ¿Qué herramientas de mapas usa el equipo además de Google Maps? (Google Earth, Waze, otras)

   > _Respuesta:_

4. ¿El equipo usa Street View? ¿Para qué?

   > _Respuesta:_

5. ¿Qué limitaciones tiene el uso actual de mapas? ¿Qué no pueden hacer que les gustaría?

   > _Respuesta:_

---

### B. Polígonos y Zonas

6. Si pudieras dibujar un polígono en un mapa y ver todas las propiedades dentro de él, ¿cómo lo usarías?

   > _Respuesta:_

7. ¿Cómo deberían definirse las zonas/submercados geográficamente? ¿Por municipio? ¿Por polígonos custom? ¿Por código postal? ¿Por corredor industrial?

   > _Respuesta:_

8. ¿Hay zonas que el equipo ya ha definido informalmente? (Ej: "corredor Toluca-Lerma", "zona norte industrial") ¿Se podrían dibujar en un mapa?

   > _Respuesta:_

9. ¿Quién debería poder crear o modificar zonas en la Plataforma?

   > _Respuesta:_

10. ¿Las zonas deberían poder solaparse? (Ej: una propiedad puede estar en "CDMX Norte" y también en "Corredor Vallejo")

    > _Respuesta:_

11. ¿Cómo se nombran las zonas? ¿Hay nomenclatura estándar o cada quien las llama diferente?

    > _Respuesta:_

---

### C. Puntos de Interés y Proximidad

12. ¿Qué "puntos de interés" son más importantes para inquilinos comerciales/industriales? (Carreteras, aeropuertos, puertos, ferrocarril, competidores, proveedores, centros de distribución)

    > _Respuesta:_

13. ¿Cómo influye el análisis de proximidad en las recomendaciones de propiedades? ¿Qué distancias/tiempos importan?

    > _Respuesta:_

14. ¿Los clientes alguna vez piden análisis de isócronas (qué es alcanzable en X minutos)?

    > _Respuesta:_

15. ¿Qué puntos de interés específicos piden los clientes? (Cercanía a su planta actual, a clientes, a proveedores)

    > _Respuesta:_

16. ¿Es más importante la distancia en kilómetros o el tiempo de traslado?

    > _Respuesta:_

17. ¿El equipo necesita calcular rutas de transporte de carga? ¿Con qué restricciones? (Altura, peso, horarios)

    > _Respuesta:_

---

### D. Capas de Información

18. ¿Qué capas geográficas serían más útiles para superponer en un mapa? (Zonificación, infraestructura, demografía, competidores, propiedades disponibles)

    > _Respuesta:_

19. ¿Qué necesita ver el equipo en un mapa durante una presentación a cliente?

    > _Respuesta:_

20. ¿Qué capas deberían poder activarse/desactivarse según el contexto?

    > _Respuesta:_

21. ¿El equipo necesita ver información de tráfico? ¿En tiempo real o histórico?

    > _Respuesta:_

22. ¿Qué información de infraestructura es crítica? (Líneas de alta tensión, gasoductos, vías de tren)

    > _Respuesta:_

23. ¿El equipo necesita ver zonas de riesgo? (Inundación, sismicidad, contaminación)

    > _Respuesta:_

---

### E. Visualización y Presentación

24. ¿Qué tan importante es la visualización 3D (terreno, alturas de edificios) vs. vista 2D del mapa?

    > _Respuesta:_

25. ¿El equipo se beneficiaría de comparar múltiples propiedades en un solo mapa?

    > _Respuesta:_

26. ¿Qué estilo de mapa prefiere el equipo? (Satélite, mapa de calles, híbrido, minimalista)

    > _Respuesta:_

27. ¿El equipo necesita exportar mapas para presentaciones? ¿En qué formato?

    > _Respuesta:_

28. ¿Los mapas deben poder imprimirse? ¿En qué tamaño/resolución?

    > _Respuesta:_

29. ¿El equipo necesita crear mapas personalizados con anotaciones, flechas, o marcadores?

    > _Respuesta:_

---

### F. Búsqueda Geográfica

30. ¿Cómo busca el equipo propiedades por ubicación hoy?

    > _Respuesta:_

31. ¿El equipo necesita buscar "propiedades dentro de X km de un punto"?

    > _Respuesta:_

32. ¿El equipo necesita buscar "propiedades a lo largo de una ruta o corredor"?

    > _Respuesta:_

33. ¿Qué tan precisa debe ser la ubicación de las propiedades? (Dirección exacta, colonia, zona)

    > _Respuesta:_

34. ¿Cómo maneja el equipo propiedades con ubicación imprecisa o confidencial?

    > _Respuesta:_

---

### G. Datos Geográficos

35. ¿Qué datos geográficos del INEGI serían útiles? (Límites AGEB, infraestructura, demografía)

    > _Respuesta:_

36. ¿El equipo maneja archivos KML o Shapefiles actualmente? ¿De dónde vienen? ¿Cómo deberían cargarse al sistema?

    > _Respuesta:_

37. ¿Hay datos geográficos propios de Beiqa que deberían integrarse? (Mapas de corredores, zonas de cobertura, etc.)

    > _Respuesta:_

38. ¿El equipo necesita importar/exportar datos geográficos? ¿En qué formatos?

    > _Respuesta:_

39. ¿Qué tan actualizados deben estar los datos geográficos? (Mapas base, límites, infraestructura)

    > _Respuesta:_

---

### H. Análisis Geoespacial Avanzado

40. ¿El equipo necesita análisis de cobertura de mercado? (Ej: "¿qué porcentaje del mercado está a menos de 30 min de esta ubicación?")

    > _Respuesta:_

41. ¿El equipo necesita identificar "huecos" en el mercado? (Zonas sin oferta de cierto tipo)

    > _Respuesta:_

42. ¿El equipo necesita análisis de densidad? (Concentración de propiedades, empresas, población)

    > _Respuesta:_

43. ¿El equipo necesita comparar ubicaciones entre sí? (Ej: "¿cuál de estas 3 propiedades tiene mejor acceso a X?")

    > _Respuesta:_

44. ¿Qué análisis geoespacial hacen los competidores que Beiqa no puede hacer hoy?

    > _Respuesta:_

---

### I. Integración con Otros Módulos

45. ¿Cómo debería integrarse el mapa con la búsqueda de propiedades?

    > _Respuesta:_

46. ¿El mapa debería mostrar datos de inteligencia de mercado? (Precios por zona, vacancia, etc.)

    > _Respuesta:_

47. ¿El mapa debería integrarse con el portal de clientes?

    > _Respuesta:_

48. ¿La IA debería poder usar el mapa para responder preguntas? (Ej: "muéstrame propiedades cerca del aeropuerto")

    > _Respuesta:_

---

### J. Experiencia de Usuario

49. ¿Qué tan técnico es el equipo con herramientas de mapas? ¿Necesitan algo muy simple o pueden manejar complejidad?

    > _Respuesta:_

50. ¿El mapa debería ser el centro de la interfaz o una herramienta complementaria?

    > _Respuesta:_

51. ¿El equipo necesita usar el mapa en dispositivos móviles? ¿En campo?

    > _Respuesta:_

52. ¿Qué frustraciones tiene el equipo con herramientas de mapas actuales?

    > _Respuesta:_

---

### K. Priorización y MVP

53. ¿Qué funcionalidad de mapas es imprescindible para el MVP?

    > _Respuesta:_

54. ¿Qué análisis geoespacial puede esperar a fases posteriores?

    > _Respuesta:_

55. ¿Qué problema geoespacial, si se resolviera, cambiaría más el día a día del equipo?

    > _Respuesta:_

56. ¿Qué es más importante: un mapa bonito o un mapa funcional con datos?

    > _Respuesta:_

---

## Notas Adicionales

> _Notas:_
