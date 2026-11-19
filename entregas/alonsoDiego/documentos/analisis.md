# Revisión del ejemplo de Tetris

[Volver al README](../README.md) · [Ver la propuesta](propuesta.md)

Revisión del modelo del dominio, la disciplina de requisitos, sus diagramas UML y el prototipo de `pyTetris`.

[Incoherencias](#incoherencias) · [Decisiones discutibles](#decisiones-discutibles)

## Incoherencias

### Se reutiliza el término «Partida» después de descartarlo en el dominio

En el apartado 3 de `docs/modeloDelDominio.md` (*Consolidación de sinónimos*) se descarta explícitamente el concepto **Partida** (*"Juego vs Partida → Juego"*) para fijar `Juego` como único término válido. Sin embargo, `docs/ProcesoRequisitos.md`, `DiagramaCasosUso.puml` y los diagramas de detalle nombran los casos de uso como `iniciarPartida()` y `reiniciarPartida()`, además de usar notas como *"partida iniciada"* o *"continuar partida"*.

Corregiría los artefactos de requisitos para renombrarlos a `iniciarJuego()` y `reiniciarJuego()`, ajustando también sus notas y etiquetas. El modelo del dominio establece el vocabulario común del proyecto; reutilizar un sinónimo previamente prohibido rompe la trazabilidad terminológica entre fases.

### Los diagramas de contexto mezclan transiciones de otros actores

En `DiagramaDeContexto001-ActorTiempo.puml` aparecen las transiciones `[*] -> Inactivo`, `Inactivo --> EnCurso` y `Finalizado --> Inactivo`, aunque el actor `Tiempo` no dispara ninguno de esos eventos en `DiagramaCasosUso.puml`. De forma inversa, `DiagramaDeContexto001-ActorJugador.puml` incluye la transición `EnCurso --> Finalizado` sin etiqueta, cuando en `descenderPieza.puml` el fin de partida lo provoca únicamente el `Tiempo` al no haber espacio para una pieza nueva.

Eliminaría de cada diagrama de contexto por actor las transiciones que no son disparadas por ese actor. El propósito de separar los diagramas de contexto por actor es aislar qué cambios de estado provoca cada uno; incluir flechas ajenas confunde las responsabilidades del sistema.

### Los nombres de estado cambian de formato entre contexto y detalle

En `DiagramaDeContexto000.puml`, `DiagramaDeContexto001-ActorJugador.puml` y `DiagramaDeContexto001-ActorTiempo.puml`, los estados del sistema se escriben en *PascalCase* (`Inactivo`, `EnCurso`, `Finalizado`). En cambio, todos los diagramas de `modelosUML/detalleCasosUso/*.puml` nombran esos mismos estados en mayúsculas con guion bajo (`INACTIVO`, `EN_CURSO`, `FINALIZADO`).

Unificaría la nomenclatura en los diagramas de contexto usando `INACTIVO`, `EN_CURSO` y `FINALIZADO`. Mantener dos identificadores distintos para los mismos estados obliga a traducir entre vistas y rompe la consistencia sintáctica de la especificación.

### Quedan restos de `acelerarCaida()` tras renombrarlo a `alternarVelocidadCaida()`

Mientras que `docs/ProcesoRequisitos.md` y `DiagramaCasosUso.puml` definen el caso de uso `alternarVelocidadCaida()`, el detalle `modelosUML/detalleCasosUso/iniciarPartida.puml` sigue indicando al finalizar que el sistema permite solicitar `acelerarCaida()`. Además, en `images/modelosUML/detalleCasosUso/` permanece el archivo `acelerarCaida.svg` sin código `.puml` asociado.

Sustituiría `acelerarCaida()` por `alternarVelocidadCaida()` dentro de `iniciarPartida.puml` (y su `.svg`) y eliminaría el archivo huérfano `acelerarCaida.svg`. Se modificó el requisito a nivel general sin propagar el cambio a todos los diagramas de detalle ni limpiar los artefactos obsoletos.

### `arrancarSistema()` habilita `iniciarPartida()` antes de llegar a `INACTIVO`

En el interior de `modelosUML/detalleCasosUso/arrancarSistema.puml` se especifica que el sistema presenta la pantalla inicial y *"permite solicitar: iniciarPartida()"* antes de completar la transición hacia `INACTIVO`. Sin embargo, `iniciarPartida.puml` y `DiagramaDeContexto001-ActorJugador.puml` establecen que `iniciarPartida()` se solicita desde el estado `INACTIVO`.

Quitaría la lista de casos de uso permitidos del interior de `arrancarSistema.puml`, dejando que el caso de uso desemboque simplemente en el estado `INACTIVO`. Un caso de uso modela la transición entre estados; las operaciones disponibles para el usuario pertenecen al estado destino una vez finalizado el arranque.

### El tablero del prototipo muestra piezas con geometría imposible

En `docs/modeloDelDominio.md` (apartados 1 y 6) se define que el juego utiliza los 7 tetrominós clásicos de 4 bloques conectados (`I`, `O`, `T`, `L`, etc.). Sin embargo, en la parte inferior del tablero de `images/tetris.png` aparece un bloque compacto de `3×4` celdas `I` (12 bloques contiguos) y otro de `4×2` celdas `L` (8 bloques contiguos), formas imposibles de construir con piezas de 4 celdas.

Redibujaría el estado del tablero en `images/tetris.png` utilizando combinaciones válidas de tetrominós de 4 bloques. Un prototipo debe respetar las reglas geométricas del dominio; mostrar estados inalcanzables induce a error sobre cómo se fijan las piezas y se completan líneas.

### El diagrama `ModeloDominio.puml` y la tabla de relaciones no coinciden

En `docs/modeloDelDominio.md` (apartado 7, *Refinamiento de relaciones*) se listan 6 relaciones con sus multiplicidades (`1:1`, `1:*`, `*:1`). No obstante, `modelosUML/ModeloDominio.puml` omite todas las multiplicidades e incluye 4 relaciones (`Juego *-- Turno`, `Pieza --> Celda`, `Turno --> Pieza` y `Turno o-- Movimiento`) que no aparecen documentadas en esa tabla.

Completaría la tabla del apartado 7 con las relaciones faltantes y añadiría en `ModeloDominio.puml` las multiplicidades y atributos descritos en el texto. El diagrama y la tabla de refinamiento deben describir exactamente la misma estructura sin omisiones mutuas.

## Decisiones discutibles

### Se modela `Turno` en un juego en tiempo real

En `ModeloDominio.puml` y `docs/modeloDelDominio.md` se decidió incluir la entidad `Turno` compuesta dentro de `Juego` (`Juego *-- Turno`) para agrupar los movimientos (`Turno o-- Movimiento`).

Eliminaría la clase `Turno` y asociaría `Movimiento` directamente con `Pieza` (o modelaría un ciclo de caída temporal si se busca reflejar el avance automático). El Tetris es un juego continuo en tiempo real donde la gravedad actúa periódicamente mientras el jugador realiza movimientos asíncronos; además, ningún jugador habla de «turnos» al explicar el Tetris, incumpliendo el propio criterio de validación del documento.

### Se usa agregación en lugar de composición entre `Juego` y `Pieza`

En `ModeloDominio.puml` y en la tabla del apartado 7 de `docs/modeloDelDominio.md` se decidió relacionar `Juego` y `Pieza` mediante agregación (`Juego o-- Pieza`) argumentando que las piezas pueden existir conceptualmente sin un juego.

Cambiaría esa relación a composición estricta (`Juego *-- Pieza`), manteniendo la asociación `Pieza --> TipoPieza`. Los 7 arquetipos abstractos ya están representados en `TipoPieza`, que sí es independiente de la partida; en cambio, una `Pieza` activa con posición y rotación en un tablero concreto carece de existencia fuera de su `Juego` y desaparece al reiniciarlo.

### Se compone `Celda` simultáneamente desde `Tablero` y desde `Linea`

En `ModeloDominio.puml` se decidió aplicar composición tanto de `Tablero` hacia `Celda` (`Tablero *-- Celda`) como de `Linea` hacia `Celda` (`Linea *-- Celda`).

Mantendría la composición `Tablero *-- Celda` y cambiaría la relación de `Linea` a asociación o agregación (`Linea o-- Celda`), o bien encadenaría `Tablero *-- Linea` y `Linea *-- Celda`. En UML la composición implica propiedad exclusiva del ciclo de vida, por lo que una misma instancia de `Celda` no puede pertenecer por composición a dos contenedores distintos a la vez.

### Se excluye `Jugador` del modelo del dominio

En el apartado 6 de `docs/modeloDelDominio.md` se decidió excluir la entidad `Jugador` por tratarse de una versión para un solo jugador donde se considera implícito.

Incluiría `Jugador` asociado a `Juego` (`Jugador 1 -- * Juego`). Aunque sea *single-player*, incluir al jugador aporta trazabilidad directa con la disciplina de requisitos (donde `Jugador` es el actor principal) y deja el modelo preparado para evoluciones básicas del dominio como el registro histórico de puntuaciones máximas.

### Se descarta la pieza siguiente (`Preview`) como detalle de interfaz

En la tabla de conceptos descartados de `docs/modeloDelDominio.md` se decidió excluir la pieza siguiente alegando que es únicamente un detalle de interfaz gráfica.

Modelaría en `Juego` la distinción entre la pieza activa y la siguiente pieza en cola (o una entidad `ColaPiezas`). Aunque su visualización sea presentación, el hecho de que la próxima pieza ya esté generada y sea conocida antes de fijar la actual es una regla de negocio esencial del Tetris que condiciona las decisiones estratégicas de cada jugada.

### Se diseña la aceleración como un interruptor binario

En `docs/ProcesoRequisitos.md` y `alternarVelocidadCaida.puml` se decidió modelar el descenso rápido mediante `alternarVelocidadCaida()`, un conmutador que mantiene fija la velocidad acelerada hasta que el jugador vuelve a solicitar el cambio.

Lo sustituiría por una caída acelerada puntual (`acelerarCaida()` o *soft drop*) o caída instantánea hasta el fondo (`soltarPieza()` o *hard drop*). En el dominio del Tetris la aceleración se utiliza de forma momentánea para acercar la pieza al fondo y recuperar al instante el control fino antes de fijarla; obligar a pulsar de nuevo para desactivar el modo rápido penaliza la jugabilidad.

### Se pide confirmación de reinicio en `FINALIZADO` y sin pausar el juego en `EN_CURSO`

En `reiniciarPartida.puml` se decidió exigir confirmación al jugador tanto desde `EN_CURSO` como desde `FINALIZADO`, sin detener el avance del juego mientras se espera su respuesta.

Permitiría reiniciar directamente sin confirmación desde `FINALIZADO`, e introduciría un estado `PAUSADO` cuando se solicite confirmación desde `EN_CURSO`. Cuando la partida ya ha terminado no hay progreso que proteger con una confirmación; por el contrario, en `EN_CURSO`, si no se pausa el sistema durante el diálogo, el actor `Tiempo` sigue ejecutando `descenderPieza()` y la partida puede perderse sola mientras el jugador decide.