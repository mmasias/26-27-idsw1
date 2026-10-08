# El concepto de simpatía

## Glosario

- **Persona**: individuo que interactúa, valora y puede ser valorado.
- **Interacción**: relación o intercambio entre dos o más personas.
- **Contexto**: situación en la que ocurre una interacción o se formula una valoración.
- **Criterio de simpatía**: aspecto que una persona tiene en cuenta al valorar a otra.
- **Valoración de simpatía**: juicio de una persona sobre otra respecto a su simpatía.

## Supuestos

- La simpatía es subjetiva.
- Una persona puede ser considerada simpática por alguien y no por otra persona.
- La valoración puede variar según el contexto.
- Una valoración se apoya en una o varias interacciones.
- No existe una fórmula universal para decidir si alguien es simpático.

## Decisiones de modelado discutibles

- **No se incluye un atributo `simpatico` en `Persona`** porque convertiría una valoración subjetiva y contextual en una propiedad absoluta.
- **`ValoracionDeSimpatia` se modela como clase conceptual** porque relaciona evaluador, evaluado, interacción, contexto y criterios.
- **`CriterioDeSimpatia` se modela por separado** porque un mismo criterio puede utilizarse en distintas valoraciones.
- **Evaluador y evaluado son roles de `Persona`** porque cualquier persona puede desempeñar ambos papeles.
