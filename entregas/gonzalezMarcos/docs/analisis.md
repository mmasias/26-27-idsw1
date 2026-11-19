# Revisión del ejemplo de Tetris

[Volver al README](../README.md) · [Ver la propuesta](propuesta.md)

Revisión del [modelo del dominio][dominio], los [requisitos][requisitos], sus diagramas y el prototipo.

[Incoherencias](#incoherencias) · [Decisiones discutibles](#decisiones-discutibles)

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

La [tabla del apartado 7][dominio] y el [modelo final][modelo] ponen composición de Tablero a Celda y de Línea a Celda. Las celdas de una línea son las mismas que las del tablero, pero una celda no puede tener dos propietarios por composición al mismo tiempo, según la [definición de composición en UML](https://www.omg.org/spec/UML/2.5.1/PDF).

Dejaría la composición entre Tablero y Celda, y pondría una asociación entre Línea y Celda. Así se indica qué celdas forman cada línea sin darles dos propietarios.

### Faltan datos en el modelo final

El [texto del dominio][dominio] indica multiplicidades y dice que puntuación y velocidad son atributos. El [diagrama final][modelo] no muestra esos datos. Además, la tabla usa `1:*` para las piezas activas, aunque los casos de uso se refieren a una pieza activa cada vez.

Añadiría al dibujo las multiplicidades y los atributos que ya se mencionan. En la relación de pieza activa pondría `0..1` y aclararía que hay una mientras se juega. Así el modelo distingue la pieza que se controla de los bloques ya fijados, y recoge la información del texto sin añadir conceptos nuevos.

### Falta concretar cuánto se mueve la pieza y hacia dónde gira

En [moverPieza()][moverPieza] se indica izquierda o derecha sin decir cuánto se mueve. En [rotarPieza()][rotarPieza] no se pide el sentido, aunque el [prototipo][imagen] ofrece dos comandos de giro.

Añadiría que cada solicitud mueve una celda y que, al girar, el jugador indica el sentido horario o antihorario. También concretaría cuánto gira, por ejemplo 90 grados por solicitud. Mantendría las respuestas que ya aparecen para los movimientos válidos y los bloqueados. Solo falta precisar la entrada para saber qué posición se espera ver.

### Falta explicar qué pasa si la pieza ya no puede bajar

En [descenderPieza()][descenderPieza], primero se presenta la pieza en una nueva posición y después se distingue si está en el fondo, sobre piezas fijadas o en una posición intermedia. No se explica por separado el caso de una pieza que ya está apoyada antes de recibir la solicitud de descenso.

Añadiría esa alternativa: si no puede bajar una celda, se queda donde está y se fija. Después se continúa con la eliminación de líneas y la siguiente pieza. Así se puede seguir el detalle también cuando la pieza aparece ya apoyada o llega a un apoyo tras moverse de lado.

El [ejemplo de una pieza apoyada](propuesta.md#ejemplos-del-descenso) muestra el resultado que esperaría.

### No se explica del todo cómo se borran las líneas ni cómo se suman los puntos

[descenderPieza()][descenderPieza] presenta el tablero sin líneas completas y la puntuación actualizada, pero no dice cómo se recolocan los bloques ni cuántos puntos se ganan.

Explicaría que se eliminan las filas completas tras fijar la pieza y que los bloques de arriba bajan para ocupar los huecos. También añadiría una regla de puntuación para cuando no se borra ninguna fila, se borra una o se borran varias. Sin eso, dos personas podrían interpretar resultados distintos a partir del mismo texto.

En la propuesta se muestra [cómo quedaría el tablero al borrar dos filas](propuesta.md#ejemplos-del-descenso).

### No se indica cómo se elige la siguiente pieza

En [descenderPieza()][descenderPieza] se genera una nueva pieza, y el [modelo del dominio][dominio] recoge los siete tipos. Sin embargo, no se dice cómo se elige el tipo que aparece. Esta observación también está en la [revisión de Silvia Montes](https://github.com/Silviamontes43/26-27-idsw1/blob/781e88abe0a879e90eb80aed588f272427857b5a/entregas/montesSilvia/tetris.md#212-no-se-explica-cómo-se-generan-las-nuevas-piezas).

Para este ejemplo propondría elegir al azar entre los siete tipos, con la misma probabilidad y permitiendo repeticiones. Aplicaría esa regla también a la primera pieza. Así queda claro que pueden salir dos iguales seguidas y que no se sigue un orden fijo, sin añadir más reglas al juego.

### No se indica con qué velocidad empieza la partida

[alternarVelocidadCaida()][alternarVelocidadCaida] explica bien los dos sentidos del cambio: de normal a acelerada y de acelerada a normal. Sin embargo, [iniciarPartida()][iniciarPartida] no indica el modo inicial ni se aclara si la velocidad se conserva cuando aparece otra pieza. La [tabla de actores][requisitos] menciona un intervalo configurado, pero no concreta su valor.

Propondría empezar a velocidad normal y conservar el modo elegido hasta que el jugador vuelva a alternarlo o reinicie. También indicaría cada cuánto baja la pieza en cada modo. Con estas aclaraciones se puede comprobar el comportamiento sin cambiar la idea del caso de uso.

### El reinicio necesita aclarar la cancelación y la espera

[reiniciarPartida()][reiniciarPartida] ya incluye confirmar y cancelar. Al cancelar aparecen salidas hacia `EN_CURSO` y `FINALIZADO`, pero no se indica cuál corresponde al estado desde el que se pidió reiniciar. Tampoco se explica si la partida avanza mientras se espera la respuesta.

Añadiría que cancelar devuelve al estado anterior: una partida en curso continúa y una terminada sigue terminada. Durante la confirmación detendría la caída y los movimientos, para que el jugador no pierda mientras responde.

Mantendría `INACTIVO` como resultado de confirmar, igual que en el ejemplo. Cambiaría la frase «limpia estado de partida» por lo que ve el jugador: la pantalla inicial y la opción de empezar otra partida. Los [diagramas de la propuesta](propuesta.md) muestran estos cambios.

### Una sola captura no muestra todas las interacciones

El [apartado de prototipado][requisitos] dice que una imagen es suficiente. La [captura][imagen] permite ver el tablero, la puntuación y los controles de movimiento y giro, pero no muestra cómo iniciar, reiniciar, confirmar, cancelar o alternar la velocidad.

Mantendría esa captura y añadiría bocetos sencillos de inicio, confirmación y fin de partida, con sus opciones. Para el descenso y el cambio de velocidad bastaría una explicación breve de lo que ocurre entre una imagen y la siguiente. Así se podrían revisar las interacciones que faltan sin rehacer el prototipo.

[dominio]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/docs/modeloDelDominio.md
[requisitos]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/docs/ProcesoRequisitos.md
[modelo]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/docs/modeloDelDominio.md#8-modelo-del-dominio-resultante
[estados]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/docs/ProcesoRequisitos.md#estados-del-sistema
[jugador]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/images/modelosUML/DiagramaDeContexto001-ActorJugador.svg
[imagen]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/images/tetris.png
[descenderPieza]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/docs/ProcesoRequisitos.md#descenderpieza
[iniciarPartida]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/docs/ProcesoRequisitos.md#iniciarpartida
[moverPieza]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/docs/ProcesoRequisitos.md#moverpieza
[reiniciarPartida]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/docs/ProcesoRequisitos.md#reiniciarpartida
[rotarPieza]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/docs/ProcesoRequisitos.md#rotarpieza
[alternarVelocidadCaida]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/docs/ProcesoRequisitos.md#alternarvelocidadcaida
