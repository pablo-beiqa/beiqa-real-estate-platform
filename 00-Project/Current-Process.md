# Proceso Actual de Trabajo

## Descripción del Proceso Sin Plataforma/Producto

Este documento describe cómo trabaja actualmente el equipo de Beiqa sin la plataforma, para entender qué automatizar y mejorar.

## Journey del Cliente

```
┌─────────────────────────────────────────────────────────────────┐
│ 1. PRIMER CONTACTO                                               │
│    • Cliente contacta (inbound) o Beiqa prospecta (outbound)     │
│    • Se registra en HubSpot como lead                            │
│    • Llamada/reunión inicial para entender necesidades           │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│ 2. CAPTURA DE REQUERIMIENTO                                      │
│    • Criterios: ubicación, tamaño (m²), presupuesto              │
│    • Tipo de inmueble, características específicas               │
│    • Timeline de ocupación                                       │
│    • Se documenta en... ¿dónde? (HubSpot? Excel? Notas?)         │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│ 3. BÚSQUEDA DE PROPIEDADES                            ⏱️ LENTO   │
│    • Búsqueda manual en Inmuebles24, Vivanuncios, etc.          │
│    • Consulta a red de contactos y brokers                       │
│    • Cada propiedad se revisa individualmente                    │
│    • Se guardan opciones en... ¿Excel? ¿Documento?               │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│ 4. ANÁLISIS Y SHORTLIST                                          │
│    • Se evalúa cada propiedad vs criterios                       │
│    • Se crea shortlist de mejores opciones                       │
│    • Análisis de ubicación... ¿manual con Google Maps?           │
│    • Comparativo de precios... ¿manual?                          │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│ 5. PRESENTACIÓN AL CLIENTE                                       │
│    • Se prepara material (¿PowerPoint? ¿PDF?)                    │
│    • Reunión de presentación de opciones                         │
│    • Cliente da feedback                                         │
│    • Posiblemente nueva ronda de búsqueda                        │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│ 6. SELECCIÓN Y NEGOCIACIÓN                                       │
│    • Cliente selecciona favoritas                                │
│    • Visitas a propiedades                                       │
│    • Negociación de términos                                     │
│    • Múltiples iteraciones posibles                              │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│ 7. CIERRE                                                        │
│    • Firma de contrato                                           │
│    • Comisión para Beiqa                                         │
│    • Se actualiza HubSpot                                        │
└─────────────────────────────────────────────────────────────────┘
```

## Herramientas Actuales


| Herramienta  | Uso Principal                | Problemas                    |
| ------------ | ---------------------------- | ---------------------------- |
| HubSpot      | CRM, pipeline de deals       | No tiene info de propiedades |
| Excel/Sheets | Tracking de propiedades      | Disperso, no centralizado    |
| Inmuebles24  | Búsqueda de propiedades      | Manual, no se guarda         |
| Vivanuncios  | Búsqueda de propiedades      | Manual, no se guarda         |
| Google Maps  | Ver ubicación de propiedades | Sin análisis avanzado        |
| Email        | Comunicación con clientes    | Sin centralizar              |
| WhatsApp     | Comunicación rápida          | Sin tracking                 |
| Teléfono     | Llamadas                     | Sin registro sistemático     |


## Preguntas a Validar con el Equipo

- ¿Dónde se documentan exactamente los requerimientos del cliente?
- ¿Cómo se guarda el shortlist de propiedades actualmente?
- ¿Qué formato tienen las presentaciones para clientes?
- ¿Cómo se comunican las actualizaciones al cliente?
- ¿Cuánto tiempo promedio toma cada etapa?
- ¿Cuáles son los principales cuellos de botella?

## Oportunidades de Mejora


| Etapa        | Pain Point       | Cómo BEIQA Ayuda          |
| ------------ | ---------------- | ------------------------- |
| Búsqueda     | Manual, lenta    | Scraper automatizado      |
| Análisis     | Sin herramientas | Market Intel + Geospatial |
| Presentación | Manual           | Reportes automáticos      |
| Seguimiento  | Disperso         | Portal + tracking         |
| Comunicación | Sin centralizar  | AI Brain + logs           |


---

## Próximos Pasos

- Entrevistas con usuarios para validar proceso
- Shadowing de un proyecto real
- Medir tiempos actuales para comparar después

