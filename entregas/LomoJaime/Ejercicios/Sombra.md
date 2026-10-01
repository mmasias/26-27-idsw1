# Una sombra

## Glosario

* Luz: Lo que ilumina.
* Obstáculo: Lo que se pone en medio y tapa la luz.
* Superficie: Donde cae la sombra.
* Sombra: La mancha que aparece en la superficie cuando el obstáculo tapa la luz.
* Zona: Cada parte de la sombra según lo oscura que sea.

## Suposiciones

* El obstáculo tapa la luz y no la deja pasar (no es transparente).
* La sombra necesita las tres cosas a la vez: luz, obstáculo y superficie. Si falta una, no hay sombra.
* Una sombra viene de una luz concreta. Con dos luces, el mismo obstáculo da dos sombras.
* La forma de la sombra cambia si se mueve cualquiera de las tres cosas.

## Decisiones de modelado

* La sombra es el centro del modelo porque es lo único que depende de las otras tres. Ninguna de ellas "contiene" a la sombra.
* Umbra y penumbra no son clases propias sino zonas dentro de la sombra, ya que solo cambia lo oscura que es esa parte. Así el modelo es más corto y no se repite.
* La sombra no es una característica del obstáculo porque cambia según la luz y la superficie, no solo según el obstáculo.
* Cada sombra tiene al menos una zona oscura. Puede tener más zonas más claras o no tenerlas.