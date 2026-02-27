---
status: "{propuesto | aceptado | rechazado | deprecado | supersedido por [ADR-NNNN](NNNN-titulo.md)}"
date: {YYYY-MM-DD}
decision-makers: "{lista de quienes toman la decisión}"
consulted: "{expertos consultados — comunicación bidireccional}"
informed: "{stakeholders informados — comunicación unidireccional}"
costo-mensual: "{$XX/mo o $0 o 'incluido en [servicio]'}"
---

# {título corto — describe el problema y la solución}

## Contexto y Problema

{Contexto completo. Frame como pregunta cuando sea posible. Links a ADRs anteriores si aplica. Suficiente background para que alguien (o un agente) entienda sin preguntas adicionales.}

## Factores de Decisión

* {restricción, requisito, o fuerza 1}
* {restricción, requisito, o fuerza 2}
* …

## Opciones Consideradas

* {opción 1}
* {opción 2}
* {opción 3}
* …

## Decisión

Opción elegida: "**{opción 1}**", porque {justificación referenciando factores y tradeoffs}.

### Consecuencias

* Bien, porque {consecuencia positiva}
* Mal, porque {consecuencia negativa aceptada}
* Neutral, porque {consecuencia neutra}
* …

## Plan de Implementación

{Qué debe hacer un agente o desarrollador para ejecutar esta decisión. Ser específico.}

* **Sistemas afectados**: {repos, servicios, DBs que cambian — ej. `beiqa-scraper`, `Supabase`, `HubSpot`}
* **Dependencias**: {paquetes, servicios, APIs a agregar/remover}
* **Patrones a seguir**: {referencia a código o docs existentes}
* **Patrones a evitar**: {qué NO hacer}
* **Configuración**: {env vars, configs, feature flags}
* **Migración**: {si reemplaza algo, pasos incrementales}

### Verificación

- [ ] {criterio verificable 1}
- [ ] {criterio verificable 2}
- [ ] …

<!-- Opcional — remover si no aplica -->
## Pros y Contras por Opción

### {opción 1}

* Bien, porque {argumento}
* Mal, porque {argumento}

### {opción 2}

* Bien, porque {argumento}
* Mal, porque {argumento}

<!-- Opcional — remover si no aplica -->
## Información Adicional

{ADRs relacionados, links, condiciones que dispararían revisar esta decisión.}
