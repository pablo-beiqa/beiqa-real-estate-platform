# Market Intelligence

**Fase del proyecto**: Fase 2+ — Post-MVP
**Estado**: 🔴 Por iniciar
**Owner**: Por definir

---

## Descripcion

El modulo de Market Intelligence transforma los datos crudos de propiedades y fuentes externas en analisis de mercado accionables para el equipo de BEIQA. Genera reportes automatizados sobre tendencias de precios de renta y venta por zona y tipo de inmueble, tasas de vacancia estimadas, analisis de oferta y demanda, y alertas cuando se detectan movimientos significativos en el mercado. Es el modulo que convierte datos en inteligencia de negocio.

Hoy el equipo de BEIQA elabora estos analisis de forma manual, consultando multiples fuentes y construyendo reportes en Excel que consumen horas de trabajo especializado. Este modulo automatiza la generacion de esos reportes con datos actualizados, permite identificar oportunidades de mercado antes que la competencia, y proporciona al equipo argumentos respaldados por datos para sus clientes corporativos. Los analisis generados alimentan tanto la operacion interna como los reportes que se presentan a clientes.

---

## Objetivos

1. Generar reportes automatizados de tendencias de mercado (precios, vacancia, oferta) por zona metropolitana y corredor industrial, reduciendo el tiempo de elaboracion de reportes de dias a minutos.
2. Automatizar el analisis competitivo de zonas, identificando corredores con mayor dinamismo y oportunidades para clientes corporativos.
3. Proveer alertas proactivas al equipo cuando se detecten cambios significativos en el mercado (nuevos desarrollos, caidas de precio, cambios en vacancia).

---

## Metricas de Exito / KPIs

| Metrica | Target | Como se mide |
|---------|--------|--------------|
| Reportes generados automaticamente | >=10 reportes de mercado por mes | Conteo de reportes generados por el sistema sin intervencion manual |
| Precision vs analisis manual | >=90% de coincidencia con analisis elaborado manualmente por el equipo | Comparacion trimestral de metricas clave (precio promedio, vacancia) entre reporte automatico y manual |
| Tiempo ahorrado en elaboracion de reportes | Reduccion de 80% (de 8h a <2h por reporte) | Medicion de tiempo con equipo de analisis antes/despues |
| Cobertura de zonas analizadas | >=15 zonas metropolitanas / corredores industriales | Conteo de zonas con datos suficientes para generar reporte completo |
| Tasa de uso de alertas | >=60% de alertas resultan en accion del equipo | (Alertas que generan seguimiento) / (total alertas enviadas) |

---

## Entregables Clave

| Entregable | Descripcion | Estado |
|-----------|-------------|--------|
| Motor de tendencias de precio | Calculo automatizado de precio promedio, mediana, y tendencia por zona/tipo/periodo con visualizaciones | 🔴 |
| Tracker de vacancia | Estimacion de tasa de vacancia por zona basada en tiempo de publicacion y rotacion de listados | 🔴 |
| Generador de reportes automatizados | Sistema que produce reportes de mercado en formato PDF/web con graficas, tablas y conclusiones | 🔴 |
| Sistema de alertas de mercado | Notificaciones al equipo cuando se detectan variaciones significativas en precio, volumen o vacancia | 🔴 |
| Comparador de corredores | Herramienta para comparar metricas clave entre zonas/corredores lado a lado | 🔴 |

---

## Dependencias

### Necesita (upstream)
- **Base de datos** → Datos historicos de propiedades con series de tiempo de precios y disponibilidad
- **Scraper** → Flujo continuo de datos actualizados de listados para calcular metricas en tiempo casi-real
- **Data Ingestion** → Datos demograficos y economicos por zona para contextualizar el analisis de mercado
- **Geospatial** → Definicion de zonas, corredores y poligonos para agregar datos geograficamente

### Depende de este (downstream)
- **Internal App** → Muestra reportes, dashboards y alertas al equipo de BEIQA
- **AI Brain** → Consume tendencias de mercado para mejorar la calidad de las recomendaciones
- **Tenant Portal** → Reportes de mercado como valor agregado para clientes corporativos

---

## Riesgos Clave

| # | Riesgo | Impacto | Probabilidad | Mitigacion |
|---|--------|---------|--------------|------------|
| 1 | Calidad insuficiente de datos historicos que produzca tendencias poco confiables | Alto | Media | Establecer umbrales minimos de datos por zona antes de generar reportes, mostrar nivel de confianza |
| 2 | Modelos estadisticos que generen conclusiones incorrectas sobre el mercado | Alto | Media | Validacion trimestral con equipo de analistas, backtesting con datos historicos conocidos |
| 3 | Sesgo en datos de scraping (solo listados publicos, no transacciones reales) | Medio | Alta | Documentar claramente las limitaciones, complementar con datos de fuentes oficiales cuando sea posible |
| 4 | Sobrecarga de alertas que el equipo ignore ("alert fatigue") | Medio | Media | Configurar umbrales inteligentes, permitir personalizacion de alertas por usuario/zona |
| 5 | Complejidad en definir zonas/corredores de forma consistente | Bajo | Media | Usar poligonos del modulo Geospatial como fuente unica de verdad para definicion de zonas |

---

## Documentos del Modulo

- [Product Questions](./Product-Questions.md) — Cuestionario de discovery
- [Requirements](./Requirements.md) — Capacidades y criterios de aceptacion
- [Research/](./Research/) — Investigacion tecnica
