# Decisiones discutibles en pyTetris (dominio y requisitos)

Cada decisión indica **qué se decidió**, **qué propongo** y **el argumento**. Son decisiones coherentes con el resto de los artefactos, pero que considero mal tomadas. Las incoherencias entre artefactos están en `README_incoherencias.md`.

**Nomenclatura:** *Dominio §n* = sección n de `modeloDelDominio.md`; *Req* = `ProcesoRequisitos.md`.

**Límites:** D6 está marcada con ⚠ porque depende de `images/tetris.png`, que no se ha podido revisar y debe verificarse.

---

## D1. "Turno" como concepto del dominio
- **Qué se decidió:** Turno es una entidad, definida como "Ciclo de juego" (Dominio §2, §4).
- **Qué propongo:** eliminarlo del dominio.
- **Argumento:** Tetris es de tiempo real y nadie habla de "turnos". Solo aparece en una "observación" de §1. "Ciclo de juego" es el *game loop*, un término técnico, lo que contradice el objetivo de "sin contaminación tecnológica". Ninguno de los siete casos de uso ni de sus detalles lo menciona, ni siquiera en `descenderPieza`, donde empieza uno nuevo al generar la pieza.

## D2. Descartar "Partida" porque sería "para multijugador"
- **Qué se decidió:** "Juego" absorbe a "Partida" (Dominio §3).
- **Qué propongo:** "Partida" para la sesión y "Juego" para Tetris como tal.
- **Argumento:** en español "partida" es una sesión de juego, también en solitario. Los estados Inactivo, EnCurso y Finalizado, y verbos como reiniciar, describen una sesión, no el juego. Fusionarlos mezcla las reglas con una ejecución concreta.

## D3. Casos de uso nombrados como operaciones y `arrancarSistema()` como derivado del dominio
- **Qué se decidió:** los casos de uso son métodos (`moverPieza()`, `descenderPieza()`), y Req dice que "del modelo del dominio surge" toda la lista.
- **Qué propongo:** nombrarlos por el objetivo del actor ("Jugar una partida", "Terminar partida"). Justificar `arrancarSistema()` aparte, porque no sale del dominio.
- **Argumento:** un caso de uso describe un objetivo con valor para el actor, no una operación interna. Los nombres con paréntesis adelantan decisiones de diseño, justo lo que la página dice evitar.

## D4. "Tiempo" como actor
- **Qué se decidió:** actor propio que solicita el descenso "según intervalo configurado" (Req, tabla de actores).
- **Qué propongo:** justificarlo mejor, o modelar el temporizador como mecanismo interno cuyo beneficiario es el Jugador.
- **Argumento:** un actor tiene objetivos propios y Tiempo no tiene ninguno. Matiz: "configurado" sí tiene un correlato (`alternarVelocidadCaida` cambia ese intervalo), pero la tabla de actores no lo enlaza. Contraargumento: UML admite actores temporales, y el diagrama de Tiempo sale limpio.

## D5. `reiniciarPartida()` lleva a Inactivo
- **Qué se decidió:** reiniciar lleva a INACTIVO, desde EN_CURSO y desde FINALIZADO (diagrama del Jugador y detalle de `reiniciarPartida`).
- **Qué propongo:** que lleve directamente a EN_CURSO, o renombrarlo (`abandonarPartida()`).
- **Argumento:** "reiniciar" significa empezar de nuevo. Tras Game Over, volver a jugar exige `reiniciarPartida()`, confirmar e `iniciarPartida()`: tres interacciones, y el nombre no cumple su semántica.

## D6. Un solo prototipo para siete casos de uso ⚠
- **Qué se decidió:** una imagen en EN_CURSO y la afirmación "agregar más sería redundante" (Req, "Prototipar casos de uso").
- **Qué propongo:** al menos una pantalla por estado (Inactivo y Finalizado).
- **Argumento:** hay tres estados y la imagen cubre uno. La pantalla inicial, el Game Over y la confirmación de reinicio son interfaces propias que dan sentido a `arrancarSistema`, `descenderPieza` y `reiniciarPartida`. Una imagen estática tampoco muestra que la pieza baja ni que cambia la velocidad.

## D7. Caída acelerada como interruptor
- **Qué se decidió:** `alternarVelocidadCaida()` activa y desactiva un modo (detalle de `alternarVelocidadCaida`).
- **Qué propongo:** acelerar mientras se mantenga la acción y volver a normal al soltar.
- **Argumento:** el interruptor obliga al Jugador a recordar el modo y, según los detalles, nada dice que la pieza siguiente no caiga también acelerada. Mantener pulsado no requiere estado persistente y evita introducir una variable de velocidad en el modelo. Contraargumento: sin eventos de soltar (por ejemplo, un prototipo por consola), el interruptor es más sencillo.

## D8. Confirmación de reinicio también desde FINALIZADO
- **Qué se decidió:** `reiniciarPartida()` pide confirmación en EN_CURSO y en FINALIZADO (detalle de `reiniciarPartida`).
- **Qué propongo:** confirmar solo en EN_CURSO.
- **Argumento:** la confirmación evita perder una partida en curso. Con Game Over no hay nada que perder, y el paso añade fricción al caso más frecuente. Combinado con D5, el uso más habitual tiene el camino más largo. Contraargumento: un único flujo simplifica el caso de uso.

## D9. La pieza se fija en el mismo descenso en que toca fondo
- **Qué se decidió:** en `descenderPieza`, si la pieza queda en el fondo o sobre piezas fijadas, pasa a "FijandoPieza" sin esperar otro tick.
- **Qué propongo:** fijar cuando el siguiente descenso no sea posible, no cuando se llega a esa posición.
- **Argumento:** así `moverPieza()` y `rotarPieza()` nunca actúan con la pieza apoyada y se pierde el último desplazamiento. En el Tetris clásico la pieza puede deslizarse mientras reposa hasta el siguiente tick.

## D10. El flujo de fijación siempre elimina líneas y actualiza la puntuación
- **Qué se decidió:** en `descenderPieza`, FijandoPieza → EliminandoLineas → GenerandoNuevaPieza es un camino único, con "tablero sin líneas completas" y "puntuación actualizada" siempre.
- **Qué propongo:** una decisión explícita "¿hay líneas completas?".
- **Argumento:** en la mayoría de las fijaciones no se completa ninguna línea, y el diagrama afirma lo contrario. No se distingue el caso en que se puntúa.

## D11. `descenderPieza` concentra casi todo el comportamiento del dominio
- **Qué se decidió:** un único caso de uso, de Tiempo, que desciende, fija, elimina líneas, puntúa, genera la pieza nueva y termina la partida (detalle de `descenderPieza`).
- **Qué propongo:** separar "eliminar líneas y puntuar" y "terminar partida" en casos de uso propios (incluidos o extendidos), o al menos justificar el nivel de agrupación.
- **Argumento:** las reglas que definen Tetris (Línea: "Completarse, eliminarse, contar puntos", Dominio §4) quedan enterradas en un caso de uso del actor Tiempo. La puntuación, que el dominio trató como atributo de Juego, no tiene ninguna regla escrita (cuántos puntos) ni caso de uso propio.