---
status: "propuesto"
date: 2026-02-27
decision-makers: "Pablo (CEO), Fabrizio (Tech Lead)"
costo-mensual: "por definir"
---

# Algoritmo de Deduplicación — Propiedades Cross-Portal

## Contexto y Problema

Con la estrategia multi-portal ([ADR-012](ADR-012-Multi-Portal-Data.md)), las mismas propiedades aparecen en múltiples portales (Inmuebles24, Pincali, CBRE, Colliers). Al consolidar datos en la tabla `properties` (golden record), se necesita un algoritmo que identifique duplicados y mergee la mejor información de cada fuente.

El problema es complejo porque:
- Las propiedades no tienen un ID universal entre portales
- Direcciones pueden estar escritas diferente ("Av. Insurgentes" vs "Avenida Insurgentes Sur")
- Coordenadas pueden tener variaciones de precisión
- Campos como precio o superficie pueden diferir entre portales

## Factores de Decisión

* 60K+ propiedades ya en la base de datos, creciendo con cada portal nuevo
* No existe un identificador común entre portales
* Campos clave para matching: dirección, coordenadas (lat/lng), superficie, tipo
* Precisión importa más que velocidad (batch nocturno, no real-time)
* El equipo tiene acceso a Claude API para LLM-assisted matching

## Opciones a Evaluar

### Scoring multi-campo determinístico
* Asignar puntaje por cada campo que coincida (dirección fuzzy match, coordenadas dentro de radio, superficie ±10%)
* Pros: predecible, auditable, rápido
* Contras: muchos edge cases, thresholds arbitrarios

### LLM-assisted matching
* Usar Claude para evaluar si dos propiedades son la misma
* Pros: maneja variaciones de texto, contexto semántico
* Contras: costo por llamada, más lento, no determinístico

### Híbrido (scoring + LLM para ambiguos)
* Scoring determinístico primero, LLM solo para casos con score medio (zona gris)
* Pros: costo-eficiente, alta precisión
* Contras: más complejo de implementar

### Librerías especializadas (splink, dedupe)
* splink (Python, Bayes probabilístico) o dedupe (Python, active learning)
* Pros: probadas en producción, modelos probabilísticos robustos
* Contras: Python (el stack es TypeScript), setup inicial significativo

## Decisión

**Pendiente.** Se definirá en paralelo con la integración de nuevos portales (cuando haya datos reales de múltiples fuentes para testear). La combinación híbrida (scoring + LLM para ambiguos) es la favorita pero falta validar con datos reales.

## Información Adicional

- Complementa: [ADR-012](ADR-012-Multi-Portal-Data.md) (Multi-Portal Data Strategy)
- Ejecución probable en: [ADR-003](ADR-003-Trigger-dev.md) (Trigger.dev batch job)
- Geocodificación ayuda al matching: [ADR-011](ADR-011-Google-Maps-Platform.md)
- H3 puede agrupar candidatos por hexágono: [ADR-009](ADR-009-H3-Indexing.md)
- Condición para activar: cuando haya ≥2 portales con datos en staging tables
