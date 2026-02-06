# Preguntas faltantes sugeridas

Este documento lista **preguntas adicionales** que se sugieren integrar en los cuestionarios existentes (00–07) para que la investigación cubra todo lo que salió en la entrevista y en las 5 preguntas centrales de investigación.

**Uso**: Revisar cada bloque; copiar las preguntas que apliquen al archivo y sección indicados. Cuando una pregunta ya esté cubierta por otra redacción en el mismo cuestionario, no hace falta duplicar.

---

## 00-Vision-General.md

**Dónde**: Después de "Decisiones Estratégicas" o en una nueva subsección "Investigación y go/no-go".

- ¿Qué resultado concreto haría que digas "esta fase de investigación valió la pena"? (Ej.: plan técnico validado, scraper funcionando, MVP en uso, claridad total de costos y factibilidad.)
- Cuando una pregunta de investigación se "responda", ¿qué formato necesitas? (Solo recomendación para decidir con tu socio, especificación técnica para el equipo de desarrollo, o ambos.)
- ¿Quién ejecutará la investigación? (Tú con ayuda de IA, el equipo técnico cuando exista, tú lo estratégico y ellos lo técnico.)
- ¿Qué condición haría que detengas el proyecto o lo replantees por completo? (Go/no-go: presupuesto, plazo, viabilidad legal, otra.)

---

## 01-Scraper-y-Adquisicion-de-Datos.md

**Dónde**: Nueva subsección "Método de captura para el MVP" o después de "Métodos de captura".

- Para el MVP, ¿qué método de captura es aceptable? (Solo automático 24/7, semi-manual con extensión o herramienta, manual con importación de Excel/CSV, o que la investigación evalúe todas las opciones.)
- Si un portal (por ejemplo EasyBroker) ofrece API oficial además de la web: ¿prefieres que prioricemos integración vía API o que la investigación compare API vs scraping y recomiende?
- Cuando la **misma propiedad** aparece varias veces en un portal (distintos agentes), ¿cómo debería comportarse el sistema desde la perspectiva del usuario? (Mostrar una sola ficha "fusionada", mostrar todas con una marca de "posible duplicado", otro.)

---

## 02-Ingestion-de-Datos.md

**Dónde**: En "Priorización" o al final.

- Para el MVP, si solo pudieras tener **una** fuente de datos externa además de los portales, ¿cuál sería la primera que quieres que se investigue e integre?

---

## 03-Base-de-Datos.md

**Dónde**: En "Información de Propiedades" o nueva subsección "Duplicados y conflictos".

- Cuando el sistema detecte que dos o más listings son la **misma propiedad** (por dirección, coordenadas o similitud), ¿qué debe ver el usuario? (Una sola propiedad "maestra" con nota de fuentes, o varias filas con indicador de "posible duplicado".)
- Si la misma propiedad viene con **datos contradictorios** (por ejemplo distinto precio o superficie según la fuente), ¿cómo debe mostrarse? (Un valor "principal" con opción de ver alternativas, el más reciente, el que venga de la fuente que prefieras, otro.)

---

## 04-Inteligencia-de-Mercado.md

**Dónde**: En "Reportes actuales" o "Confianza y calidad".

- ¿Cuál es el **mínimo** que debe tener un "reporte de mercado" para ser útil? (Un número clave, un gráfico, una tabla de comparables, un PDF de varias páginas.)

---

## 05-Geoespacial-y-GIS.md

**Dónde**: En "Visualización".

- Para el MVP, ¿la visualización 2D del mapa es suficiente o consideras imprescindible tener algo en 3D (terreno, alturas) desde el inicio?

---

## 06-Cerebro-IA.md

**Dónde**: En "Prioridad" o "Integración con Otros Módulos".

- ¿La **deduplicación** de propiedades (reconocer que varios listings son el mismo inmueble) debe apoyarse en IA desde el MVP, o puede ser reglas simples al inicio y IA después?
- ¿Qué decisiones **nunca** debería tomar la IA sin que un humano las revise o apruebe? (Ej.: fusionar dos propiedades como duplicados, enviar algo al cliente, otra.)

---

## 07-Portal-Tenants.md

**Dónde**: En "Prioridad".

- ¿Cuál es el **mínimo** que el portal de clientes debe permitir para que consideres que "hay portal" y no solo entrega por PDF/Excel? (Solo ver shortlist, ver y comparar, dar feedback, descargar documento, otro.)

---

## Resumen por tema transversal

| Tema | Cuestionarios donde añadir preguntas |
|------|--------------------------------------|
| Go/no-go y valor de la investigación | 00-Vision-General |
| Formato y ownership de la investigación | 00-Vision-General |
| Método de captura (automático vs manual vs híbrido) | 01-Scraper |
| API vs scraping cuando exista API | 01-Scraper |
| Comportamiento ante duplicados (usuario) | 01-Scraper, 03-Base de Datos |
| Datos contradictorios (misma propiedad) | 03-Base de Datos |
| Mínimo de reporte de mercado | 04-Inteligencia |
| 2D vs 3D para MVP | 05-Geoespacial |
| IA en deduplicación y límites de autonomía | 06-Cerebro-IA |
| Mínimo para "portal" de clientes | 07-Portal-Tenants |

Cuando integres estas preguntas en los archivos correspondientes, puedes marcar este documento como "Integrado" o eliminar las secciones ya incorporadas.
