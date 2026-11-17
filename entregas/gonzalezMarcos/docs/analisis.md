# Análisis del ejemplo de Tetris

[Volver al README](../README.md) · [Ver la propuesta](propuesta.md)

He revisado el [modelo del dominio][dominio] y el [proceso de requisitos][requisitos], incluyendo sus textos, tablas, diagramas y prototipo. Los enlaces corresponden al commit `92c950ad9746dc768be130f75c988c0f712d5509`, consultado el 17/11/2026. Así se puede comprobar qué versión estoy comentando.

He separado **3 incoherencias** y **20 decisiones discutibles**. Una falta de información no siempre es una contradicción. Por eso, los detalles que quedan sin explicar están en el segundo grupo.

## Incoherencias

### I01. Acelerar y alternar no son lo mismo

- **Dónde y qué dice cada uno:** el [catálogo de casos de uso][requisitos] incluye `alternarVelocidadCaida()`. Su [detalle][alternarVelocidadCaida] permite pasar de normal a acelerada y también volver a normal. Sin embargo, al terminar [iniciarPartida()][iniciarPartida], el sistema ofrece `acelerarCaida()`, que no aparece en el catálogo.
- **Qué corregiría:** cambiaría `acelerarCaida()` por `alternarVelocidadCaida()` en el detalle de iniciar y en su imagen.
- **Por qué:** si la caída ya está acelerada, alternar la devuelve a normal. Acelerar no expresa ese resultado. Mantendría el nombre que ya comparten el catálogo, el contexto del jugador y el detalle de alternar.

### I02. Falta una transición en el diagrama general

- **Dónde y qué dice cada uno:** en el [diagrama general][estados] no aparece el paso de `EnCurso` a `Inactivo`. En el [contexto del jugador][jugador] sí aparece mediante `reiniciarPartida()`. El [detalle de reiniciar][reiniciarPartida] también permite ese resultado cuando se confirma.
- **Qué corregiría:** añadiría al diagrama general la transición de `EN_CURSO` a `INACTIVO` por reinicio confirmado.
- **Por qué:** la vista general debe recoger los cambios de estado que aparecen en las vistas más detalladas. Aquí no se indica que sea una vista parcial. No eliminaría el reinicio durante la partida, porque los otros dos diagramas coinciden en permitirlo.

### I03. No queda claro si hay nivel o no

- **Dónde y qué dice cada uno:** en [Modelo del dominio, apartado 6][dominio], se decide que Puntuación y Nivel son propiedades de Juego. Más abajo, la tabla de «Simplificaciones conscientes» dice que no hay niveles de dificultad.
- **Qué corregiría:** mantendría la simplificación sin niveles y quitaría Nivel de la decisión del apartado 6, indicando que se ha descartado.
- **Por qué:** no basta con quitarlo como entidad si todavía se conserva como propiedad. Aunque el documento cuenta la evolución del modelo, no señala que esta decisión haya quedado sustituida. Ningún caso de uso necesita niveles; alternar dos velocidades tampoco implica tener niveles de dificultad.

## Decisiones discutibles

### D01. Las celdas pertenecen por composición a dos elementos

- **Decisión:** tanto la [tabla del apartado 7][dominio] como el [modelo final][modelo] ponen composición de Tablero a Celda y de Línea a Celda.
- **Propuesta:** dejar la composición entre Tablero y Celda, y usar una asociación entre Línea y Celda.
- **Argumento:** las celdas de una línea son las mismas que las del tablero. No deberían tener dos propietarios por composición al mismo tiempo. La tabla y el dibujo coinciden, por eso lo considero una decisión de modelado mal elegida y no una diferencia entre artefactos. Esta restricción aparece en [UML 2.5.1, apartado 9.5.3, página 112](https://www.omg.org/spec/UML/2.5.1/PDF).

### D02. El modelo final pierde información que ya se había decidido

- **Decisión:** el [texto del dominio][dominio] indica multiplicidades y dice que puntuación y velocidad son atributos. El [diagrama final][modelo] no muestra ni multiplicidades ni atributos.
- **Propuesta:** añadir los datos que ya se han acordado: un tablero por partida, las multiplicidades de celdas y líneas, puntuación y modo de caída.
- **Argumento:** si solo miro el resultado final, no puedo recuperar esas decisiones. No digo que una multiplicidad sin escribir signifique necesariamente uno; el problema es que falta información útil para entender el modelo.

### D03. No se precisa cuántas piezas están activas ni qué representa un turno

- **Decisión:** la [tabla de relaciones][dominio] usa `1:*` para las piezas activas. El [modelo][modelo] relaciona Turno con Pieza y Movimiento, pero no explica cuándo empieza y termina un turno.
- **Propuesta:** indicar que hay una pieza activa mientras la partida está en curso y que puede no haber ninguna antes de empezar o al terminar. Definir el turno desde la aparición de una pieza hasta que queda fijada.
- **Argumento:** se distingue así la pieza que se controla de las ocupaciones ya fijadas. También se evita interpretar que cada movimiento es un turno o que pueden controlarse varias piezas a la vez. La relación actual no obliga a tener varias, pero tampoco expresa el límite.

### D04. La elección de Juego frente a Partida no está bien justificada

- **Decisión:** en el [apartado 3 del dominio][dominio] se elige Juego porque se considera Partida más propio del multijugador.
- **Propuesta:** usar Partida para una sesión concreta y Sistema Tetris para la aplicación. También valdría mantener Juego si se aclara que significa Partida.
- **Argumento:** también hay partidas de un solo jugador. Lo que interesa distinguir es arrancar el sistema de empezar una partida. Un pequeño glosario ayudaría a mantener los mismos nombres, incluidos `EnCurso` y `EN_CURSO`.

### D05. Se elimina Bloque sin explicar qué pasa al borrar parte de una pieza

- **Decisión:** en el [apartado 3 y la tabla de descartados][dominio] se elimina Bloque porque se considera parte de Pieza y se dice que no existe independientemente.
- **Propuesta:** distinguir una celda, que es un espacio del tablero, de un bloque, que lo ocupa. Si no se quiere añadir Bloque al modelo, explicar que al fijarse la pieza quedan ocupaciones independientes.
- **Argumento:** al borrar una línea pueden desaparecer unos bloques de una pieza y quedar otros. El modelo debe permitir explicar ese caso, aunque se mantenga sencillo.

### D06. Línea mezcla la fila con estar completa

- **Decisión:** en la [lista de conceptos][dominio], Línea es una fila horizontal completa. Después se utiliza como agrupación de celdas del tablero.
- **Propuesta:** definir Fila como las celdas de una misma altura, y fila completa como aquella cuyas celdas están todas ocupadas por bloques fijados.
- **Argumento:** una fila existe aunque esté vacía. Usar la misma palabra para la estructura y para una condición deja dudas sobre qué se elimina y qué representa el diagrama.

### D07. Algunas razones para incluir o excluir conceptos son poco claras

- **Decisión:** en el [apartado 4 y los descartados][dominio] se usan los verbos para decidir qué es una entidad y se descarta Columna por no tener comportamiento. Jugador se excluye por ser monojugador.
- **Propuesta:** decidir según la información y las relaciones que necesita el ejemplo. Columna puede quedar como coordenada y Jugador fuera del modelo si no se necesitan sus datos.
- **Argumento:** un concepto puede ser importante aunque no tenga acciones propias o solo haya uno. Mantendría esas exclusiones, pero cambiaría sus razones. Que Jugador no esté en el dominio no impide que sea actor en los casos de uso.

### D08. Falta concretar las formas y los movimientos

- **Decisión:** el [dominio][dominio] menciona siete tipos, pero no los desarrolla. [moverPieza()][moverPieza] indica izquierda o derecha sin distancia. [rotarPieza()][rotarPieza] no recoge el sentido, aunque el [prototipo][imagen] ofrece dos comandos de giro.
- **Propuesta:** definir los siete tipos y sus posiciones de aparición, un desplazamiento de una celda por solicitud y giros de 90 grados en el sentido pedido. Aclarar el punto de giro de cada tipo.
- **Argumento:** sin esas reglas no se puede saber qué posición debería mostrar el sistema ni cuándo un giro es válido. No hace falta añadir reglas avanzadas, pero sí explicar las básicas. El detalle no niega el giro en ambos sentidos: simplemente no lo concreta.

### D09. Siguiente pieza y sombra se descartan solo por ser visibles en la interfaz

- **Decisión:** en los [conceptos descartados][dominio], Preview/Siguiente y Sombra/Ghost se consideran solo detalles de interfaz.
- **Propuesta:** dejarlos fuera por simplificar el alcance del ejercicio, separando la información de su representación visual.
- **Argumento:** saber cuál es la siguiente pieza o dónde aterrizaría la actual tiene un significado, aunque se muestre en pantalla. La puntuación también se muestra y sí forma parte del modelo. No propongo añadir estas funciones, sino mejorar la justificación para excluirlas.

### D10. Pausar se menciona, pero no se aclara si entra en el ejercicio

- **Decisión:** el [apartado 4 del dominio][dominio] asocia pausar a Juego, pero el [catálogo][requisitos] no incluye pausa.
- **Propuesta:** indicar que se descarta o se deja para otra versión. Si se incluye, habría que añadir sus estados y explicar qué pasa con el tiempo.
- **Argumento:** una idea inicial no tiene que convertirse en requisito. Lo que falta es dejar clara la decisión para que no parezca un olvido. Por eso no lo cuento como incoherencia.

### D11. La validación se afirma sin explicar cómo se comprobó

- **Decisión:** en los [apartados 1 y 9 del dominio][dominio] se mencionan reglas oficiales y validación con expertos. La tabla concluye que el modelo cubre la mecánica completa, pero no identifica las reglas ni los ejemplos comprobados.
- **Propuesta:** indicar qué versión del juego se toma como referencia y anotar algunos casos revisados, como borrar varias filas o no poder colocar la siguiente pieza.
- **Argumento:** así se puede comprobar en qué se basa la conclusión. No afirmo que no se validara; digo que el documento no permite saber qué se revisó.

### D12. Se puede confundir terminar una partida con cerrar el sistema

- **Decisión:** el [diagrama general][estados] permite pasar de Finalizado al estado final y también volver a Inactivo, sin explicar qué provoca cada salida. El [diagrama de casos de uso][casos] tampoco dibuja el límite del sistema.
- **Propuesta:** identificar el sistema y dejar `FINALIZADO` como estado desde el que se puede pedir reinicio. Quitar la salida terminal sin explicar, o definirla si cerrar la aplicación entra en el alcance.
- **Argumento:** perder una partida no significa necesariamente cerrar el juego. No hace falta inventar un caso de salida si no se quiere estudiar; basta con aclarar qué representan los estados.

### D13. Las salidas de varios detalles quedan poco claras

- **Decisión:** en [reiniciar][reiniciarPartida], [descender][descenderPieza], [mover][moverPieza] y [rotar][rotarPieza], distintas alternativas llegan al mismo final interno y después salen hacia resultados diferentes. Se usan colores y notas, pero no siempre condiciones claras. Al cancelar el reinicio aparecen dos destinos sin indicar de cuál se venía.
- **Propuesta:** escribir la condición de cada salida. En reiniciar, cancelar desde `EN_CURSO` vuelve a `EN_CURSO`, y cancelar desde `FINALIZADO` vuelve a `FINALIZADO`. En el [contexto del jugador][jugador], indicar también que solo el reinicio confirmado lleva a `INACTIVO`.
- **Argumento:** no debería depender del color saber qué resultado corresponde. Cancelar tampoco debe reactivar una partida terminada. El contexto podría resumir solo los casos que se completan, pero tendría que decirlo.

### D14. Se muestra el descenso antes de comprobar si se puede bajar

- **Decisión:** en [descenderPieza()][descenderPieza], primero se presenta una nueva posición y después se comprueba si está en el fondo o sobre piezas fijadas.
- **Propuesta:** comprobar antes si la pieza puede bajar una celda. Si no puede, fijarla donde está. Si puede, mostrar la nueva posición y dejar claro cuándo se fija al quedar apoyada.
- **Argumento:** una pieza puede aparecer ya apoyada o llegar a un apoyo tras moverse lateralmente. El detalle debe cubrirlo sin mostrar antes una posición que choque con otra pieza o salga del tablero. Esto no demuestra un fallo del programa; es una duda que deja el detalle.

### D15. Borrar líneas y actualizar puntos no basta para definir el resultado

- **Decisión:** [descenderPieza()][descenderPieza] presenta el tablero sin líneas completas y la puntuación actualizada, pero no explica cómo se recolocan las ocupaciones ni cuántos puntos se ganan.
- **Propuesta:** indicar que se eliminan todas las filas completas tras la fijación y que las ocupaciones superiores bajan los huecos correspondientes. Añadir una regla de puntos para cero, una o varias filas.
- **Argumento:** sin esas reglas podrían obtenerse tableros y puntuaciones diferentes siguiendo el mismo texto. Una regla sencilla podría ser 100 puntos por fila, si se acuerda para este ejercicio; no la doy por regla oficial de Tetris.

### D16. Las velocidades no están definidas del todo

- **Decisión:** [alternarVelocidadCaida()][alternarVelocidadCaida] distingue normal y acelerada. La [tabla de actores][requisitos] habla de un intervalo configurado, pero no se concreta. [iniciar][iniciarPartida] y [reiniciar][reiniciarPartida] tampoco indican el modo inicial.
- **Propuesta:** fijar los dos intervalos, empezar en normal y aclarar si el modo se mantiene al aparecer otra pieza. Explicar si alternar solo cambia el intervalo o provoca además un descenso.
- **Argumento:** caer más rápido es una descripción insuficiente para comprobar el comportamiento. También hay que saber cuándo se vuelve a normal. Mantendría el descenso como respuesta al actor Tiempo y la alternancia como cambio de velocidad.

### D17. Reiniciar no explica bien el resultado ni la espera de confirmación

- **Decisión:** [reiniciarPartida()][reiniciarPartida] lleva a `INACTIVO` y dice que el sistema limpia el estado. No aclara si la partida sigue avanzando mientras se espera la confirmación.
- **Propuesta:** mantener ese destino y explicar que se vuelve a la pantalla inicial, con puntuación cero y opción de iniciar. Mientras se confirma, detener temporalmente la partida; si se cancela, volver al estado anterior sin cambios.
- **Argumento:** el nombre reiniciar podría entenderse como empezar otra partida directamente, pero aquí hace falta solicitar iniciar después. Además, si el tiempo sigue avanzando durante la confirmación, se podría perder antes de responder. La respuesta debe explicar qué ve el jugador, no solo una limpieza interna.

### D18. Game Over no explica cómo continuar

- **Decisión:** la última alternativa de [descenderPieza()][descenderPieza] presenta Game Over. El [contexto del jugador][jugador] permite reiniciar desde Finalizado, pero esa opción no se presenta en el detalle.
- **Propuesta:** mostrar la puntuación y el tablero final, permitir pedir reinicio y aclarar que ya no se puede mover o girar la pieza.
- **Argumento:** llegar a un estado no explica por sí solo qué información se conserva ni qué puede solicitar el jugador a continuación.

### D19. El prototipo no permite comprobar todos los casos de uso

- **Decisión:** el [apartado de prototipado][requisitos] dice que una sola imagen es suficiente. La [captura][imagen] muestra tablero, puntuación y controles 4, 6, 7 y 9. No muestra cómo iniciar, reiniciar, confirmar, cancelar o alternar la velocidad.
- **Propuesta:** añadir bocetos sencillos de inicio, confirmación y fin de partida; explicar los controles que faltan y qué ocurre con un comando no válido. Para la velocidad y el descenso, acompañarlos de una secuencia breve que muestre los cambios.
- **Argumento:** una captura no muestra movimiento ni permite distinguir dos velocidades. El texto de alternar tampoco explica la vuelta de acelerada a normal. No hace falta una pantalla por caso de uso, pero sí poder seguir las interacciones que se han definido. Las teclas concretas deben quedar aquí, sin mezclarlas con los detalles abstractos.

### D20. Faltan condiciones para comprobar la respuesta del sistema

- **Decisión:** los [detalles de casos de uso][requisitos] dicen qué se presenta, pero no cuánto puede tardar en aparecer ni con qué precisión se respeta el intervalo de caída.
- **Propuesta:** acordar un tiempo máximo de respuesta a las solicitudes y una tolerancia para los intervalos, indicando cómo se medirían.
- **Argumento:** un movimiento mostrado varios segundos después podría cumplir literalmente el texto y hacer que el juego no se pueda usar bien. No hace falta añadir muchas exigencias: solo las necesarias para comprobar la interacción.

## Criterios que he mantenido

No considero un error usar Tiempo como actor: los [apuntes de encontrar actores y casos de uso](https://github.com/mmasias/idsw1/blob/main/temario/contenidos/00006-CdU.eAyCdU.md) lo permiten. Tampoco veo mal usar diagramas de estados para los detalles, porque es una de las formas trabajadas en [detallar casos de uso](https://github.com/mmasias/idsw1/blob/main/temario/contenidos/00008-Cdu.dCdU.md).

He seguido también los apuntes de [modelo del dominio](https://github.com/mmasias/idsw1/blob/main/temario/contenidos/00004-MdD.md) y [prototipado](https://github.com/mmasias/idsw1/blob/main/temario/contenidos/00009-CdU.ICdU.md). El [ejemplo de hdC](https://github.com/mmasias/hdC/blob/main/docs/UMLdocs/mdd.md) me ha servido para ver cómo se complementan las distintas vistas.

No propongo añadir multijugador, niveles ni funciones avanzadas. He comparado la documentación del alcance indicado, no la implementación. También he contrastado los contenidos de las fuentes PlantUML con sus SVG publicados y no he encontrado diferencias en los puntos comentados.

[dominio]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/docs/modeloDelDominio.md
[requisitos]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/docs/ProcesoRequisitos.md
[modelo]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/modelosUML/ModeloDominio.puml
[casos]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/modelosUML/DiagramaCasosUso.puml
[estados]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/modelosUML/DiagramaDeContexto000.puml
[jugador]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/modelosUML/DiagramaDeContexto001-ActorJugador.puml
[tiempo]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/modelosUML/DiagramaDeContexto001-ActorTiempo.puml
[imagen]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/images/tetris.png
[descenderPieza]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/modelosUML/detalleCasosUso/descenderPieza.puml
[iniciarPartida]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/modelosUML/detalleCasosUso/iniciarPartida.puml
[moverPieza]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/modelosUML/detalleCasosUso/moverPieza.puml
[reiniciarPartida]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/modelosUML/detalleCasosUso/reiniciarPartida.puml
[rotarPieza]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/modelosUML/detalleCasosUso/rotarPieza.puml
[alternarVelocidadCaida]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/modelosUML/detalleCasosUso/alternarVelocidadCaida.puml
[arrancarSistema]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/modelosUML/detalleCasosUso/arrancarSistema.puml
