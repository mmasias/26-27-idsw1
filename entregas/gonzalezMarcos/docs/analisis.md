# Revisión del ejemplo de Tetris

[Volver al README](../README.md) · [Ver la propuesta](propuesta.md)

Revisión del [modelo del dominio][dominio], los [requisitos][requisitos], sus diagramas y el prototipo.

## Incoherencias

### Acelerar y alternar no son lo mismo

El [catálogo de casos de uso][requisitos] incluye `alternarVelocidadCaida()`. Su [detalle][alternarVelocidadCaida] permite pasar de velocidad normal a acelerada y volver a normal. Sin embargo, al terminar [iniciarPartida()][iniciarPartida], el sistema ofrece `acelerarCaida()`, que no aparece en el catálogo.

Cambiaría ese nombre por `alternarVelocidadCaida()` en el detalle de iniciar y en su imagen. Si la pieza ya cae rápido, alternar hace que vuelva a caer a velocidad normal; acelerar no expresa eso. Además, el catálogo, el contexto del jugador y el detalle de alternar ya usan el mismo nombre.

### Falta una transición en el diagrama general

En el [diagrama general][estados] no aparece el paso de `EnCurso` a `Inactivo`. En el [contexto del jugador][jugador] sí aparece con `reiniciarPartida()`. El [detalle de reiniciar][reiniciarPartida] también permite llegar a `INACTIVO` cuando se confirma el reinicio durante una partida.

Añadiría esa transición al diagrama general. Si pretende mostrar los estados del sistema, debería recoger este cambio, que ya está en los otros dos diagramas. No quitaría la posibilidad de reiniciar mientras se juega, porque ambos coinciden en permitirla.

### No queda claro si hay nivel o no

En el [apartado 6 del modelo del dominio][dominio] se decide que Puntuación y Nivel son propiedades de Juego. Más abajo, la tabla de «Simplificaciones conscientes» dice que no hay niveles de dificultad.

Dejaría el ejemplo sin niveles y corregiría el apartado 6 para indicar que Nivel se ha descartado. El documento cuenta cómo se fue construyendo el modelo, pero no señala que la primera decisión haya cambiado. Quitar Nivel como entidad no lo elimina del modelo si todavía se mantiene como propiedad. Además, los casos de uso no necesitan niveles: cambiar entre dos velocidades no implica tener niveles de dificultad.

## Decisiones discutibles

### Las celdas pertenecen por composición a dos elementos

La [tabla del apartado 7][dominio] y el [modelo final][modelo] ponen composición de Tablero a Celda y de Línea a Celda. Pero las celdas de una línea son las mismas que las del tablero, y una celda no debería tener dos propietarios por composición al mismo tiempo. Esta es la regla que recoge la [referencia de UML sobre composición](https://www.omg.org/spec/UML/2.5.1/PDF).

Dejaría la composición entre Tablero y Celda, y pondría una asociación entre Línea y Celda. Así se puede indicar qué celdas forman cada línea sin darles dos propietarios. Aquí la tabla y el dibujo coinciden; lo que cambiaría es la relación elegida.

### Faltan datos en el modelo final

El [texto del dominio][dominio] indica multiplicidades y dice que puntuación y velocidad son atributos. Sin embargo, el [diagrama final][modelo] no muestra esos datos.

Añadiría las multiplicidades que ya se han decidido, la puntuación y el modo de caída. Por ejemplo, debería verse que una partida tiene un tablero. Así el dibujo se entendería sin tener que buscar esa información por toda la página.

### No se explica bien qué es una pieza activa ni cuánto dura un turno

La [tabla de relaciones][dominio] usa `1:*` para las piezas activas. El [modelo][modelo] también relaciona Turno con Pieza y Movimiento, pero no explica cuándo empieza y termina un turno.

Indicaría que, mientras se juega, hay una sola pieza activa, y que puede no haber ninguna antes de empezar o al terminar. También definiría el turno desde que aparece una pieza hasta que queda fijada. Así se distingue la pieza que se está controlando de los bloques que ya están colocados, y no se confunde cada movimiento con un turno nuevo. La relación actual permite varias piezas activas, aunque no obliga a que las haya.

### La elección de Juego frente a Partida no me parece bien justificada

En el [apartado 3 del dominio][dominio] se elige Juego porque se considera que Partida es más propio del multijugador. Sin embargo, también hay partidas de un solo jugador.

Usaría Partida para una sesión de juego y Sistema Tetris para la aplicación. También se podría mantener Juego si se explica que significa Partida. Lo importante es distinguir entre abrir el juego y empezar una partida, y usar las palabras con el mismo significado en todo el documento.

### Se descarta Bloque sin explicar qué queda al borrar una línea

En el [apartado 3 y la tabla de descartados][dominio] se elimina Bloque porque se considera parte de Pieza y se dice que no existe independientemente. El problema es que, al borrar una línea, pueden desaparecer algunos bloques de una pieza y quedar otros.

Distinguiría la celda, que es un espacio del tablero, del bloque que la ocupa. Si se quiere evitar añadir Bloque al modelo, al menos explicaría que, cuando una pieza queda fijada, sus partes pueden permanecer o desaparecer por separado. Esto permite entender qué pasa cuando una pieza no se borra entera.

### Línea se usa con dos sentidos

En la [lista de conceptos][dominio], Línea se define como una fila horizontal completa. Después se usa como una agrupación de celdas del tablero.

Llamaría Fila al conjunto de celdas que están a la misma altura, y fila completa a la que tiene todas sus celdas ocupadas por bloques fijados. Una fila existe aunque esté vacía. Separar las dos ideas ayudaría a entender qué representa el diagrama y cuándo hay que eliminar una fila.

### Algunas razones para descartar conceptos no me convencen

En el [apartado 4 y los conceptos descartados][dominio] se usan los verbos para decidir qué es una entidad. Columna se descarta por no tener comportamiento propio y Jugador por tratarse de un juego de un solo jugador.

Creo que habría que decidirlo según la información que necesita el ejemplo. Columna podría quedar como una coordenada y Jugador fuera del modelo si no se van a guardar datos de la persona. Mantendría esas exclusiones, pero cambiaría la explicación: un concepto puede ser importante aunque no tenga acciones propias o solo haya uno. Además, Jugador puede seguir siendo actor aunque no aparezca como entidad del dominio.

### Falta concretar las formas y los movimientos

El [dominio][dominio] menciona siete tipos de piezas, pero no explica sus formas. En [moverPieza()][moverPieza] se indica izquierda o derecha sin decir cuánto se mueve. En [rotarPieza()][rotarPieza] no se pide el sentido del giro, aunque el [prototipo][imagen] ofrece dos comandos distintos.

Describiría las siete piezas, dónde aparece cada una y alrededor de qué punto gira. También indicaría que cada solicitud mueve una celda o gira 90 grados en el sentido elegido. Sin esas reglas, no se puede comprobar qué posición debería mostrar el sistema ni si una rotación es válida.

### Siguiente pieza y sombra se descartan solo por ser detalles de interfaz

En los [conceptos descartados][dominio], Preview/Siguiente y Sombra/Ghost se consideran únicamente detalles de interfaz. Yo los dejaría fuera para simplificar el ejercicio, pero no por esa razón.

Saber cuál es la siguiente pieza o dónde caería la actual también tiene un significado dentro del juego. La puntuación se muestra en pantalla y sí se incluye en el modelo. Por eso explicaría que estas funciones no se van a estudiar en este ejemplo, en vez de descartarlas solo por cómo se muestran.

### Se menciona pausar, pero después no se vuelve a explicar

El [apartado 4 del dominio][dominio] incluye pausar entre las acciones de Juego, pero el [catálogo][requisitos] no tiene un caso de uso para ello.

Indicaría que la pausa se ha descartado o se deja para otra versión. Si se quiere incluir, habría que explicar sus estados y qué ocurre con el tiempo. Dejar escrita esa decisión permitiría distinguir un descarte de un olvido.

### Se dice que el modelo está validado, pero faltan ejemplos de esa revisión

En los [apartados 1 y 9 del dominio][dominio] se mencionan reglas oficiales y validación con expertos. La tabla dice que el modelo cubre la mecánica completa, pero no indica qué reglas se consultaron ni qué situaciones se comprobaron.

Añadiría la versión del juego que se toma como referencia y algunos ejemplos revisados, como borrar varias filas o no poder colocar la siguiente pieza. Así se podría comprobar en qué se basa la conclusión de que el modelo está completo.

### Terminar una partida no es lo mismo que cerrar el juego

El [diagrama general][estados] permite pasar de Finalizado al estado final y también volver a Inactivo, sin explicar qué provoca cada salida.

Dejaría `FINALIZADO` como el estado desde el que se puede pedir reinicio. Quitaría la salida al estado final, salvo que se quiera explicar también cómo se cierra la aplicación. Perder una partida no significa que se cierre el juego, y el diagrama debería dejar clara esa diferencia.

### No siempre se sabe qué salida corresponde a cada resultado

En [reiniciar][reiniciarPartida] y [descender][descenderPieza], varias alternativas llegan al mismo final interno y después aparecen salidas hacia estados diferentes. Hay colores y notas, pero faltan condiciones en las salidas. Por ejemplo, al cancelar el reinicio aparecen dos destinos sin indicar de cuál se venía.

Escribiría la condición en cada salida. En descender, solo se llegaría a `FINALIZADO` si no cabe la nueva pieza. En reiniciar, cancelar desde `EN_CURSO` devolvería a `EN_CURSO`, y cancelar desde `FINALIZADO` devolvería a `FINALIZADO`. En el [contexto del jugador][jugador] también aclararía que solo se llega a `INACTIVO` si se confirma el reinicio. Así no hay que deducirlo por el color, ni queda la duda de si cancelar puede reactivar una partida perdida.

### Se muestra el descenso antes de comprobar si la pieza puede bajar

En [descenderPieza()][descenderPieza], primero se presenta la pieza en una nueva posición y después se comprueba si está en el fondo o sobre piezas fijadas.

Comprobaría antes si puede bajar una celda. Si no puede, la fijaría donde está. Si puede, mostraría la nueva posición y explicaría cuándo queda fijada al tocar un apoyo. Esto cubre el caso de una pieza que aparece ya apoyada o llega a un apoyo tras moverse de lado. El detalle no debería dejar la duda de si antes se muestra una posición que choca con otra pieza o sale del tablero.

### No se explica del todo cómo se borran las líneas ni cómo se suman los puntos

[descenderPieza()][descenderPieza] presenta el tablero sin líneas completas y la puntuación actualizada, pero no dice cómo se recolocan los bloques ni cuántos puntos se ganan.

Explicaría que se eliminan todas las filas completas tras fijar la pieza y que los bloques de arriba bajan para ocupar los huecos. También añadiría una regla de puntuación para cuando no se borra ninguna fila, se borra una o se borran varias. Sin eso, dos personas podrían interpretar resultados distintos a partir del mismo texto.

### Faltan datos sobre las velocidades y el tiempo de respuesta

[alternarVelocidadCaida()][alternarVelocidadCaida] distingue velocidad normal y acelerada, pero no dice cada cuánto baja la pieza ni con qué modo se empieza. Los [detalles][requisitos] tampoco indican cuánto puede tardar el sistema en mostrar la respuesta a una solicitud.

Definiría los dos intervalos, empezaría en normal y aclararía si la velocidad se mantiene cuando aparece otra pieza. Al alternar, cambiaría solo la velocidad, sin hacer que la pieza baje además una posición. También acordaría un tiempo máximo de respuesta. Decir que cae más rápido no basta para comprobarlo, y un movimiento que tarda varios segundos en verse puede hacer que sea difícil jugar.

### Reiniciar deja dudas sobre la confirmación y lo que ocurre después

[reiniciarPartida()][reiniciarPartida] lleva a `INACTIVO` y dice que el sistema limpia el estado. No explica si la partida sigue avanzando mientras el jugador responde a la confirmación.

Mantendría ese destino, pero diría que se vuelve a la pantalla inicial, con puntuación cero y opción de iniciar. Mientras se espera la respuesta, detendría la partida; si se cancela, se volvería a ella sin cambios. Así no se puede perder mientras se está confirmando. También aclararía que reiniciar no empieza otra partida directamente, porque aquí hay que solicitar iniciar después.

### Game Over no explica cómo continuar

La última alternativa de [descenderPieza()][descenderPieza] presenta Game Over. El [contexto del jugador][jugador] permite reiniciar desde Finalizado, pero el detalle no ofrece esa opción.

Mostraría el tablero y la puntuación final, y permitiría pedir reinicio. También aclararía que ya no se puede mover ni girar la pieza. Saber que se ha llegado a `FINALIZADO` no explica por sí solo qué ve el jugador ni qué puede hacer a continuación.

### Una sola captura no muestra todos los casos de uso

El [apartado de prototipado][requisitos] dice que una imagen es suficiente. Sin embargo, la [captura][imagen] solo muestra el tablero, la puntuación y los controles 4, 6, 7 y 9. No se ve cómo iniciar, reiniciar, confirmar, cancelar o alternar la velocidad.

Añadiría bocetos sencillos de la pantalla inicial, la confirmación y el fin de partida. Explicaría los controles que faltan y qué ocurre si se introduce un comando que no existe. Para el descenso y la velocidad, pondría una secuencia breve de lo que va pasando, incluyendo la vuelta de velocidad acelerada a normal. Una captura quieta no permite comprobar movimiento ni velocidad. Las teclas concretas las dejaría en el prototipo, no en los detalles de los casos de uso.

[dominio]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/docs/modeloDelDominio.md
[requisitos]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/docs/ProcesoRequisitos.md
[modelo]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/modelosUML/ModeloDominio.puml
[estados]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/modelosUML/DiagramaDeContexto000.puml
[jugador]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/modelosUML/DiagramaDeContexto001-ActorJugador.puml
[imagen]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/images/tetris.png
[descenderPieza]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/modelosUML/detalleCasosUso/descenderPieza.puml
[iniciarPartida]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/modelosUML/detalleCasosUso/iniciarPartida.puml
[moverPieza]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/modelosUML/detalleCasosUso/moverPieza.puml
[reiniciarPartida]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/modelosUML/detalleCasosUso/reiniciarPartida.puml
[rotarPieza]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/modelosUML/detalleCasosUso/rotarPieza.puml
[alternarVelocidadCaida]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/modelosUML/detalleCasosUso/alternarVelocidadCaida.puml
