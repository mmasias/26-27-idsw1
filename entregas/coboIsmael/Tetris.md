# Reto 002 - Tetris

## 1. Incoherencias

### 1. Nombre de la acción para acelerar la caída

- **Dónde:** [diagrama de casos de uso](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaCasosUso.puml) y [detalle de `iniciarPartida()`](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/iniciarPartida.puml).
- **Qué dice cada uno:** el diagrama general utiliza `alternarVelocidadCaida()`, mientras que el detalle de inicio menciona `acelerarCaida()`.
- **Qué corregiría y por qué:** usaría el mismo nombre en ambos. Si la acción consiste en alternar la velocidad, cambiaría el detalle a `alternarVelocidadCaida()` para evitar confusiones.

### 2. Reiniciar la partida

- **Dónde:** [diagrama de contexto del jugador](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorJugador.puml) y [detalle de `reiniciarPartida()`](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml).
- **Qué dice cada uno:** en el diagrama de contexto, `reiniciarPartida()` lleva directamente a `Inactivo`; en el detalle, el jugador tiene que confirmar el reinicio y también puede cancelarlo.
- **Qué corregiría y por qué:** indicaría en el diagrama de contexto que el cambio a `Inactivo` solo ocurre si se confirma. Así queda claro que cancelar mantiene el estado anterior.

### 3. Salida desde el estado finalizado

- **Dónde:** [diagrama de contexto general](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto000.puml) y [diagrama de contexto del jugador](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorJugador.puml).
- **Qué dice cada uno:** el general representa una salida desde `Finalizado` hasta el estado final del sistema; el del jugador solo muestra que desde `Finalizado` se puede reiniciar.
- **Qué corregiría y por qué:** aclararía qué acción provoca el cierre y la añadiría al modelo del jugador si le corresponde. Si no existe esa opción, quitaría la transición del diagrama general.

## 2. Decisiones discutibles

### 1. Considerar «Tiempo» como actor

- **Decisión:** en el [diagrama de casos de uso](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaCasosUso.puml), `Tiempo` aparece como actor de `descenderPieza()`.
- **Propuesta:** aclararía si se trata de un temporizador externo; si es parte del propio juego, no lo pondría como actor.
- **Argumento:** los actores representan elementos externos al sistema, y el tiempo puede ser simplemente una condición interna.

### 2. Usar nombres de métodos para los casos de uso

- **Decisión:** el [diagrama de casos de uso](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaCasosUso.puml) utiliza nombres como `moverPieza()` y `iniciarPartida()`.
- **Propuesta:** escribiría «Mover pieza» e «Iniciar partida».
- **Argumento:** los casos de uso deberían expresar acciones del usuario, no parecer funciones de programación.

### 3. No indicar las multiplicidades en el diagrama del dominio

- **Decisión:** el [modelo del dominio](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/ModeloDominio.puml) representa relaciones entre conceptos sin mostrar sus multiplicidades.
- **Propuesta:** añadiría valores como `1` o `*` donde corresponda, siguiendo las relaciones descritas en la [documentación](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md).
- **Argumento:** permitiría saber directamente cuántos elementos se relacionan, sin consultar otra tabla.

### 4. Mostrar un único prototipo

- **Decisión:** en la [sección de prototipado](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#prototipar-casos-de-uso) se utiliza una sola [imagen del juego](https://github.com/puntoReflex/pyTetris/blob/main/images/tetris.png).
- **Propuesta:** añadiría imágenes sencillas de la pantalla inicial, la confirmación de reinicio y el final de partida.
- **Argumento:** una imagen de la partida en curso no permite comprobar cómo son las demás interacciones.

### 5. No explicar completamente las dos rotaciones

- **Decisión:** el [prototipo](https://github.com/puntoReflex/pyTetris/blob/main/images/tetris.png) muestra dos controles de rotación, pero el [detalle de `rotarPieza()`](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/rotarPieza.puml) solo habla de rotar, sin distinguir las direcciones.
- **Propuesta:** especificaría qué hace cada tecla y qué ocurre cuando la rotación no es válida.
- **Argumento:** así el jugador y quien desarrolle el juego entenderán exactamente el comportamiento esperado.
