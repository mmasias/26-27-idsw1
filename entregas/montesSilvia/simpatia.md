## Escenario: definir si alguien es simpático

Se propone modelar la simpatía como una valoración contextual de una persona, obtenida a partir de observaciones/evaluaciones de sus interacciones y de varios criterios de comportamiento.

Diagrama de dominio

El diagrama UML incluido en documento.uml representa las entidades principales y sus relaciones.



Glosario

Persona: individuo cuya simpatía se evalúa.

Contexto: situación o entorno en el que se valora la simpatía.

Interacción: encuentro o comportamiento observable entre personas dentro de un contexto.

Evaluación de simpatía: valoración de una persona según uno o varios criterios.

Criterio de simpatía: aspecto usado para valorar la simpatía, por ejemplo, amabilidad o respeto.

Simpatía: resultado agregado de las evaluaciones para una persona y un contexto.

Supuestos

La simpatía no es absoluta: una persona puede resultar simpática en un contexto y no necesariamente en otro.

La simpatía se obtiene a partir de conductas observables o evaluaciones, no de una característica física o fija.

Los criterios y sus pesos son configurables; por defecto pueden considerarse aspectos como amabilidad, escucha, respeto, humor y consideración.

Se considera que alguien es simpático cuando el nivel de simpatía calculado alcanza un umbral previamente acordado.

La valoración puede proceder de varias evaluaciones para evitar que una única interacción determine por sí sola el resultado.

Decisiones de modelado discutibles

Simpatía como concepto derivado y contextual: se evita poner esSimpatico como un atributo permanente de Persona, porque convertiría una valoración subjetiva y dependiente de la situación en una propiedad absoluta.

Separación entre Interaccion y EvaluacionSimpatia: una interacción es un hecho del dominio; una evaluación es una interpretación de ese hecho. Mantenerlos separados permite registrar diferentes valoraciones sobre una misma interacción.

Criterios explícitos: se modelan como conceptos del dominio porque permiten explicar de dónde procede la valoración de simpatía y modificar los criterios sin cambiar el concepto de persona.

Umbral para “simpático”: se usa una regla de negocio sencilla: si el resultado agregado alcanza el umbral definido, esSimpatico es verdadero. El valor exacto del umbral queda fuera del modelo conceptual porque puede variar según el contexto o los requisitos.

## Regla conceptual

De forma simplificada:

Una persona es simpática en un contexto cuando las evaluaciones relevantes de sus interacciones, ponderadas según los criterios establecidos, producen un nivel de simpatía igual o superior al umbral definido.