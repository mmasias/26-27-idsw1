# Una sombra

## Glosario

- **Fuente de luz**: elemento que emite luz.
- **Objeto**: cuerpo que bloquea total o parcialmente la luz.
- **Superficie**: lugar sobre el que puede proyectarse una sombra.
- **Sombra**: región producida por la ocultación de la luz por un objeto.
- **Escena**: contexto que reúne los elementos implicados.

## Supuestos

- Se modela una sombra proyectada sobre una superficie.
- Una sombra depende de una fuente de luz, un objeto y una superficie.
- Un mismo objeto puede producir sombras distintas según la escena.
- No se modelan magnitudes físicas como intensidad, distancia o ángulo.

## Decisiones de modelado discutibles

- **`Sombra` se modela como concepto propio** porque es relevante en el dominio y depende de varios elementos, no solo del objeto.
- **`Escena` se modela como contexto común** para agrupar los elementos implicados sin entrar en detalles geométricos.
- **La sombra no se modela como atributo de `Objeto`** porque no es una propiedad fija del objeto.
