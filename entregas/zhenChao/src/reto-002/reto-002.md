# Revisión de pyTetris

<br>

## INCOHERENCIAS


### 1. La velocidad de caída tiene dos nombres

| | |
|---|---|
| **Dónde** | [`docs/ProcesoRequisitos.md`](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md), [`modelosUML/DiagramaCasosUso.puml`](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaCasosUso.puml) y [`modelosUML/detalleCasosUso/iniciarPartida.puml`](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/iniciarPartida.puml).[`images/modelosUML/detalleCasosUso/acelerarCaida.svg`](https://github.com/puntoReflex/pyTetris/blob/main/images/modelosUML/detalleCasosUso/acelerarCaida.svg). |
| **Problema** | En los requisitos y en el diagrama general se llama `alternarVelocidadCaida()`, pero en el detalle de `iniciarPartida()` aparece `acelerarCaida()`.  |
| **Qué cambiaría** | Utilizaría `alternarVelocidadCaida()` en todos los documentos y actualizaría o eliminaría el SVG antiguo. |
| **Por qué** | La misma funcionalidad debería tener el mismo nombre y una única fuente UML que se pueda mantener. |

### 2. Falta una transición de reinicio en el diagrama general

| | |
|---|---|
| **Dónde** | [`modelosUML/DiagramaDeContexto000.puml`](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto000.puml), [`modelosUML/DiagramaDeContexto001-ActorJugador.puml`](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorJugador.puml) y [`modelosUML/detalleCasosUso/reiniciarPartida.puml`](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml). |
| **Problema** | El diagrama del jugador permite pasar de `EnCurso` a `Inactivo` mediante `reiniciarPartida()`, pero esa transición no aparece en el diagrama general. |
| **Qué cambiaría** | Añadiría `EnCurso --> Inactivo : reiniciarPartida()` al diagrama general. |
| **Por qué** | El diagrama general debería incluir las transiciones que aparecen en los diagramas más detallados. |

### 3. El estado Finalizado tiene una salida sin explicar

| | |
|---|---|
| **Dónde** | [`modelosUML/DiagramaDeContexto000.puml`](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto000.puml), [`modelosUML/DiagramaDeContexto001-ActorJugador.puml`](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorJugador.puml) y [`modelosUML/DiagramaDeContexto001-ActorTiempo.puml`](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorTiempo.puml). |
| **Problema** | El diagrama general muestra `Finalizado -> [*]`, que representa la salida del sistema. Sin embargo, los diagramas de los actores no explican qué acción cierra el juego. |
| **Qué cambiaría** | Eliminaría esa salida si no existe una función para cerrar el juego o añadiría y documentaría ese comportamiento. |
| **Por qué** | Las transiciones del sistema deberían tener una acción o condición que explique cuándo se producen. |

### 4. Los nombres de los estados cambian entre diagramas

| | |
|---|---|
| **Dónde** | [`modelosUML/DiagramaDeContexto000.puml`](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto000.puml) y los diagramas de [`modelosUML/detalleCasosUso/`](https://github.com/puntoReflex/pyTetris/tree/main/modelosUML/detalleCasosUso/). |
| **Problema** | En los diagramas de contexto se usan `Inactivo`, `EnCurso` y `Finalizado`, mientras que en los detalles aparecen `INACTIVO`, `EN_CURSO` y `FINALIZADO`. |
| **Qué cambiaría** | Elegiría una única forma de escribir los estados y la utilizaría en todos los diagramas. |
| **Por qué** | Así se evita confusión y queda claro que se está hablando de los mismos estados. |

### 5. El descenso siempre pasa por eliminar líneas

| | |
|---|---|
| **Dónde** | [`modelosUML/detalleCasosUso/descenderPieza.puml`](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/descenderPieza.puml). |
| **Problema** | Cuando la pieza se fija, el flujo siempre pasa por `EliminandoLineas` y actualiza la puntuación. No se contempla el caso en el que la pieza se fija sin completar ninguna fila. |
| **Qué cambiaría** | Añadiría una decisión para comprobar si hay filas completas. Solo se eliminarían y se sumarían puntos cuando corresponda. |
| **Por qué** | Una pieza puede quedarse fija sin completar ninguna fila, por lo que el diagrama debería contemplar las dos posibilidades. |

### 6. El diagrama del dominio no refleja todo lo explicado en el documento

| | |
|---|---|
| **Dónde** | [`docs/modeloDelDominio.md`](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md) y [`modelosUML/ModeloDominio.puml`](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/ModeloDominio.puml). |
| **Problema** | El documento explica relaciones y multiplicidades, pero el diagrama no muestra las multiplicidades. Además, el UML incluye relaciones como `Pieza --> Celda` y `Turno o-- Movimiento` que no aparecen en la tabla de relaciones del documento. |
| **Qué cambiaría** | Revisaría el documento y el diagrama para que incluyan las mismas relaciones y multiplicidades. |
| **Por qué** | Los dos artefactos deberían representar el mismo modelo del dominio. |

<br>

##  DECISIONE DISCUTIBLES


### 1. Incluir Turno en un juego que funciona continuamente

| | |
|---|---|
| **Dónde** | [`docs/modeloDelDominio.md`](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md) y [`modelosUML/ModeloDominio.puml`](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/ModeloDominio.puml). |
| **Qué se decidió** | Se incluye `Turno` como concepto relacionado con la pieza y los movimientos. |
| **Qué propondría** | Quitaría `Turno` y describiría el juego mediante la pieza activa y los movimientos del jugador. |
| **Por qué** | En Tetris no se alternan turnos entre jugadores. La pieza baja continuamente mientras el jugador la controla, así que el concepto de turno no parece representar bien cómo funciona el juego. |



### 2. Alternar la velocidad en vez de acelerar mientras se pulsa una tecla

| | |
|---|---|
| **Dónde** | [`modelosUML/detalleCasosUso/alternarVelocidadCaida.puml`](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/alternarVelocidadCaida.puml). |
| **Qué se decidió** | La velocidad funciona como un interruptor: se cambia de normal a rápida y después se vuelve a normal. |
| **Qué propondría** | Haría que la pieza acelerase mientras el jugador mantiene pulsada una tecla y que recuperase su velocidad normal al soltarla. |
| **Por qué** | Con el sistema actual el jugador puede olvidarse de desactivar la velocidad rápida y perder el control de la siguiente pieza. La aceleración mientras se mantiene la tecla resulta más directa. |

### 3. Pedir confirmación incluso cuando la partida ya ha terminado

| | |
|---|---|
| **Dónde** | [`modelosUML/detalleCasosUso/reiniciarPartida.puml`](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml). |
| **Qué se decidió** | El sistema pide confirmación para reiniciar tanto desde `EN_CURSO` como desde `FINALIZADO`. |
| **Qué propondría** | Pediría confirmación solo si la partida sigue en curso. Si ya ha terminado, reiniciaría directamente. |
| **Por qué** | La confirmación sirve para evitar perder una partida activa por error. Después de un Game Over ya no hay progreso que se pueda perder, por lo que ese paso añade una acción innecesaria. |




