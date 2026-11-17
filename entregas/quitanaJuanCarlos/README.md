## 1. Incoherencias

### 1.1. El cambio de velocidad tiene dos nombres

**Dónde lo he visto:** En la [lista de casos de uso](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md) y en el [detalle de iniciarPartida()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/iniciarPartida.puml).

**Qué dice cada uno:** En la lista aparece `alternarVelocidadCaida()`, pero al explicar cómo se inicia una partida se habla de `acelerarCaida()`. No entiendo que sean exactamente lo mismo, porque alternar permite pasar de una velocidad a otra, mientras que acelerar solo significa ir más rápido.

**Qué cambiaría y por qué:** Usaría `alternarVelocidadCaida()` en los dos sitios. Así queda claro que se trata de la misma acción y que el jugador puede volver a la velocidad normal.

### 1.2. No queda claro si el juego tiene niveles

**Dónde lo he visto:** En las partes de [identificación de conceptos, decisiones de modelado y simplificaciones](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md).

**Qué dice cada una:** Primero se identifica el nivel como la dificultad actual. Más adelante se decide dejarlo como una propiedad del juego. Sin embargo, al hablar de las simplificaciones se dice que no hay niveles de dificultad.

**Qué cambiaría y por qué:** Aclararía si el nivel aumenta mientras jugamos o si se refiere a elegir una dificultad antes de empezar. Si no se va a utilizar ninguno de los dos, quitaría la referencia al nivel. Creo que, tal como está, se puede entender que existe y que no existe a la vez.

### 1.3. Las celdas aparecen como parte de dos elementos distintos

**Dónde lo he visto:** En la [tabla de relaciones](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md) y en el [diagrama final del dominio](https://github.com/puntoReflex/pyTetris/blob/main/images/modelosUML/ModeloDominio.svg).

**Qué dice cada uno:** El tablero está formado por celdas, pero también se representa que cada línea está formada por celdas. En los dos casos se usa una relación de composición, es decir, se indica que las celdas son partes propias de ambos elementos.

**Qué cambiaría y por qué:** Yo dejaría que el tablero estuviera formado por líneas y cada línea por sus celdas. Otra opción sería tratar la línea solo como una agrupación de celdas del tablero, sin darle una segunda composición. Lo cambiaría porque una misma celda no debería tener dos propietarios a la vez en este tipo de relación.

### 1.4. Reiniciar no siempre lleva al mismo estado

**Dónde lo he visto:** En el [diagrama de contexto del jugador](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorJugador.puml) y en el [detalle de reiniciarPartida()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml).

**Qué dice cada uno:** El diagrama de contexto muestra que al reiniciar se vuelve al estado inactivo. En cambio, el detalle dice que antes aparece una confirmación y que el jugador puede cancelar. Si cancela, sigue donde estaba.

**Qué cambiaría y por qué:** Indicaría en el diagrama de contexto que se vuelve a inactivo **solo cuando se confirma** el reinicio. También dejaría claro que, si se cancela, se mantiene el estado anterior. Así ambos diagramas contarían lo mismo.

### 1.5. Algunos resultados de los casos de uso no quedan bien diferenciados

**Dónde lo he visto:** En los detalles de [moverPieza()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/moverPieza.puml), [rotarPieza()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/rotarPieza.puml), [descenderPieza()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/descenderPieza.puml) y [reiniciarPartida()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml).

**Qué dicen:** Dentro de los diagramas se distingue, por ejemplo, entre un movimiento válido y uno bloqueado, o entre confirmar y cancelar un reinicio. Sin embargo, esos caminos terminan en el mismo punto interno y después aparecen varias salidas sin indicar claramente cuál corresponde a cada resultado.

**Qué cambiaría y por qué:** Marcaría las condiciones de cada salida: «movimiento válido», «movimiento bloqueado», «reinicio confirmado», «reinicio cancelado», etc. Creo que así se seguiría mejor el recorrido y no habría dudas sobre qué pasa en cada situación.

### 1.6. El descenso se muestra antes de comprobar si se puede realizar

**Dónde lo he visto:** En el [detalle de descenderPieza()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/descenderPieza.puml) y, para compararlo, en el [detalle de moverPieza()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/moverPieza.puml).

**Qué dice cada uno:** Al descender, primero se indica que se presenta la pieza en la nueva posición y después se comprueba si ha llegado al fondo o ha chocado. En el movimiento lateral sucede al revés: primero se comprueba si la posición es válida.

**Qué cambiaría y por qué:** Yo comprobaría primero si la pieza puede bajar. Si puede, la mostraría una fila más abajo; si no, la dejaría donde está y continuaría con el proceso de fijarla. Me parece más lógico y evita mostrar una posición que podría ser imposible.

### 1.7. El prototipo no explica cómo cambiar la velocidad

**Dónde lo he visto:** En la [lista de casos de uso](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md), en el [detalle del cambio de velocidad](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/alternarVelocidadCaida.puml) y en la [imagen del prototipo](https://github.com/puntoReflex/pyTetris/blob/main/images/tetris.png).

**Qué dice cada uno:** Los requisitos incluyen la opción de alternar la velocidad. Pero la imagen solo enseña las teclas para mover y rotar las piezas. Además, el texto da a entender que esa imagen es suficiente para conocer las interacciones.

**Qué cambiaría y por qué:** Añadiría el control para cambiar la velocidad o explicaría que esta función todavía no está representada en la imagen. No afirmaría que la función no existe en el juego, porque la captura por sí sola no demuestra eso. Lo que falta es mostrar cómo se utiliza.

### 1.8. El prototipo tampoco enseña la confirmación del reinicio

**Dónde lo he visto:** En el [detalle de reiniciarPartida()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml) y en la parte de [prototipado de requisitos](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md).

**Qué dice cada uno:** En el detalle se explica que el jugador tiene que confirmar o cancelar el reinicio. La imagen únicamente enseña una partida en curso, sin ninguna pantalla o mensaje de confirmación. Aun así, el documento presenta esa imagen como suficiente para los casos de uso.

**Qué cambiaría y por qué:** Añadiría una imagen de la confirmación, con sus dos opciones. Creo que es necesaria para entender qué verá el jugador y cómo podrá decidir si reinicia o continúa.

### 1.9. La vuelta desde «Finalizado» no está explicada igual

**Dónde lo he visto:** En el [contexto del jugador](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorJugador.puml) y en el [contexto de tiempo](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorTiempo.puml).

**Qué dice cada uno:** En el primero, para pasar de finalizado a inactivo aparece `reiniciarPartida()`. En el segundo se dibuja el mismo cambio sin indicar qué lo provoca.

**Qué cambiaría y por qué:** Pondría la acción que provoca ese cambio o explicaría que el segundo diagrama la omite porque solo muestra lo relacionado con el tiempo. Si no, cuesta saber si el paso es automático o depende del jugador.

## 2. Decisiones que yo habría tomado de otra forma

### 2.1. La cantidad de piezas activas

**Qué se decidió:** En la [tabla de relaciones del dominio](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md) se permite asociar un juego con varias piezas activas.

**Qué propongo y por qué:** Yo separaría la pieza que está cayendo de las que ya han quedado colocadas. Durante una partida normal solo hay una pieza activa a la vez. Así el modelo reflejaría mejor lo que ocurre en el juego.

### 2.2. Faltan cantidades en las relaciones del diagrama

**Qué se decidió:** La tabla explica cuántos tableros, piezas o celdas se relacionan, pero esas cantidades no aparecen en el [diagrama final](https://github.com/puntoReflex/pyTetris/blob/main/images/modelosUML/ModeloDominio.svg).

**Qué propongo y por qué:** Yo añadiría esas cantidades al dibujo. Me resultaría más fácil entenderlo sin tener que consultar una tabla aparte. No lo considero una contradicción, sino información que falta en el diagrama.

### 2.3. No se indican las dimensiones del tablero en el modelo

**Qué se decidió:** En la [lista inicial de conceptos](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md) se habla de un tablero de 10 × 20, pero el modelo no recoge ese dato.

**Qué propongo y por qué:** Si se quiere representar ese Tetris concreto, yo indicaría que el tablero tiene 10 columnas y 20 filas. Así quedaría definida una de sus reglas básicas, sin tener que decidir todavía cómo programarlo.

### 2.4. El concepto de «turno» no me parece suficientemente claro

**Qué se decidió:** Se incluye `Turno` en el [modelo del dominio](https://github.com/puntoReflex/pyTetris/blob/main/images/modelosUML/ModeloDominio.svg), relacionado con las piezas y los movimientos.

**Qué propongo y por qué:** Yo explicaría que un turno empieza cuando aparece una pieza y termina cuando queda fija, si eso es lo que se quiere representar. Si no se necesita ese concepto, lo quitaría. En Tetris las piezas bajan continuamente y no hay turnos tan claros como en otros juegos.

### 2.5. Los tipos de piezas podrían representarse de forma más sencilla

**Qué se decidió:** Se incluye `TipoPieza` como una clase propia en el [modelo del dominio](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md).

**Qué propongo y por qué:** Yo valoraría usar una lista cerrada con los siete tipos: I, O, T, S, Z, J y L. Si cada tipo tuviera más información o reglas propias, mantendría la clase. Pero si solo se usa para saber qué forma tiene una pieza, la lista me parece suficiente.

### 2.6. El motivo para quitar «Jugador» no me convence

**Qué se decidió:** En el [modelo del dominio](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md) se excluye `Jugador` porque Tetris se juega de forma individual.

**Qué propongo y por qué:** Yo decidiría si hace falta representar información del jugador, como su nombre o sus puntuaciones, aunque solo juegue una persona. Si no hace falta guardar nada de él, entonces sí lo quitaría. Que aparezca como actor en requisitos y no como clase del dominio no me parece un error: ahí cumple funciones diferentes.

### 2.7. No tengo claro que «Tiempo» deba ser un actor

**Qué se decidió:** En los [requisitos](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md) el tiempo aparece como actor y es quien solicita el descenso de las piezas.

**Qué propongo y por qué:** Yo aclararía si hay algún reloj externo que avisa al juego. Si el propio juego controla cuándo bajan las piezas, no pondría el tiempo como un actor separado. Me parece más fácil entenderlo como una regla del funcionamiento del juego.

### 2.8. Los casos de uso parecen nombres de funciones

**Qué se decidió:** Se utilizan nombres como `moverPieza()`, `rotarPieza()` y `reiniciarPartida()` en el [diagrama de casos de uso](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaCasosUso.puml).

**Qué propongo y por qué:** Yo los llamaría «Mover pieza», «Rotar pieza» y «Reiniciar partida». Creo que así se entiende mejor lo que quiere hacer el jugador, sin que parezca que ya estamos definiendo funciones del programa.

### 2.9. Los diagramas de contexto muestran principalmente estados

**Qué se decidió:** En la sección de [diagramas de contexto](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md) aparecen `Inactivo`, `EnCurso` y `Finalizado`, junto con los cambios entre ellos.

**Qué propongo y por qué:** Yo los presentaría como diagramas de estados o como vistas del recorrido de cada actor. Para enseñar el contexto del sistema utilizaría también un dibujo donde se viera qué actores interactúan con el juego y cuál es su límite.

### 2.10. Reiniciar lleva a la pantalla inicial

**Qué se decidió:** El [detalle de reiniciarPartida()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml) termina en `INACTIVO` cuando el jugador confirma.

**Qué propongo y por qué:** Yo distinguiría entre «Reiniciar partida», que empezaría una nueva desde cero, y «Volver al inicio», que nos devolvería a la pantalla inicial. La solución actual puede ser válida, pero creo que el nombre puede llevar a confusión.

### 2.11. Los dos botones de rotación no se explican

**Qué se decidió:** El [prototipo](https://github.com/puntoReflex/pyTetris/blob/main/images/tetris.png) muestra `7=rotarS` y `9=rotarU`, pero el [detalle de rotarPieza()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/rotarPieza.puml) solo habla de rotar una pieza.

**Qué propongo y por qué:** Yo explicaría qué hace cada tecla y si giran en sentidos contrarios. No creo que haga falta crear dos casos de uso distintos, pero sí indicar que existen dos opciones.

### 2.12. No se explica cómo se calculan los puntos

**Qué se decidió:** En el [detalle de descenderPieza()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/descenderPieza.puml) se dice que se actualiza la puntuación al eliminar líneas.

**Qué propongo y por qué:** Yo incluiría una regla sencilla que indique cuántos puntos se consiguen al eliminar una, dos o más líneas. Sin esa información, dos versiones del juego podrían dar puntuaciones diferentes y ambas cumplirían lo que está escrito.

### 2.13. Se mezcla el inicio del juego con el inicio de una partida

**Qué se decidió:** Se define `arrancarSistema()` y también `iniciarPartida()`. Además, el [diagrama general de estados](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto000.puml) muestra una salida final sin explicar cómo se cierra el programa.

**Qué propongo y por qué:** Yo separaría el momento de abrir o cerrar el programa del momento de jugar una partida. Así sería más sencillo entender qué estados pertenecen a la aplicación y cuáles pertenecen a la partida.

### 2.14. Se menciona la posibilidad de pausar, pero después desaparece

**Qué se decidió:** En la [tabla de responsabilidades del dominio](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md) se menciona que el juego puede pausarse, pero no aparece un caso de uso para hacerlo en los [requisitos](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md).

**Qué propongo y por qué:** Yo aclararía si pausar queda fuera de esta versión o si realmente se quiere incluir. No creo que sea obligatorio añadir una pausa, pero sí dejarlo explicado.

### 2.15. Las velocidades de caída quedan poco definidas

**Qué se decidió:** Se habla de una caída automática por intervalos y de dos velocidades en el [documento de requisitos](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md), pero no se concreta cuándo cambia cada una ni qué ocurre después.

**Qué propongo y por qué:** Yo definiría, al menos, qué significa velocidad normal y acelerada, cómo se cambia entre ellas y si al aparecer una nueva pieza se conserva la velocidad elegida. Así se podría comprobar si el comportamiento es el esperado.

## 3. Valoración final

De los problemas que he encontrado, los que me parecen más claros son el cambio de nombre de la función de velocidad, la forma de relacionar las celdas en el modelo y la diferencia entre el reinicio explicado en el contexto y el reinicio detallado. En esos casos puedo señalar dos partes concretas de la documentación que no encajan bien entre sí.

También cambiaría la manera de explicar algunos casos de uso y añadiría información al prototipo. No porque el juego tenga que ser más complejo, sino porque con lo que aparece ahora hay situaciones que yo no sabría interpretar de una única forma.