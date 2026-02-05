# Estimación de Presupuesto Total

## Objetivo

Consolidar todas las estimaciones de costos para la plataforma BEIQA.

**Estado**: 🔴 Por completar

---

## 1. Costos de APIs y Servicios (Mensual)


| Servicio                    | Uso Estimado | Precio         | Costo/Mes      | Notas             |
| --------------------------- | ------------ | -------------- | -------------- | ----------------- |
| **Google Maps Platform**    |              |                |                |                   |
| - Maps JavaScript API       | ??? cargas   | $7/1000        | $???           |                   |
| - Geocoding API             | ??? requests | $5/1000        | $???           |                   |
| - Places API                | ??? requests | $17/1000       | $???           |                   |
| - Routes API                | ??? requests | $5/1000        | $???           |                   |
| - Crédito gratuito          |              |                | -$200          |                   |
| **Subtotal Google**         |              |                | **$???**       |                   |
|                             |              |                |                |                   |
| **INEGI / Gobierno**        | Ilimitado    | Gratis         | $0             |                   |
|                             |              |                |                |                   |
| **OpenAI / Claude**         | ??? tokens   | $???/1K tokens | $???           | Para AI Brain     |
|                             |              |                |                |                   |
| **Proveedores comerciales** | Suscripción  | ???            | $???           | Si decidimos usar |
|                             |              |                |                |                   |
| **TOTAL APIs**              |              |                | **$??? / mes** |                   |


---

## 2. Costos de Infraestructura (Mensual)

### Escenario MVP (Mínimo)


| Recurso        | Especificación             | Provider | Costo/Mes      |
| -------------- | -------------------------- | -------- | -------------- |
| Servidor app   | 2 vCPU, 4GB RAM            | ???      | $???           |
| Base de datos  | PostgreSQL managed, ??? GB | ???      | $???           |
| Object storage | ??? GB                     | ???      | $???           |
| CDN            | Básico                     | ???      | $???           |
| Dominio        | .com                       | ???      | $???           |
| SSL            | Let's Encrypt              | Gratis   | $0             |
| **TOTAL MVP**  |                            |          | **$??? / mes** |


### Escenario Producción


| Recurso              | Especificación | Provider | Costo/Mes      |
| -------------------- | -------------- | -------- | -------------- |
| Servidor(es) app     | ???            | ???      | $???           |
| Base de datos        | ???            | ???      | $???           |
| Object storage       | ???            | ???      | $???           |
| CDN                  | ???            | ???      | $???           |
| Backups              | ???            | ???      | $???           |
| Monitoring           | ???            | ???      | $???           |
| **TOTAL Producción** |                |          | **$??? / mes** |


---

## 3. Costos de Desarrollo (One-time)

### Si desarrollo interno


| Concepto               | Horas Estimadas | Costo/Hora | Total    |
| ---------------------- | --------------- | ---------- | -------- |
| Fase 0 - Investigación | ???             | $???       | $???     |
| Fase 1 - MVP           | ???             | $???       | $???     |
| Fase 2 - Datos + Mapas | ???             | $???       | $???     |
| Fase 3 - AI            | ???             | $???       | $???     |
| Fase 4 - Portal        | ???             | $???       | $???     |
| **TOTAL Desarrollo**   | ??? horas       |            | **$???** |


### Si outsourcing


| Concepto          | Estimación | Notas       |
| ----------------- | ---------- | ----------- |
| Desarrollo MVP    | $???       | Por cotizar |
| Fases adicionales | $???       | Por cotizar |


---

## 4. Costos Recurrentes (Mensual)


| Concepto          | Costo/Mes |
| ----------------- | --------- |
| Infraestructura   | $???      |
| APIs              | $???      |
| Mantenimiento     | $???      |
| **TOTAL MENSUAL** | **$???**  |


---

## 5. Resumen de Inversión

### Inversión Inicial (Year 1)


| Concepto              | Monto    |
| --------------------- | -------- |
| Desarrollo            | $???     |
| Setup infraestructura | $???     |
| **TOTAL INICIAL**     | **$???** |


### Costos Operativos (Mensual)


| Concepto        | MVP      | Producción |
| --------------- | -------- | ---------- |
| Infraestructura | $???     | $???       |
| APIs            | $???     | $???       |
| Mantenimiento   | $???     | $???       |
| **TOTAL/MES**   | **$???** | **$???**   |


### Costo Anual Operativo


| Escenario  | Cálculo   | Total      |
| ---------- | --------- | ---------- |
| MVP        | $??? x 12 | $??? / año |
| Producción | $??? x 12 | $??? / año |


---

## 6. Supuestos y Variables

### Variables que afectan costos


| Variable           | Impacto en Costo                 | Cómo Mitigar                   |
| ------------------ | -------------------------------- | ------------------------------ |
| Volumen de datos   | Mayor storage, más procesamiento | Optimizar queries, cachear     |
| Uso de Google Maps | Pay per use                      | Evaluar alternativas gratuitas |
| Features de AI     | Tokens de LLM                    | Optimizar prompts, cachear     |
| Número de usuarios | Más cómputo                      | Escalar gradualmente           |


### Supuestos

- Precios de AWS/GCP/Azure pueden variar
- Uso de APIs es estimación, necesita validación
- Costos de desarrollo dependen del equipo

---

## 7. Recomendaciones de Ahorro


| Área      | Recomendación                                 | Ahorro Estimado |
| --------- | --------------------------------------------- | --------------- |
| Mapas     | Usar OSM + Leaflet para funciones básicas     | $???/mes        |
| Hosting   | Empezar con tier pequeño, escalar después     | $???/mes        |
| AI        | Usar modelo más económico para tareas simples | $???/mes        |
| Geocoding | Cachear resultados, no repetir                | $???/mes        |


---

## 8. Próximos Pasos

1. [ ] Completar estimación de volúmenes de uso
2. [ ] Obtener cotizaciones de providers
3. [ ] Calcular costos específicos por escenario
4. [ ] Presentar a stakeholders para aprobación
5. [ ] Definir presupuesto máximo

---

## Notas

- Todos los costos son estimaciones preliminares
- Se requiere validación con uso real
- Considerar buffer de 20-30% para imprevistos

