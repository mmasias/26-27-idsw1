
# Reto 002 - Revisión de `pyTetris`

## Incoherencias

### 1. Falta de relación entre `Pieza` y `Tablero`
- En el modelo del dominio aparecen `Pieza` y `Tablero`, pero no queda del todo clara la relación entre ambos. En los requisitos se menciona que las piezas se mueven, giran y descienden dentro del tablero.
- Propongo aclarar esta relación, indicando que el tablero contiene las piezas colocadas y que existe una pieza activa que el jugador puede controlar.

### 2. La puntuación no está bien representada
- En los requisitos se menciona la puntuación, pero en el modelo del dominio no queda claro cómo se relaciona con las acciones que realiza el jugador.
- Propongo añadir `puntuación` como atributo de `Partida`, indicando que aumenta cuando se completan líneas.

### 3. Falta de claridad al finalizar la partida
- En los requisitos se explica cómo iniciar y reiniciar una partida, pero no queda suficientemente claro qué sucede cuando el jugador ya no puede colocar más piezas.
- Propongo añadir el estado `Partida finalizada` al modelo de estados y explicar qué condición provoca que termine el juego.

---

## Decisiones discutibles

### 1. Utilizar `Tiempo` como actor
- En los requisitos se utiliza `Tiempo` como actor para representar la caída automática de las piezas. Sin embargo, el tiempo no es un usuario que interactúe directamente con el juego.
- Propongo representar la caída automática como un comportamiento interno del sistema, ya que forma parte del funcionamiento del juego y no de una acción realizada por un usuario.

### 2. Separar demasiado los movimientos de las piezas
- Los requisitos incluyen acciones como `desplazarPieza`, `rotarPieza` y `descenderPieza`. Aunque son acciones diferentes, todas están relacionadas con el control de la pieza actual.
- Propongo agrupar estas acciones dentro de un caso de uso general llamado `Controlar pieza`, manteniendo los movimientos específicos como acciones asociadas. Así el modelo quedaría más organizado.

### 3. Mejorar la descripción de `reiniciarPartida`
- En el caso de uso `reiniciarPartida` se describen acciones internas del sistema, pero no se explica claramente qué observa el jugador al reiniciar.
- Propongo describir el resultado desde el punto de vista del jugador, indicando que aparece el tablero vacío, la puntuación vuelve a cero y se genera una nueva pieza para comenzar.

---

## Conclusión

Considero que el modelo de `pyTetris` representa las funciones principales del juego, pero hay algunos aspectos que podrían mejorarse para que los requisitos y el modelo del dominio sean más coherentes.

Mis propuestas se centran en aclarar las relaciones entre las entidades, mejorar la representación de la puntuación y la finalización de la partida, y simplificar algunos casos de uso.

Con estos cambios, el modelo sería más fácil de comprender y estaría mejor organizado.
