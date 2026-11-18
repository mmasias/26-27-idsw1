# RETO 002 – Tetris

## 1. Incoherencias

### 1.1. El cambio de velocidad recibe dos nombres distintos

**Dónde lo he encontrado:** En la [lista de casos de uso](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md) y en el [detalle de iniciarPartida()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/iniciarPartida.puml).

**Qué afirma cada uno:** La lista incluye `alternarVelocidadCaida()`, mientras que la descripción del inicio de una partida menciona `acelerarCaida()`. No me parece que sean exactamente lo mismo: alternar permite cambiar entre una velocidad y otra, y acelerar solo implica ir más rápido.

**Qué cambiaría y por qué:** Emplear `alternarVelocidadCaida()` en ambos lugares. De esa manera se ve que es la misma acción y que el jugador puede regresar a la velocidad normal.

### 1.2. No está claro si el juego tiene niveles

**Dónde lo he encontrado:** En los apartados de [identificación de conceptos, decisiones de modelado y simplificaciones](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md).

**Qué afirma cada uno:** Al principio el nivel se identifica como la dificultad actual. Más adelante se decide mantenerlo como una propiedad del juego. Pero en las simplificaciones se indica que no existen niveles de dificultad.

**Qué cambiaría y por qué:** Precisar si el nivel sube a medida que se juega o si se trata de elegir una dificultad antes de comenzar. Si no se va a usar ninguna de las dos opciones, eliminar la referencia al nivel. En el estado actual, se puede entender que el nivel existe y que no existe a la vez.

### 1.3. Las celdas pertenecen a dos elementos diferentes

**Dónde lo he encontrado:** En la [tabla de relaciones](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md) y en el [diagrama final del dominio](https://github.com/puntoReflex/pyTetris/blob/main/images/modelosUML/ModeloDominio.svg).

**Qué afirma cada uno:** Se indica que el tablero está compuesto por celdas y, a la vez, que cada línea está compuesta por celdas. En ambos casos se utiliza una relación de composición, es decir, se da a entender que las celdas son partes propias de los dos elementos.

**Qué cambiaría y por qué:** Haría que el tablero se compusiera de líneas y cada línea de sus celdas. Otra posibilidad sería considerar la línea únicamente como una agrupación de celdas del tablero, sin una segunda composición. El motivo es que, en este tipo de relación, una misma celda no debería tener dos dueños al mismo tiempo.

### 1.4. Reiniciar no conduce siempre al mismo estado

**Dónde lo he encontrado:** En el [diagrama de contexto del jugador](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorJugador.puml) y en el [detalle de reiniciarPartida()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml).

**Qué afirma cada uno:** El diagrama de contexto indica que reiniciar devuelve al estado inactivo. El detalle, en cambio, explica que primero se pide una confirmación y que el jugador puede cancelar; si lo hace, todo sigue como estaba.

**Qué cambiaría y por qué:** Reflejar en el diagrama de contexto que se vuelve a inactivo **solo cuando se confirma** el reinicio, y dejar claro que, si se cancela, se conserva el estado anterior. Así los dos diagramas cuentan lo mismo.

### 1.5. Los resultados de algunos casos de uso no se distinguen bien

**Dónde lo he encontrado:** En los detalles de [moverPieza()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/moverPieza.puml), [rotarPieza()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/rotarPieza.puml), [descenderPieza()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/descenderPieza.puml) y [reiniciarPartida()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml).

**Qué afirman:** Dentro de los diagramas se diferencia, por ejemplo, entre un movimiento válido y otro bloqueado, o entre confirmar y cancelar un reinicio. Sin embargo, esos caminos acaban en el mismo punto interno y después aparecen varias salidas sin señalar con claridad cuál corresponde a cada resultado.

**Qué cambiaría y por qué:** Etiquetar las condiciones de cada salida: «movimiento válido», «movimiento bloqueado», «reinicio confirmado», «reinicio cancelado», etc. Con eso se seguiría mejor el recorrido y no quedarían dudas sobre lo que ocurre en cada caso.

### 1.6. El descenso se muestra antes de comprobar si es posible

**Dónde lo he encontrado:** En el [detalle de descenderPieza()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/descenderPieza.puml) y, como comparación, en el [detalle de moverPieza()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/moverPieza.puml).

**Qué afirma cada uno:** Al descender, primero se dice que la pieza se presenta en su nueva posición y después se comprueba si ha tocado el fondo o ha chocado. En el movimiento lateral ocurre al revés: primero se verifica que la posición sea válida.

**Qué cambiaría y por qué:** Comprobar antes si la pieza puede bajar. Si puede, mostrarla una fila más abajo; si no, dejarla donde está y seguir con el proceso de fijarla. Me parece más lógico, y evita enseñar una posición que podría ser imposible.

### 1.7. El prototipo no explica cómo cambiar la velocidad

**Dónde lo he encontrado:** En la [lista de casos de uso](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md), en el [detalle del cambio de velocidad](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/alternarVelocidadCaida.puml) y en la [imagen del prototipo](https://github.com/puntoReflex/pyTetris/blob/main/images/tetris.png).

**Qué afirma cada uno:** Los requisitos contemplan la opción de alternar la velocidad. La imagen, sin embargo, solo muestra las teclas para mover y rotar las piezas. Además, el texto da a entender que esa imagen basta para conocer todas las interacciones.

**Qué cambiaría y por qué:** Añadir el control para cambiar la velocidad o aclarar que esta función aún no aparece en la imagen. No afirmaría que la función no existe en el juego, porque la captura por sí sola no lo demuestra. Lo que falta es mostrar cómo se usa.

### 1.8. El prototipo tampoco muestra la confirmación del reinicio

**Dónde lo he encontrado:** En el [detalle de reiniciarPartida()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml) y en el apartado de [prototipado de requisitos](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md).

**Qué afirma cada uno:** El detalle explica que el jugador debe confirmar o cancelar el reinicio. La imagen, en cambio, muestra únicamente una partida en marcha, sin ninguna pantalla ni mensaje de confirmación. Aun así, el documento presenta esa imagen como suficiente para cubrir los casos de uso.

**Qué cambiaría y por qué:** Incorporar una imagen de la confirmación con sus dos opciones. Me parece necesaria para entender qué verá el jugador y cómo decidirá si reinicia o continúa.

### 1.9. El regreso desde «Finalizado» no se explica de la misma manera

**Dónde lo he encontrado:** En el [contexto del jugador](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorJugador.puml) y en el [contexto de tiempo](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorTiempo.puml).

**Qué afirma cada uno:** En el primero, el paso de finalizado a inactivo aparece con `reiniciarPartida()`. En el segundo, ese mismo cambio se dibuja sin señalar qué lo provoca.

**Qué cambiaría y por qué:** Indicar la acción que origina el cambio, o explicar que el segundo diagrama la omite porque solo recoge lo relacionado con el tiempo. De lo contrario, es difícil saber si la transición es automática o depende del jugador.

## 2. Decisiones que habría resuelto de otro modo

### 2.1. El número de piezas activas

**Qué se decidió:** En la [tabla de relaciones del dominio](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md) se admite que un juego esté asociado a varias piezas activas.

**Qué propongo y por qué:** Separaría la pieza que está cayendo de las que ya han quedado colocadas. En una partida normal solo hay una pieza activa en cada momento. De ese modo el modelo reproduciría mejor lo que sucede en el juego.

### 2.2. Faltan las cantidades en las relaciones del diagrama

**Qué se decidió:** La tabla detalla cuántos tableros, piezas o celdas se relacionan entre sí, pero esas cantidades no figuran en el [diagrama final](https://github.com/puntoReflex/pyTetris/blob/main/images/modelosUML/ModeloDominio.svg).

**Qué propongo y por qué:** Añadir esas cantidades al dibujo. Me resultaría más sencillo entenderlo sin tener que mirar una tabla aparte. No lo veo como una contradicción, sino como información que echo en falta en el diagrama.

### 2.3. El modelo no recoge las dimensiones del tablero

**Qué se decidió:** En la [lista inicial de conceptos](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md) se menciona un tablero de 10 × 20, pero el modelo no incluye ese dato.

**Qué propongo y por qué:** Si se pretende representar este Tetris en concreto, indicaría que el tablero tiene 10 columnas y 20 filas. Con eso quedaría fijada una de sus reglas básicas, sin necesidad de decidir todavía cómo se programa.

### 2.4. El concepto de «turno» no me parece lo bastante claro

**Qué se decidió:** `Turno` forma parte del [modelo del dominio](https://github.com/puntoReflex/pyTetris/blob/main/images/modelosUML/ModeloDominio.svg) y se relaciona con las piezas y los movimientos.

**Qué propongo y por qué:** Explicaría que un turno comienza cuando aparece una pieza y termina cuando esta queda fija, si es eso lo que se quiere representar. Si el concepto no hace falta, lo eliminaría. En Tetris las piezas descienden de forma continua y no hay turnos tan definidos como en otros juegos.

### 2.5. Los tipos de pieza podrían representarse de manera más simple

**Qué se decidió:** `TipoPieza` aparece como una clase independiente en el [modelo del dominio](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md).

**Qué propongo y por qué:** Consideraría una lista cerrada con los siete tipos: I, O, T, S, Z, J y L. Si cada tipo tuviera más información o reglas propias, conservaría la clase. Pero si solo sirve para saber qué forma tiene una pieza, la lista me parece suficiente.

### 2.6. No me convence el motivo para descartar a «Jugador»

**Qué se decidió:** El [modelo del dominio](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md) excluye `Jugador` con el argumento de que Tetris se juega de forma individual.

**Qué propongo y por qué:** Decidiría si hace falta guardar información del jugador, como su nombre o sus puntuaciones, aunque juegue una sola persona. Si no hay que guardar nada sobre él, entonces sí lo quitaría. Que figure como actor en los requisitos y no como clase del dominio no lo considero un error: en cada sitio cumple funciones distintas.

### 2.7. No tengo claro que «Tiempo» deba ser un actor

**Qué se decidió:** En los [requisitos](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md) el tiempo es un actor y es quien pide que las piezas desciendan.

**Qué propongo y por qué:** Aclararía si existe algún reloj externo que avise al juego. Si es el propio juego quien controla cuándo bajan las piezas, no pondría el tiempo como un actor aparte. Me resulta más fácil de entender como una regla del funcionamiento del juego.

### 2.8. Los casos de uso parecen nombres de funciones

**Qué se decidió:** En el [diagrama de casos de uso](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaCasosUso.puml) se emplean nombres como `moverPieza()`, `rotarPieza()` y `reiniciarPartida()`.

**Qué propongo y por qué:** Los llamaría «Mover pieza», «Rotar pieza» y «Reiniciar partida». Así se entiende mejor lo que quiere hacer el jugador, sin que parezca que ya estamos definiendo funciones del programa.

### 2.9. Los diagramas de contexto muestran sobre todo estados

**Qué se decidió:** En el apartado de [diagramas de contexto](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md) figuran `Inactivo`, `EnCurso` y `Finalizado`, junto con las transiciones entre ellos.

**Qué propongo y por qué:** Los presentaría como diagramas de estados o como vistas del recorrido de cada actor. Para mostrar el contexto del sistema usaría además un dibujo en el que se viera qué actores interactúan con el juego y dónde está su límite.

### 2.10. Reiniciar conduce a la pantalla inicial

**Qué se decidió:** El [detalle de reiniciarPartida()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml) termina en `INACTIVO` cuando el jugador confirma.

**Qué propongo y por qué:** Diferenciaría entre «Reiniciar partida», que comenzaría una nueva desde cero, y «Volver al inicio», que nos llevaría de regreso a la pantalla inicial. La solución actual puede ser válida, pero el nombre puede inducir a confusión.

### 2.11. Los dos botones de rotación no tienen explicación

**Qué se decidió:** El [prototipo](https://github.com/puntoReflex/pyTetris/blob/main/images/tetris.png) muestra `7=rotarS` y `9=rotarU`, pero el [detalle de rotarPieza()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/rotarPieza.puml) solo habla de rotar una pieza.

**Qué propongo y por qué:** Explicaría qué hace cada tecla y si giran en sentidos opuestos. No creo necesario crear dos casos de uso distintos, pero sí indicar que hay dos opciones.

### 2.12. No se explica cómo se calculan los puntos

**Qué se decidió:** El [detalle de descenderPieza()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/descenderPieza.puml) dice que la puntuación se actualiza al eliminar líneas.

**Qué propongo y por qué:** Incluiría una regla sencilla que diga cuántos puntos se obtienen al eliminar una, dos o más líneas. Sin esa información, dos versiones del juego podrían dar puntuaciones distintas y ambas cumplirían lo que está escrito.

### 2.13. Se confunde el inicio del juego con el inicio de una partida

**Qué se decidió:** Se definen `arrancarSistema()` e `iniciarPartida()`. Además, el [diagrama general de estados](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto000.puml) muestra una salida final sin explicar cómo se cierra el programa.

**Qué propongo y por qué:** Separaría el momento de abrir o cerrar el programa del momento de jugar una partida. Así sería más fácil ver qué estados pertenecen a la aplicación y cuáles a la partida.

### 2.14. Se menciona la posibilidad de pausar, pero luego desaparece

**Qué se decidió:** En la [tabla de responsabilidades del dominio](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md) se señala que el juego puede pausarse, pero en los [requisitos](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md) no hay ningún caso de uso para hacerlo.

**Qué propongo y por qué:** Aclararía si la pausa queda fuera de esta versión o si de verdad se quiere incluir. No considero obligatorio añadirla, pero sí dejar explicada la decisión.

### 2.15. Las velocidades de caída están poco definidas

**Qué se decidió:** El [documento de requisitos](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md) habla de una caída automática por intervalos y de dos velocidades, pero no concreta cuándo cambia cada una ni qué sucede después.

**Qué propongo y por qué:** Definiría, como mínimo, qué significan velocidad normal y velocidad acelerada, cómo se pasa de una a otra y si, al aparecer una pieza nueva, se mantiene la velocidad elegida. Así se podría comprobar si el comportamiento es el esperado.

## 3. Valoración final

Entre los problemas que he detectado, estan el cambio de nombre de la función de velocidad que es el más obvio, la manera de relacionar las celdas en el modelo y la diferencia entre el reinicio descrito en el contexto y el reinicio detallado. En esos tres casos puedo señalar dos partes concretas de la documentación que no encajan entre sí. 

Aparte, modificaría la forma de explicar algunos casos de uso y ampliaría la información del prototipo. No lo haría para que el juego fuese más complejo, sino porque hay situaciones que yo no sabría interpretar de una única manera. 

En cuanto a las decisiones discutibles, no doy por hecho que mi propuesta sea la única correcta. Son cambios que haría para que el modelo me resultara más fácil de entender y para que el comportamiento del juego quedara mejor explicado.