# Hallazgos

[README](../README.md) · [Plan](plan.md)

Reviso el [modelo del dominio](https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/docs/modeloDelDominio.md) (MdD) y los [requisitos](https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/docs/ProcesoRequisitos.md) (Req) de pyTetris en el commit `92c950a`.

# Incoherencias

## Dentro del modelo del dominio

### MD-1. Los atributos del texto no están en el diagrama

**Dónde y qué dicen:** el §6 del MdD decide que la Puntuación es una *"propiedad de Juego"*, y la tabla de descartados dice que la Velocidad es un *"atributo de Juego"*. En cambio, el diagrama final no tiene ningún atributo.

**Qué corrijo y por qué:** el diagrama, añadiendo `puntuación` y `velocidad` a `Juego`. El texto ya lo decidió y los requisitos muestran la puntuación. Si no está en el diagrama, la puntuación desaparece del modelo.

### MD-2. Las multiplicidades del texto no están en el diagrama

**Dónde y qué dicen:** la tabla del §7 da multiplicidades (1:1, 1:\*, \*:1), y la Versión 2 dice *"se refinan multiplicidades"*. El diagrama no tiene ninguna. Además, el diagrama tiene cuatro relaciones que la tabla no justifica: Juego–Turno, Pieza–Celda, Turno–Pieza y Turno–Movimiento.

**Qué corrijo y por qué:** el diagrama, porque es el resultado final y debe recoger lo que el texto ya decidió. Sin multiplicidades no se sabe, por ejemplo, si hay una pieza activa o varias.

### MD-3. Nivel existe y no existe a la vez

**Dónde y qué dicen:** el §6 dice que *"Puntuación y Nivel son propiedades de Juego"*. La tabla de simplificaciones dice *"Sin niveles de dificultad"*.

**Qué corrijo y por qué:** el §6, quitando Nivel. La simplificación es la decisión final, y ningún caso de uso ni pantalla usa niveles.

### MD-4. La Línea se define como "completa", pero se usa como cualquier fila

**Dónde y qué dicen:** el §2 define *"Línea: fila horizontal completa"*. Pero el §4 dice que la línea puede *"completarse"*, y el diagrama dice que el tablero *"se organiza en"* líneas.

**Qué corrijo y por qué:** la definición del §2: *"fila horizontal del tablero; está completa cuando todas sus celdas están ocupadas"*. Una línea que ya es completa no puede completarse, y al empezar la partida no habría ninguna línea en el tablero.

### MD-5. Una pieza en el texto, varias en el diagrama

**Dónde y qué dicen:** el §1 dice *"mueves la pieza actual"* y el §5 *"cada turno involucra una pieza"*. La tabla del §7 y el diagrama dicen *"piezas activas"*, con multiplicidad 1:\*. Los requisitos hablan siempre de *"la pieza activa"*, en singular.

**Qué corrijo y por qué:** el §7 y el diagrama, poniendo `0..1` pieza activa. Todos los demás artefactos coinciden en una sola, y es la regla del juego. Es 0 cuando la partida aún no ha empezado o entre fijar una pieza y que salga la siguiente.

## Entre el modelo del dominio y los requisitos

### MR-1. Juego en el MdD, Partida en los requisitos

**Dónde y qué dicen:** el §3 del MdD unifica *"Juego vs Partida → Juego"* porque *"Partida es más específico para juegos multijugador"*. Los requisitos usan *partida* en todas partes: `iniciarPartida()`, `reiniciarPartida()`, *"partida iniciada"*, *"continuar partida"*.

**Qué corrijo y por qué:** el MdD, cambiando `Juego` por `Partida`. El vocabulario tiene que ser el mismo en todo el proyecto, y los requisitos usan el término correcto. Una partida es cada sesión de juego, haya uno o varios jugadores, y lo que se inicia y se reinicia es una partida, no "el juego".

### MR-2. Turnos en el MdD, tiempo real en los requisitos

**Dónde y qué dicen:** el MdD dice *"cada turno mueves la pieza actual"* y su diagrama tiene `Juego *-- Turno`. En los requisitos, las piezas bajan solas porque lo dispara el actor **Tiempo**, y el jugador puede mover y girar las veces que quiera, sin turnos. Ningún caso de uso, estado ni pantalla menciona turnos.

**Qué corrijo y por qué:** el MdD, quitando `Turno`. El Tetris se juega en tiempo real, no por turnos, y los requisitos lo modelan bien con el actor Tiempo.

### MR-3. "Movimiento" no significa lo mismo

**Dónde y qué dicen:** el §3 del MdD junta rotación, caída y desplazamiento en un solo concepto, *Movimiento*. En los requisitos, `moverPieza()` es solo *"izquierda o derecha"*, y rotar y caer son casos de uso distintos.

**Qué corrijo y por qué:** el MdD, separando los tres conceptos. Así es como habla el jugador ("mover", "girar", "bajar"), y ahora la misma palabra significa cosas distintas en cada documento.

### MR-4. Los requisitos hablan de "piezas fijadas", y el MdD no las tiene

**Dónde y qué dicen:** el detalle de `descenderPieza()` dice *"sobre piezas fijadas"* y *"tablero actualizado con pieza fijada"*. En el MdD, lo que queda en el tablero son celdas ocupadas, y el concepto de pieza fijada no existe.

**Qué corrijo y por qué:** el detalle, cambiando "piezas fijadas" por "celdas ocupadas". Al borrar una línea, una pieza puede quedar partida, así que ya no es una pieza. El MdD lo resuelve bien con las celdas.

### MR-5. El MdD dice que el juego se pausa, pero no hay forma de pausar

**Dónde y qué dicen:** el §4 del MdD dice que el Juego puede *"iniciar, terminar, pausar"*. En los requisitos no existe `pausarPartida()` ni un estado `Pausado`, y el MdD tampoco incluye la pausa entre sus simplificaciones.

**Qué corrijo y por qué:** el MdD, añadiendo *"sin pausa"* a las simplificaciones. Es el cambio más pequeño y deja claro que es una decisión, no un olvido.

### MR-6. "Del modelo del dominio surge", pero no surge

**Dónde y qué dicen:** los requisitos dicen *"del modelo del dominio surge"* su tabla de casos de uso. Pero `arrancarSistema()` y `alternarVelocidadCaida()` usan conceptos (sistema, velocidad) que no están en el diagrama del MdD. Y al revés: `Turno`, `Movimiento`, `Celda` y `Linea` no generan ningún caso de uso.

**Qué corrijo y por qué:** el MdD, aplicando MD-1, MR-1 y MR-2. Así la trazabilidad que promete el proyecto pasa a ser cierta.

### MR-7. El MdD elige el español y el prototipo usa inglés

**Dónde y qué dicen:** el §3 del MdD elige "Pieza" porque *"es más natural en español"*, y el concepto es *Puntuación*. El prototipo muestra `Score: 0` y el detalle de `descenderPieza()` habla de *"Game Over"*.

**Qué corrijo y por qué:** el prototipo y el detalle, poniendo *Puntuación* y *fin de partida*. Son lo primero que ve el usuario, y deben usar el vocabulario que eligió el dominio.

## Dentro de los requisitos y con el prototipo

### RQ-1. `iniciarPartida()` ofrece un caso de uso que no existe

**Dónde y qué dicen:** al terminar `iniciarPartida()`, el sistema permite solicitar *"acelerarCaida()"*. La tabla de casos de uso, el diagrama de casos de uso, el contexto del Jugador y su propio detalle lo llaman `alternarVelocidadCaida()`.

**Qué corrijo y por qué:** el detalle de `iniciarPartida()`. Cuatro artefactos frente a uno, y además no significan lo mismo: *acelerar* va en un solo sentido, mientras que *alternar* también devuelve la velocidad normal.

### RQ-2. Falta el reinicio en el diagrama de estados del sistema

**Dónde y qué dicen:** el contexto del Jugador tiene `EnCurso → Inactivo` con `reiniciarPartida()`, y el detalle de reiniciar también permite hacerlo durante la partida. El diagrama de estados del sistema y el de contexto del Tiempo no tienen esa transición.

**Qué corrijo y por qué:** esos dos diagramas, añadiendo la transición. El diagrama de estados del sistema presenta *"el modelo como un todo"*, así que tiene que tener todas las transiciones.

### RQ-3. El sistema puede terminar, pero nadie lo cierra

**Dónde y qué dicen:** el diagrama de estados del sistema tiene `Finalizado → [*]`, el fin del sistema. Ningún diagrama de contexto ni caso de uso provoca esa transición. Existe `arrancarSistema()`, pero no hay un caso de uso para salir.

**Qué corrijo y por qué:** los requisitos, añadiendo `salirDelSistema()` para el Jugador. Si el jugador arranca el sistema, también tiene que poder cerrarlo.

### RQ-4. El prototipo gira en dos sentidos y el caso de uso en ninguno

**Dónde y qué dicen:** el prototipo tiene dos comandos de giro (`7=rotar↺`, `9=rotar↻`). El detalle de `rotarPieza()` solo dice *"solicita rotar pieza"*. En cambio, `moverPieza()` sí indica la dirección: *"izquierda o derecha"*.

**Qué corrijo y por qué:** el detalle de `rotarPieza()`, añadiendo *"(horario o antihorario)"*. Es una elección del jugador, y `moverPieza()` demuestra que el criterio del documento es indicarla.

### RQ-5. El prototipo no permite pedir dos casos de uso

**Dónde y qué dicen:** el prototipo solo tiene los comandos 4, 6, 7 y 9. Pero durante la partida el jugador también puede pedir `alternarVelocidadCaida()` y `reiniciarPartida()`, según el contexto del Jugador y el detalle de iniciarPartida.

**Qué corrijo y por qué:** el prototipo, añadiendo un comando para cada uno. Si el usuario no puede pedirlos, el prototipo no sirve para validarlos.

### RQ-6. "Todo está en esa única imagen", pero faltan pantallas

**Dónde y qué dicen:** el texto del prototipo dice *"agregar más sería redundante"*. Pero los detalles describen una pantalla inicial (`arrancarSistema`), un mensaje de confirmación (`reiniciarPartida`) y un mensaje de fin de partida (`descenderPieza`), y la imagen solo muestra la partida en curso.

**Qué corrijo y por qué:** el prototipo, añadiendo esas tres pantallas. Lo que el usuario ve y elige en ellas no está "implícito": son decisiones de interacción que hay que validar.

### RQ-7. El intervalo es "configurado", pero nadie lo configura

**Dónde y qué dicen:** la tabla de actores dice que el Tiempo baja las piezas *"según intervalo configurado"*. Ningún caso de uso configura ese intervalo; solo se puede alternar entre velocidad normal y acelerada.

**Qué corrijo y por qué:** la tabla de actores: *"según la velocidad actual (normal o acelerada)"*. Tiene que describir lo que el sistema permite de verdad.

## Menores

- **ME-1.** La Versión 2 del MdD dice que *"se agregan TipoPieza y Turno"*, pero Turno ya estaba en la Versión 0 y en el §2. Corrijo la Versión 2, porque la historia del modelo tiene que ser fiel.
- **ME-2.** El §2 del MdD escribe *"Tetrimino"* y el §3 *"Tetrómino"*. Corrijo el §3, porque el vocabulario no puede tener dos grafías.
- **ME-3.** Los diagramas de estados usan `EnCurso` y los detalles `EN_CURSO` (igual con los otros estados). Corrijo los detalles para que coincidan con los diagramas, que son los que definen los estados.


# Decisiones discutibles

## Del modelo del dominio

### DD-1. Movimiento como clase

**Qué se decidió:** juntar rotación, caída y desplazamiento en la clase `Movimiento`, unida a `Turno`.

**Qué propongo:** quitarla del modelo. Lo que hay que guardar es la posición y la orientación de la pieza activa.

**Argumento:** un movimiento es una acción, no algo que se recuerde: nadie consulta "los movimientos de la partida". Las acciones son casos de uso, y de hecho los requisitos las convierten en `moverPieza()`, `rotarPieza()` y `descenderPieza()`.

### DD-2. Celda con dos dueños por composición

**Qué se decidió:** `Tablero *-- Celda` y `Linea *-- Celda`, las dos como composición.

**Qué propongo:** `Tablero ◆ Linea ◆ Celda`, sin la composición directa entre Tablero y Celda.

**Argumento:** en una composición, la parte pertenece a un solo todo y desaparece con él. Aquí cada celda tendría dos dueños. Además, al borrar una línea las celdas no se destruyen: se vacían y las de arriba bajan.

### DD-3. Juego–Pieza como agregación

**Qué se decidió:** agregación, porque las piezas *"pueden existir conceptualmente sin un juego"*.

**Qué propongo:** una asociación normal, `Juego 1 — 0..1 Pieza`, con el rol "pieza activa".

**Argumento:** una pieza que cae no existe fuera de una partida. Además, el propio MdD descarta *Bloque* porque *"no existe independientemente"*: con ese mismo criterio, la pieza tampoco existe sin la partida.

### DD-4. Flechas en el modelo del dominio

**Qué se decidió:** relaciones con flecha, como `Pieza --> TipoPieza`, `Pieza --> Celda` y `Turno --> Pieza`.

**Qué propongo:** líneas sin flecha.

**Argumento:** la flecha indica qué objeto conoce a cuál, y eso es una decisión de diseño. El MdD dice estar *"sin contaminación tecnológica"*, pero esa decisión la adelanta.

### DD-5. Excluir al Jugador porque "el sistema no lo necesita"

**Qué se decidió:** quitar `Jugador` porque *"el sistema no necesita modelar múltiples jugadores"*.

**Qué propongo:** mantener la exclusión, pero con un argumento del dominio: no hay ningún dato del jugador que guardar (ni nombre, ni récords).

**Argumento:** "el sistema no lo necesita" es un razonamiento de requisitos, y el MdD habla del mundo real. Además, que haya un solo jugador no lo hace desaparecer: es el actor principal de los requisitos.

### DD-6. Una validación que nadie ha hecho

**Qué se decidió:** responder las preguntas de validación del §9 con *"Sí"* y *"No"*, sin decir a quién se consultó, y concluir que *"no sobra"* ningún concepto.

**Qué propongo:** indicar con quién se validó y qué cambió en el modelo como resultado.

**Argumento:** los propios requisitos desmienten la conclusión: `Turno` y `Movimiento` no se usan en ningún caso de uso (MR-2 y DD-1), y la puntuación, que sí se usa, no está en el diagrama (MD-1).


## De los requisitos

### DD-7. Cambiar la velocidad con un interruptor

**Qué se decidió:** `alternarVelocidadCaida()`, que cambia entre velocidad normal y acelerada y deja la velocidad elegida activa hasta que se vuelva a cambiar.

**Qué propongo:** `bajarPieza()`: cada vez que el jugador lo pide, la pieza baja una fila, igual que `moverPieza()` la mueve una celda a los lados.

**Argumento:** en el Tetris, el jugador "baja la pieza" cuando quiere; no activa un modo. El interruptor deja dudas que ningún artefacto resuelve: ¿la siguiente pieza sale acelerada? ¿Con qué velocidad empieza la partida? Con `bajarPieza()` esas dudas no existen.

### DD-8. Reiniciar no reinicia

**Qué se decidió:** `reiniciarPartida()` pide confirmación desde `EnCurso` y desde `Finalizado`, y si se confirma lleva a `Inactivo`.

**Qué propongo:** llamarlo `abandonarPartida()` cuando se pide desde `EnCurso`, con confirmación. Desde `Finalizado`, permitir `iniciarPartida()` directamente, sin confirmación.

**Argumento:** "reiniciar" promete empezar de nuevo, pero el jugador acaba en la pantalla inicial y tiene que pedir otra vez `iniciarPartida()`. Además, después de perder no hay nada que perder: preguntar *"¿seguro que quieres reiniciar?"* no protege de nada.