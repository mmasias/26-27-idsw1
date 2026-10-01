# Farmear aura

## Glosario

- **Persona**: individuo que realiza acciones o las observa.
- **Acción**: comportamiento realizado por una persona.
- **Contexto**: situación en la que ocurre una acción.
- **Valoración de aura**: juicio de un observador sobre el efecto de una acción en el aura percibida de alguien.
- **Episodio de farmeo**: conjunto de acciones asociadas a la obtención o aumento de aura percibida.

## Supuestos

- El aura se entiende como una valoración social percibida, no como una propiedad física.
- El efecto de una acción sobre el aura depende del observador y del contexto.
- Un episodio de farmeo puede contener una o varias acciones.
- No toda acción tiene por qué realizarse conscientemente para obtener aura.
- No se supone una escala universal de puntos de aura.

## Decisiones de modelado discutibles

- **El aura no se modela como atributo numérico de `Persona`** porque es subjetiva y puede variar entre observadores.
- **`ValoracionDeAura` se modela como clase conceptual** porque relaciona observador, persona valorada, acción y contexto.
- **`EpisodioDeFarmeo` agrupa acciones** para representar “farmear aura” como un proceso y no necesariamente como una única acción.
- **Observador y persona valorada son roles de `Persona`** porque cualquier persona puede desempeñar ambos papeles.
