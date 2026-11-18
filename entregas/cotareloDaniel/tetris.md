# Reto 002 – Tetris

## 1. Incoherencias

### 1. Falta una transición de reinicio en el diagrama general

- **Dónde:** [Estados del sistema](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto000.puml) y [diagrama de contexto del Jugador](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorJugador.puml).
- **Qué dice cada uno:** El diagrama general no recoge el paso de EN_CURSO a INACTIVO. El del Jugador sí lo permite mediante `reiniciarPartida()`.
- **Qué corregiría y por qué:** Añadiría esa transición al diagrama general, porque debe recoger todos los cambios posibles entre los estados principales.

### 2. El reinicio puede cancelarse, pero el diagrama de contexto no lo muestra

- **Dónde:** [Diagrama de contexto del Jugador](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorJugador.puml) y [detalle de `reiniciarPartida()`](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml).
- **Qué dice cada uno:** El primero representa `reiniciarPartida()` como un paso directo a INACTIVO. El segundo permite cancelar el reinicio y conservar el estado EN_CURSO o FINALIZADO.
- **Qué corregiría y por qué:** Aclararía en el diagrama de contexto que el cambio a INACTIVO solo ocurre si se confirma el reinicio. Si se cancela, la partida sigue como estaba.

### 3. No queda clara la cantidad de piezas activas

- **Dónde:** [Modelo del dominio, apartado 7](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#7-refinamiento-de-relaciones), y detalles de [`iniciarPartida()`](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/iniciarPartida.puml) y [`descenderPieza()`](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/descenderPieza.puml).
- **Qué dice cada uno:** La tabla plantea una relación de uno a muchos entre Juego y «Pieza (activas)». Los casos de uso, en cambio, trabajan con una pieza activa que se sustituye por otra.
- **Qué corregiría y por qué:** Precisaría en el modelo que solo hay una pieza activa a la vez. Puede haber muchas piezas en el tablero, pero no todas siguen bajo el control del jugador.

### 4. La misma acción aparece con nombres distintos

- **Dónde:** [Tabla de casos de uso](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#casos-de-uso) y [detalle de `iniciarPartida()`](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/iniciarPartida.puml).
- **Qué dice cada uno:** La tabla define `alternarVelocidadCaida()`, pero al iniciar la partida se menciona `acelerarCaida()` entre las acciones disponibles.
- **Qué corregiría y por qué:** Utilizaría `alternarVelocidadCaida()` en los dos sitios. Así se mantiene el mismo nombre y queda reflejado que se puede acelerar y volver a la velocidad normal.

### 5. La tabla de relaciones y el modelo final no recogen lo mismo

- **Dónde:** [Modelo del dominio, apartados 7 y 8](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#7-refinamiento-de-relaciones) y [diagrama UML final](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/ModeloDominio.puml).
- **Qué dice cada uno:** La tabla desarrolla seis relaciones, mientras que el diagrama añade otras como Juego–Turno, Pieza–Celda, Turno–Pieza y Turno–Movimiento.
- **Qué corregiría y por qué:** Completaría la tabla con esas relaciones y sus cantidades. Si el diagrama es el modelo final, la explicación escrita debería recogerlo entero.

### 6. El diagrama del Tiempo incluye un cambio que corresponde al Jugador

- **Dónde:** [Contexto del actor Tiempo](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorTiempo.puml), [contexto del Jugador](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorJugador.puml) y [detalle del reinicio](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml).
- **Qué dice cada uno:** En el diagrama del Tiempo aparece una transición sin explicación de FINALIZADO a INACTIVO. En los otros dos documentos, ese cambio depende de que el jugador solicite y confirme un reinicio.
- **Qué corregiría y por qué:** Quitaría esa transición del contexto del Tiempo o indicaría expresamente que la provoca el Jugador. El tiempo hace bajar las piezas, no decide reiniciar una partida.

### 7. Se elimina «Partida» como concepto, pero se sigue utilizando

- **Dónde:** [Modelo del dominio, apartado 3](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#3-primera-depuración-consolidación-de-sinónimos) y [casos de uso](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#casos-de-uso).
- **Qué dice cada uno:** El modelo unifica Juego y Partida bajo «Juego». Sin embargo, los requisitos mantienen nombres como `iniciarPartida()` y `reiniciarPartida()`.
- **Qué corregiría y por qué:** Distinguiría los dos conceptos: Juego sería el programa o sus reglas y Partida, cada intento del jugador. Así se puede mantener el vocabulario de los casos de uso sin contradecir la decisión del modelo.

## 2. Decisiones discutibles

### 1. Dar dos composiciones a una misma celda

- **Dónde:** [Relaciones del modelo del dominio](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#7-refinamiento-de-relaciones).
- **Qué se decidió:** Tanto Tablero–Celda como Línea–Celda se representan mediante composición.
- **Qué propongo y por qué:** Dejaría la composición entre Tablero y Celda y usaría una asociación entre Línea y Celda. La celda forma parte del tablero y puede pertenecer a una línea, pero no debería depender por composición de dos elementos distintos.

### 2. Incluir Turno como entidad

- **Dónde:** [Modelo del dominio](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#8-modelo-del-dominio-resultante).
- **Qué se decidió:** Se incluye Turno y se relaciona con Pieza y Movimiento.
- **Qué propongo y por qué:** Lo eliminaría si no se necesita representar nada más que la caída de cada pieza. Tetris funciona de manera continua y el jugador puede mover una pieza varias veces mientras cae; llamarlo «turno» puede complicar la explicación.

### 3. Considerar el Tiempo como actor

- **Dónde:** [Tabla de actores](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#actores).
- **Qué se decidió:** El Tiempo es un actor que solicita el descenso automático de las piezas.
- **Qué propongo y por qué:** Lo trataría como un mecanismo interno del juego. Un actor representa algo externo que interactúa con el sistema; aquí el propio sistema puede controlar cada cuánto baja la pieza.

### 4. Volver a INACTIVO después de reiniciar

- **Dónde:** [Detalle de `reiniciarPartida()`](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml).
- **Qué se decidió:** Tras confirmar el reinicio, se borra la partida y se vuelve al estado INACTIVO.
- **Qué propongo y por qué:** Haría que empezara directamente una nueva partida. Si alguien pulsa «reiniciar», normalmente espera volver a jugar desde cero sin tener que iniciar otra vez.

### 5. No concretar el sentido de giro

- **Dónde:** [Detalle de `rotarPieza()`](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/rotarPieza.puml).
- **Qué se decidió:** Se indica que el jugador solicita rotar la pieza, pero no se especifica hacia qué lado.
- **Qué propongo y por qué:** Añadiría el sentido de rotación como dato de la solicitud. Girar a izquierda o derecha puede producir posiciones distintas y es importante dejar claro qué se comprueba.

### 6. Considerar suficiente una sola imagen del prototipo

- **Dónde:** [Apartado «Prototipar casos de uso»](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#prototipar-casos-de-uso).
- **Qué se decidió:** Se afirma que una imagen de la partida en marcha basta para explicar todos los casos de uso.
- **Qué propongo y por qué:** Añadiría al menos una representación del reinicio con su confirmación. Una captura fija ayuda a entender el tablero, pero no muestra una cancelación ni explica los cambios entre estados.

### 7. Tratar el arranque del programa como un objetivo de juego

- **Dónde:** [Tabla de casos de uso](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#casos-de-uso) y [detalle de `arrancarSistema()`](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/arrancarSistema.puml).
- **Qué se decidió:** Se incluye `arrancarSistema()` entre los casos de uso del Jugador.
- **Qué propongo y por qué:** Lo separaría de las acciones que persigue el jugador dentro del Tetris. Abrir el programa es necesario, pero su objetivo principal es jugar, mover piezas y gestionar la partida, no arrancar el sistema en sí.
