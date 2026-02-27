---
status: "{propuesto | aceptado | rechazado | deprecado | supersedido por [ADR-NNNN](NNNN-titulo.md)}"
date: {YYYY-MM-DD}
decision-makers: "{lista de quienes toman la decisión}"
costo-mensual: "{$XX/mo o $0}"
---

# {título corto}

## Contexto y Problema

{Por qué esta decisión necesita tomarse. Incluir suficiente background para entender sin preguntas.}

## Decisión

{Qué elegimos hacer. Scope y non-goals.}

## Consecuencias

* Bien, porque {consecuencia positiva}
* Mal, porque {consecuencia negativa}
* …

## Plan de Implementación

* **Sistemas afectados**: {repos, servicios, DBs}
* **Dependencias**: {paquetes, APIs}
* **Patrones a seguir / evitar**: {referencia}

### Verificación

- [ ] {cómo confirmar que se implementó correctamente}

<!-- Opcional — remover si no aplica -->
## Alternativas Consideradas

* {Alternativa 1}: {por qué se rechazó}
* {Alternativa 2}: {por qué se rechazó}

<!-- Opcional — remover si no aplica -->
## Información Adicional

{ADRs relacionados, docs, condiciones para revisitar.}
