
# Reto 002 – Tetris

## 1. Incoherencias

### 1. Falta una transición en el diagrama general de estados

- **Artefactos analizados:** [Diagrama general de estados](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto000.puml) y [diagrama de contexto del Jugador](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorJugador.puml).

- **Incoherencia detectada:** En el diagrama general no aparece una transición desde EN_CURSO hasta INACTIVO. Sin embargo, en el diagrama del Jugador sí se contempla este cambio mediante reiniciarPartida().

- **Corrección propuesta y justificación:** Añadiría al diagrama general la transición de EN_CURSO a INACTIVO cuando se confirma el reinicio. Así se representarían los mismos cambios de estado en ambos diagramas y no habría diferencias entre ellos.

### 2. No se representa la posibilidad de cancelar un reinicio

- **Artefactos analizados:** [Diagrama de contexto del Jugador](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorJugador.puml) y [detalle de reiniciarPartida()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml).

- **Incoherencia detectada:** En el diagrama de contexto, reiniciarPartida() provoca el paso a INACTIVO. Sin embargo, en el detalle del caso de uso se permite cancelar el reinicio, manteniendo la partida en EN_CURSO o FINALIZADO, dependiendo del estado en el que estuviera.

- **Corrección propuesta y justificación:** Aclararía que solamente se pasa a INACTIVO cuando el jugador confirma el reinicio. Si lo cancela, debería conservarse el estado anterior. De esta forma, el diagrama de contexto también reflejaría el comportamiento alternativo.

### 3. Se utilizan nombres distintos para una misma funcionalidad

- **Artefactos analizados:** [Tabla de casos de uso](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#casos-de-uso) y [detalle de iniciarPartida()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/iniciarPartida.puml).

- **Incoherencia detectada:** En la tabla de requisitos se utiliza alternarVelocidadCaida(), mientras que en iniciarPartida() aparece acelerarCaida() como una de las acciones que puede solicitar el jugador.

- **Corrección propuesta y justificación:** Utilizaría alternarVelocidadCaida() en los dos artefactos, ya que permite expresar el cambio entre velocidad normal y acelerada. Mantener el mismo nombre evitaría confusiones al relacionar los distintos casos de uso.

### 4. No queda suficientemente definida la cantidad de piezas activas

- **Artefactos analizados:** [Modelo del dominio, apartado 7](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#7-refinamiento-de-relaciones) y [detalle de descenderPieza()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/descenderPieza.puml).

- **Incoherencia detectada:** El modelo del dominio establece una relación de uno a muchos entre Juego y las piezas activas. En cambio, descenderPieza() describe el descenso de una pieza que, cuando queda fijada, es sustituida por otra nueva.

- **Corrección propuesta y justificación:** Precisaría que solamente puede haber una pieza activa en cada momento. Aunque el tablero contenga varias piezas colocadas, únicamente una está siendo controlada. Esto permitiría diferenciar las piezas activas de las que ya forman parte del tablero.

### 5. El contexto del Tiempo muestra una transición que no está explicada

- **Artefactos analizados:** [Diagrama de contexto del Tiempo](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorTiempo.puml), [contexto del Jugador](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorJugador.puml) y [detalle de reiniciarPartida()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml).

- **Incoherencia detectada:** En el diagrama del Tiempo aparece una transición de FINALIZADO a INACTIVO sin indicar qué acción la provoca. En los otros artefactos, ese cambio está relacionado con la solicitud y confirmación de reinicio por parte del Jugador.

- **Corrección propuesta y justificación:** Aclararía qué provoca esa transición o la eliminaría del contexto del Tiempo si no corresponde a una acción suya. El Tiempo interviene en la caída automática de las piezas, mientras que el reinicio depende de una decisión del jugador.

---

## 2. Decisiones discutibles

### 1. Utilizar composición en dos relaciones que incluyen Celda

- **Artefacto analizado:** [Modelo del dominio, apartado 7](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#7-refinamiento-de-relaciones).

- **Decisión original:** Se representa mediante composición tanto la relación Tablero–Celda como Línea–Celda.

- **Alternativa propuesta y justificación:** Mantendría la composición entre Tablero y Celda, pero utilizaría una asociación entre Línea y Celda. Las celdas pertenecen al tablero y pueden formar líneas, pero no deberían tener dos propietarios mediante composición. Así las relaciones quedarían representadas de manera más adecuada.

### 2. Considerar Juego y Partida como un mismo concepto

- **Artefacto analizado:** [Modelo del dominio, apartado 3](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#3-primera-depuración-consolidación-de-sinónimos).

- **Decisión original:** Se eliminó Partida como concepto independiente y se mantuvo únicamente Juego, argumentando que Partida es más propio de juegos multijugador.

- **Alternativa propuesta y justificación:** Diferenciaría ambos conceptos. Juego podría representar el Tetris y sus reglas, mientras que Partida sería cada intento que realiza el jugador. Aunque sea un juego individual, una persona puede jugar varias partidas, por lo que considero que tienen significados diferentes.

### 3. Incluir Turno como entidad del dominio

- **Artefacto analizado:** [Modelo del dominio](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/ModeloDominio.puml).

- **Decisión original:** Se incluye Turno como una entidad relacionada con las piezas y los movimientos.

- **Alternativa propuesta y justificación:** Revisaría si es necesario mantener esta entidad. En Tetris las piezas descienden continuamente y el jugador puede realizar varios movimientos mientras caen. Por eso, utilizar Turno puede resultar confuso si no se explica qué representa exactamente. Lo sustituiría por un concepto relacionado con el ciclo de caída de una pieza o lo eliminaría si no aporta información importante.

### 4. Excluir al Jugador del modelo del dominio

- **Artefacto analizado:** [Modelo del dominio, apartado 6](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#6-decisiones-críticas-de-modelado).

- **Decisión original:** Se descartó Jugador porque el Tetris planteado es individual y no necesita gestionar varios participantes.

- **Alternativa propuesta y justificación:** Consideraría incluirlo, aunque solamente exista un jugador por partida. Es quien inicia el juego, mueve las piezas y toma decisiones. Su inclusión permitiría reflejar estas relaciones sin necesidad de convertir el Tetris en un juego multijugador.

### 5. Considerar el Tiempo como un actor

- **Artefacto analizado:** [Tabla de actores](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#actores).

- **Decisión original:** Se identifica al Tiempo como actor encargado de solicitar el descenso automático de las piezas.

- **Alternativa propuesta y justificación:** Lo trataría como un comportamiento automático del propio sistema, siempre que sea el juego quien controle los intervalos de caída. Los actores representan elementos externos y, si el descenso se gestiona internamente, no sería necesario incluir al Tiempo como actor independiente.

### 6. Volver al estado INACTIVO después de reiniciar

- **Artefacto analizado:** [Detalle de reiniciarPartida()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml).

- **Decisión original:** Cuando el jugador confirma el reinicio, se limpia la partida anterior y se vuelve al estado INACTIVO.

- **Alternativa propuesta y justificación:** Haría que comenzara directamente una partida nueva. Cuando alguien selecciona reiniciar, normalmente espera volver a jugar desde cero, por lo que tener que iniciar otra vez la partida supone un paso adicional que podría evitarse.

### 7. No indicar el sentido de rotación de las piezas

- **Artefacto analizado:** [Detalle de rotarPieza()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/rotarPieza.puml).

- **Decisión original:** Se contempla que el jugador solicite rotar una pieza y que el sistema compruebe si el giro es válido, pero no se especifica hacia qué lado se realiza.

- **Alternativa propuesta y justificación:** Añadiría el sentido de rotación solicitado, diferenciando entre izquierda y derecha, o aclararía que solamente existe una dirección de giro. Esto permitiría conocer el comportamiento esperado y comprobar correctamente cada posibilidad.

### 8. Considerar suficiente una sola imagen para el prototipado

- **Artefactos analizados:** [Apartado de prototipado](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#prototipar-casos-de-uso) y [prototipo del Tetris](https://github.com/puntoReflex/pyTetris/blob/main/images/tetris.png).

- **Decisión original:** Se considera que una captura del juego en funcionamiento es suficiente para representar los distintos casos de uso.

- **Alternativa propuesta y justificación:** Añadiría algunos prototipos para situaciones que no se muestran, como la confirmación de reinicio o el final de una partida. La imagen actual permite ver el tablero y las teclas para mover y rotar, pero no explica visualmente cómo se inicia una partida, cómo se cancela un reinicio o cómo se cambia la velocidad. No sería necesario representar todos los casos de uso, solamente aquellos que aporten información diferente.

### 9. Incluir arrancarSistema() como caso de uso del Jugador

- **Artefactos analizados:** [Tabla de casos de uso](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#casos-de-uso) y [detalle de arrancarSistema()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/arrancarSistema.puml).

- **Decisión original:** Se incluye arrancarSistema() entre los casos de uso que puede solicitar el Jugador.

- **Alternativa propuesta y justificación:** Separaría el arranque de la aplicación de las acciones relacionadas directamente con la partida. Aunque abrir el programa sea necesario, el objetivo principal del jugador es jugar y controlar las piezas. Así, los casos de uso estarían más centrados en los objetivos del actor.

### 10. No dejar claras las condiciones de salida de reiniciarPartida()

- **Artefacto analizado:** [Detalle de reiniciarPartida()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml).

- **Decisión original:** El caso de uso representa tres posibles estados de salida: INACTIVO, EN_CURSO y FINALIZADO. Sin embargo, las condiciones de cada transición no aparecen indicadas directamente en las salidas del diagrama.

- **Alternativa propuesta y justificación:** Añadiría las condiciones que corresponden a cada transición. Si se confirma el reinicio, se pasa a INACTIVO. Si se cancela, se mantiene el estado anterior. Esto facilitaría la interpretación del diagrama y evitaría tener que deducir qué salida corresponde a cada situación.

### 11. La validación con expertos del dominio no queda justificada

- **Artefacto analizado:** [Modelo del dominio, apartado de validación con expertos](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md).

- **Decisión original:** Se incluye una sección denominada «Validación con expertos del dominio», en la que aparecen varias preguntas respondidas con Sí o No, pero no se identifica quién ha realizado esa validación.

- **Alternativa propuesta y justificación:** Indicaría quién ha participado en la revisión y cómo se han obtenido las respuestas. Si solamente se trata de una comprobación realizada por los autores, cambiaría el nombre de la sección por «Autoevaluación del modelo». De esta manera, quedaría más claro qué tipo de revisión se ha llevado a cabo.

### 12. No explicar todas las relaciones que aparecen en el modelo final

- **Artefactos analizados:** [Modelo del dominio, apartado 7](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#7-refinamiento-de-relaciones) y [diagrama UML](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/ModeloDominio.puml).

- **Decisión original:** La tabla de refinamiento explica seis relaciones, pero el diagrama final contiene otras adicionales, como Juego–Turno, Turno–Pieza, Turno–Movimiento y Pieza–Celda.

- **Alternativa propuesta y justificación:** Completaría la tabla con las relaciones que faltan e indicaría sus multiplicidades. Aunque el diagrama ya las represente, considero que la explicación escrita debería recogerlas para facilitar la comprensión del modelo completo.
