# Revisión del análisis y modelado de pyTetris

## 1. Aspectos que presentan contradicciones o falta de claridad

### 1.1. Diferencias en el nombre de la acción de cambiar la velocidad

**Referencias:** [Lista de casos de uso](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md) y [detalle de iniciarPartida()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/iniciarPartida.puml).

En la documentación se utilizan dos nombres para referirse a una acción que parece ser la misma. En la lista de casos de uso aparece `alternarVelocidadCaida()`, mientras que en la explicación del inicio de una partida se utiliza `acelerarCaida()`.

Considero que ambos nombres no expresan exactamente lo mismo, ya que acelerar implica aumentar la velocidad, mientras que alternar permite cambiar entre dos velocidades.

**Mi propuesta:** Mantendría `alternarVelocidadCaida()` en ambos documentos, porque representa mejor el comportamiento que se quiere ofrecer al jugador y evita utilizar dos nombres diferentes para una misma acción.

### 1.2. Dudas sobre la existencia de niveles de dificultad

**Referencia:** [Modelo del dominio](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md).

Al revisar la identificación de conceptos, las decisiones de modelado y las simplificaciones, he encontrado información que puede resultar contradictoria. Por una parte, se incluye el nivel como concepto relacionado con la dificultad actual del juego. Sin embargo, posteriormente se indica que no existen niveles de dificultad.

**Mi propuesta:** Aclararía si el nivel aumenta automáticamente durante la partida o si se trata de una dificultad que selecciona el jugador antes de comenzar. En caso de que ninguna de estas opciones forme parte del juego, eliminaría las referencias al nivel para evitar confusiones.

### 1.3. Problemas con la composición de las celdas

**Referencias:** [Tabla de relaciones](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md) y [diagrama del modelo del dominio](https://github.com/puntoReflex/pyTetris/blob/main/images/modelosUML/ModeloDominio.svg).

En el modelo se indica que el tablero está compuesto por celdas y, al mismo tiempo, que las líneas también están compuestas por esas mismas celdas. Esto puede generar un problema, puesto que se está utilizando una relación de composición para representar dos propietarios diferentes.

**Mi propuesta:** Organizaría la estructura de manera jerárquica: el tablero estaría formado por líneas y cada línea contendría sus propias celdas. También podría representarse la línea como una agrupación lógica de celdas, sin establecer una segunda composición.

### 1.4. Comportamiento diferente al reiniciar una partida

**Referencias:** [Diagrama de contexto del jugador](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorJugador.puml) y [detalle de reiniciarPartida()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml).

He observado que los dos documentos no representan exactamente el mismo comportamiento. En el diagrama de contexto, al reiniciar se vuelve directamente al estado inactivo. Sin embargo, en el detalle del caso de uso se indica que primero aparece una confirmación y que el jugador puede cancelar la operación.

**Mi propuesta:** Especificaría que el cambio al estado inactivo solamente se produce cuando el jugador confirma el reinicio. Si decide cancelarlo, debería conservarse el estado en el que se encontraba anteriormente.

### 1.5. Falta de diferenciación entre los distintos resultados

**Referencias:** [moverPieza()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/moverPieza.puml), [rotarPieza()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/rotarPieza.puml), [descenderPieza()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/descenderPieza.puml) y [reiniciarPartida()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml).

En estos diagramas aparecen diferentes posibilidades durante la ejecución, como realizar un movimiento correctamente, encontrar un bloqueo o confirmar y cancelar un reinicio. Sin embargo, los caminos no siempre permiten identificar claramente qué resultado corresponde a cada salida.

**Mi propuesta:** Añadiría condiciones o etiquetas a las distintas transiciones, indicando situaciones como «movimiento válido», «movimiento bloqueado», «reinicio confirmado» o «reinicio cancelado». De esta forma, sería más sencillo seguir los diagramas y comprender el comportamiento del sistema.

### 1.6. Orden de las operaciones al descender una pieza

**Referencias:** [detalle de descenderPieza()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/descenderPieza.puml) y [detalle de moverPieza()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/moverPieza.puml).

En el caso de uso de descenso, primero se muestra la pieza en su nueva posición y después se comprueba si ha llegado al fondo o si existe una colisión. En cambio, cuando se realiza un movimiento lateral, primero se comprueba que la nueva posición sea válida.

**Mi propuesta:** Seguiría el mismo orden en ambos casos. Primero comprobaría si la pieza puede descender y, si es posible, actualizaría su posición. Si no puede bajar, permanecería en su posición actual y se continuaría con el proceso de fijación.

### 1.7. Información incompleta sobre los controles de velocidad

**Referencias:** [Documento de requisitos](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md), [detalle de alternarVelocidadCaida()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/alternarVelocidadCaida.puml) y [prototipo](https://github.com/puntoReflex/pyTetris/blob/main/images/tetris.png).

Los requisitos contemplan la posibilidad de cambiar la velocidad de caída de las piezas. Sin embargo, en el prototipo únicamente aparecen las teclas para moverlas y rotarlas. Por tanto, no queda representado cómo se utiliza esta otra funcionalidad.

**Mi propuesta:** Incorporaría al prototipo el control correspondiente al cambio de velocidad o añadiría una explicación indicando que todavía no está representado. Así se evitaría dar por hecho que la imagen muestra todas las interacciones disponibles.

### 1.8. Ausencia de una pantalla para confirmar el reinicio

**Referencias:** [detalle de reiniciarPartida()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml) y [prototipado de requisitos](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md).

El detalle del reinicio explica que el jugador puede confirmar o cancelar la operación. No obstante, el prototipo solamente presenta una partida en funcionamiento y no incluye ninguna ventana o mensaje que represente esta confirmación.

**Mi propuesta:** Añadiría una captura donde apareciese el mensaje de confirmación junto con las dos opciones disponibles. Esto permitiría comprender mejor cómo se presenta esta situación al jugador.

### 1.9. Diferencias en la transición desde el estado finalizado

**Referencias:** [Contexto del jugador](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorJugador.puml) y [contexto de tiempo](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorTiempo.puml).

En el diagrama correspondiente al jugador, el paso desde «Finalizado» hasta «Inactivo» se produce mediante `reiniciarPartida()`. Sin embargo, en el diagrama del tiempo aparece la misma transición, pero no se indica qué acción la provoca.

**Mi propuesta:** Añadiría la acción que origina ese cambio o explicaría que se ha omitido porque el segundo diagrama se centra únicamente en el comportamiento del tiempo. De esta manera, quedaría claro si la transición depende del jugador o se produce automáticamente.

---

## 2. Propuestas de mejora del modelo

Además de las contradicciones anteriores, hay algunas decisiones de diseño que considero que podrían haberse planteado de otra manera. No significa necesariamente que las soluciones actuales sean incorrectas, sino que existen otras alternativas que, desde mi punto de vista, podrían representar mejor el funcionamiento del juego.

### 2.1. Representación de las piezas activas

**Referencia:** [Modelo del dominio](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md).

En la tabla de relaciones se permite que un juego tenga asociadas varias piezas activas. Sin embargo, durante una partida normal de Tetris solamente existe una pieza que está cayendo en cada momento.

**Mi propuesta:** Diferenciaría la pieza que está actualmente en movimiento de aquellas que ya han quedado colocadas en el tablero. De esta forma, el modelo representaría de manera más fiel el funcionamiento habitual del juego.

### 2.2. Información sobre las multiplicidades

**Referencia:** [Diagrama del modelo del dominio](https://github.com/puntoReflex/pyTetris/blob/main/images/modelosUML/ModeloDominio.svg).

Aunque en la tabla de relaciones se especifican las cantidades de elementos relacionados, como tableros, piezas o celdas, estas multiplicidades no aparecen reflejadas en el diagrama final.

**Mi propuesta:** Añadiría las multiplicidades directamente en las asociaciones del diagrama. Así se podría interpretar el modelo completo sin tener que consultar continuamente la tabla.

### 2.3. Tamaño y dimensiones del tablero

**Referencia:** [Identificación de conceptos del modelo del dominio](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md).

En la identificación inicial de conceptos se establece que el tablero tiene unas dimensiones de 10 × 20, pero esta información no se conserva de forma explícita en el modelo final.

**Mi propuesta:** Incluiría las dimensiones del tablero, indicando que dispone de 10 columnas y 20 filas. Considero que es un dato importante para definir las características del Tetris que se quiere desarrollar.

### 2.4. Utilidad del concepto de turno

**Referencia:** [Modelo del dominio](https://github.com/puntoReflex/pyTetris/blob/main/images/modelosUML/ModeloDominio.svg).

El modelo incorpora el concepto `Turno` y lo relaciona con las piezas y los movimientos. Sin embargo, no queda suficientemente explicado qué representa exactamente dentro de un juego como Tetris.

**Mi propuesta:** Explicaría que un turno comienza cuando aparece una pieza y termina cuando esta queda fijada, siempre que esa sea la interpretación que se pretende utilizar. Si no tiene una función concreta, consideraría eliminarlo, ya que Tetris no funciona mediante turnos tradicionales.

### 2.5. Simplificación de los tipos de piezas

**Referencia:** [Modelo del dominio](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md).

Se ha decidido representar `TipoPieza` como una clase independiente. No obstante, si su única finalidad es identificar la forma de cada pieza, podría resultar una solución algo más compleja de lo necesario.

**Mi propuesta:** Utilizaría una lista cerrada con los siete tipos de piezas disponibles: I, O, T, S, Z, J y L. Mantendría la clase únicamente si cada tipo necesita almacenar información adicional o tener un comportamiento propio.

### 2.6. Justificación de la ausencia de la clase Jugador

**Referencia:** [Modelo del dominio](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md).

Se ha decidido no incluir la clase `Jugador` porque el juego está pensado para una única persona. Sin embargo, considero que jugar individualmente no implica necesariamente que no sea útil representar al jugador.

**Mi propuesta:** Primero analizaría si es necesario guardar información como el nombre del usuario o sus puntuaciones. Si el sistema no necesita almacenar ningún dato relacionado con él, entonces sí mantendría su exclusión. Que aparezca como actor en los requisitos y no como clase del dominio no supone por sí mismo una contradicción.

### 2.7. Papel del tiempo como actor

**Referencia:** [Documento de requisitos](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md).

En los requisitos se representa el tiempo como un actor que solicita que las piezas desciendan automáticamente. No obstante, no queda claro si existe algún elemento externo que controle este comportamiento o si es el propio juego quien lo gestiona.

**Mi propuesta:** Aclararía quién controla realmente los intervalos de caída. Si es el propio sistema el que decide cuándo debe descender una pieza, no consideraría necesario representar el tiempo como un actor independiente.

### 2.8. Nombres utilizados en los casos de uso

**Referencia:** [Diagrama de casos de uso](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaCasosUso.puml).

Los casos de uso aparecen con nombres como `moverPieza()`, `rotarPieza()` y `reiniciarPartida()`. Estos nombres recuerdan más a funciones de programación que a las acciones que realiza un usuario.

**Mi propuesta:** Cambiaría los nombres por «Mover pieza», «Rotar pieza» y «Reiniciar partida». Creo que esta forma resulta más apropiada para describir las funcionalidades desde el punto de vista del jugador.

### 2.9. Representación de los diagramas de contexto

**Referencia:** [Diagramas de contexto](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md).

Los diagramas presentan principalmente los estados «Inactivo», «EnCurso» y «Finalizado», mostrando cómo se pasa de uno a otro.

**Mi propuesta:** Los identificaría como diagramas de estados o como representaciones del recorrido de cada actor. Para mostrar realmente el contexto del sistema, añadiría también un diagrama en el que apareciesen los actores externos, sus interacciones y los límites del juego.

### 2.10. Diferenciar entre reiniciar y volver al inicio

**Referencia:** [detalle de reiniciarPartida()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml).

Actualmente, cuando el jugador confirma el reinicio, el sistema vuelve al estado `INACTIVO`.

**Mi propuesta:** Diferenciaría las acciones de «Reiniciar partida» y «Volver al inicio». La primera debería comenzar una nueva partida desde cero, mientras que la segunda llevaría al jugador a la pantalla inicial. Aunque la solución actual puede ser válida, creo que separar ambos conceptos evitaría posibles confusiones.

### 2.11. Explicación de las teclas de rotación

**Referencias:** [Prototipo del juego](https://github.com/puntoReflex/pyTetris/blob/main/images/tetris.png) y [detalle de rotarPieza()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/rotarPieza.puml).

En el prototipo aparecen dos controles diferentes: `7=rotarS` y `9=rotarU`. Sin embargo, el detalle del caso de uso solamente explica que se puede rotar una pieza.

**Mi propuesta:** Añadiría una explicación sobre la función de cada tecla, indicando si permiten girar la pieza en sentidos contrarios. No crearía necesariamente dos casos de uso distintos, pero sí dejaría documentadas las dos posibilidades.

### 2.12. Reglas para obtener puntuación

**Referencia:** [detalle de descenderPieza()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/descenderPieza.puml).

En el documento se indica que la puntuación se actualiza cuando se eliminan líneas, pero no se especifica cuántos puntos corresponden a cada situación.

**Mi propuesta:** Establecería una regla que indicase la puntuación obtenida al eliminar una, dos, tres o más líneas. Así se evitarían interpretaciones diferentes durante el desarrollo del juego.

### 2.13. Diferenciar el arranque del sistema y el comienzo de una partida

**Referencia:** [Diagrama general de estados](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto000.puml).

Se utilizan las acciones `arrancarSistema()` e `iniciarPartida()`, pero no queda completamente diferenciada la función de cada una. Además, en el diagrama general aparece una salida final sin explicar cómo se cierra el programa.

**Mi propuesta:** Separaría el funcionamiento general de la aplicación del desarrollo de una partida. Así se distinguiría claramente entre abrir o cerrar el programa y comenzar o terminar una partida.

### 2.14. Funcionalidad de pausa sin desarrollar

**Referencias:** [Tabla de responsabilidades del dominio](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md) y [documento de requisitos](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md).

En la tabla de responsabilidades se menciona que la partida puede pausarse. Sin embargo, esta posibilidad no aparece después como un caso de uso en los requisitos.

**Mi propuesta:** Aclararía si la pausa forma parte de las funcionalidades previstas o si se ha descartado para esta versión. No considero imprescindible incorporarla, pero sí explicar por qué aparece mencionada en un documento y no en el otro.

### 2.15. Definición de las velocidades de caída

**Referencia:** [Documento de requisitos](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md).

Se contemplan dos velocidades de caída y un descenso automático mediante intervalos de tiempo, pero no se especifica completamente cómo funciona el cambio entre ellas.

**Mi propuesta:** Definiría qué se considera velocidad normal y velocidad acelerada, cómo puede cambiar el jugador entre ambas y si la velocidad seleccionada se mantiene cuando aparece una pieza nueva. Esto permitiría establecer unas reglas claras para comprobar el comportamiento del juego.

---

## 3. Conclusión

Después de revisar los documentos y los diagramas de pyTetris, considero que hay varios aspectos que deberían aclararse para conseguir que todos los elementos del proyecto sigan los mismos criterios.

Las cuestiones que más destacaría son las diferencias en el nombre de la acción de cambiar la velocidad, la representación de las celdas mediante composiciones y las distintas interpretaciones del reinicio de una partida. Son los ejemplos en los que resulta más evidente que dos partes de la documentación no terminan de coincidir.

Por otro lado, también considero que algunos elementos del modelo podrían simplificarse o explicarse mejor, especialmente los relacionados con las piezas, los turnos, el jugador y las reglas de puntuación.

En general, no creo que sea necesario complicar el diseño añadiendo más funcionalidades, sino conseguir que las que ya están definidas se entiendan correctamente y estén representadas de forma coherente en todos los documentos.