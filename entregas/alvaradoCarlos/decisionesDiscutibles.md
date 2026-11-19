# Decisiones Discutibles en pyTetris

De acuerdo con las instrucciones de [reto002.md](https://github.com/puntoReflex/26-27-idsw1/blob/main/evaluaciones/retos/reto002.md), una **decisión discutible** se define como:

> *"Algo coherente con el resto, pero que crean que está mal decidido. Para cada una: qué se decidió, qué proponen y el argumento. 'Es que yo lo habría hecho distinto' sin argumento no cuenta como decisión discutible."*

---

## Decisión 1: Modelar la entidad `Turno` en un juego en tiempo real

### 1. Qué se decidió
En [`docs/modeloDelDominio.md`](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md) (Secciones 2, 4, 5 y diagrama de la Sección 8), se introduce `Turno` como una clase conceptual central del modelo del dominio, ligada estructuralmente a `Juego`, `Pieza` y `Movimiento`:

```plantuml
Juego *-- Turno : "se desarrolla en turnos"
Turno --> Pieza : "involucra"
Turno o-- Movimiento : "permite"
```
Se justifica en el texto bajo la premisa de que *"cada turno involucra una pieza"* y *"un turno permite movimientos"*.

### 2. Qué propongo
**Eliminar por completo la entidad `Turno` del modelo del dominio.** La relación entre `Juego`, `Tablero` y `Pieza` debe modelar directamente que el juego gestiona la pieza activa que desciende sobre el tablero, sin intermediación de turnos.

### 3. Argumento técnico
El concepto de "Turno" es propio y natural de juegos de lógica discreta o por turnos secuenciales (como el ajedrez, las damas, el parchís o juegos de cartas), donde los jugadores alternan acciones en intervalos cerrados y el sistema espera pasivamente la decisión del participante.

El Tetris es un videojuego arcade continuo en **tiempo real asíncrono**:
* La gravedad desciende la pieza a intervalos temporales automáticos independientes de las órdenes del jugador.
* El jugador puede enviar ráfagas continuas de traslaciones y rotaciones o no pulsar ninguna tecla mientras la pieza cae.

Forzar una entidad `Turno` impone un paradigma que no describe la realidad operativa del juego (violando la directriz de [`00004-MdD.md`](https://github.com/mmasias/IdSw1/blob/main/temario/contenidos/00004-MdD.md) de *"representar la realidad existente tal como se vive"*). En la práctica, dicha clase carece de atributos propios, de responsabilidades claras y de un ciclo de vida definido (¿cuándo empieza el turno?, ¿al spawnear la pieza?, ¿cada segundo de caída?, ¿con cada pulsación de tecla?), introduciendo ruido y acoplamiento artificial en el diagrama.

> En mi [Modelo del Dominio propuesto](modelosUML/ModeloDominioPropuesto.puml) ([ver SVG](images/ModeloDominioPropuesto.svg)) se prescinde de `Turno`, reflejando una estructura limpia y orientada a la dinámica real del juego.

---

## Decisión 2: Ausencia del Caso de Uso de "Pausar partida"

### 1. Qué se decidió
En [`docs/ProcesoRequisitos.md`](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md), los requisitos funcionales omiten cualquier mecanismo para pausar el juego. En la máquina de estados global (`DiagramaDeContexto000.puml`) y en el diagrama de contexto del actor (`DiagramaDeContexto001-ActorJugador.puml`), el sistema únicamente conoce los estados `Inactivo`, `EnCurso` y `Finalizado`. Una vez que el jugador inicia la partida, está obligado a jugar ininterrumpidamente hasta que pierda o reinicie.

### 2. Qué propongo
Incorporar a la disciplina de requisitos el caso de uso **`Pausar partida`** (con soporte para reanudar, finalizar o reiniciar la partida desde dicho estado), introduciendo formalmente el estado **`Pausado`** en la máquina de estados del juego.

### 3. Argumento técnico
Esta decisión es discutible por dos razones fundamentales:
1. En [`docs/modeloDelDominio.md`](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#4-análisis-de-responsabilidades-verbos) (Sección 4, *"Análisis de responsabilidades: verbos"*), se identificaron textualmente para la entidad `Juego`:
   > *"Juego: Iniciar, terminar, **pausar**, mantener puntuación"*
   
   Haber reconocido la pausa como una responsabilidad intrínseca del juego en el dominio para luego ignorarla por completo al transformarla en casos de uso representa una ruptura de trazabilidad.
2. Según los requisitos de la asignatura ([`00006-CdU.eAyCdU.md`](https://github.com/mmasias/IdSw1/blob/main/temario/contenidos/00006-CdU.eAyCdU.md) y [`l'Rubrica.CdU.md`](https://github.com/mmasias/26-27-IdSw1-SdR/blob/main/evaluaciones/SdR/l'Rubrica.CdU.md)), los casos de uso deben capturar *"los objetivos y problemas reales del cliente/usuario al interactuar con el sistema"*. En cualquier aplicación interactiva en tiempo real donde el tiempo corre sin esperar al usuario, suspender temporalmente el bucle de juego ante contingencias externas (una llamada, una interrupción física) es un requisito funcional básico. Asimismo, encontrarse en pausa debe ofrecer opciones de salida al usuario: no solo retomar el juego (`reanudarPartida()`), sino también abandonar o reiniciar (`finalizarPartida()` / `reiniciarPartida()`) sin verse forzado a reactivar la caída de las piezas.

> Se formaliza en tres diagramas:
> * [Diagrama de Casos de Uso propuesto](modelosUML/DiagramaCasosUsoPropuesto.puml) ([ver SVG](images/DiagramaCasosUsoPropuesto.svg)), que incluye los casos de uso *"Pausar partida"* y *"Finalizar partida"*.
> * [Diagrama de Estados propuesto](modelosUML/DiagramaEstadosPropuesto.puml) ([ver SVG](images/DiagramaEstadosPropuesto.svg)), que añade el estado `Pausado` con las transiciones hacia `EnCurso`, `Finalizado` y `[*]`.
> * [Detalle del caso de uso `pausarPartida()`](modelosUML/detalleCasosUso/pausarPartida.puml) ([ver SVG](images/detalleCasosUso/pausarPartida.svg)), especificando el origen desde `EN_CURSO`, la detención del reloj y sus tres salidas operativas: reanudar hacia `EN_CURSO`, finalizar hacia `FINALIZADO` y reiniciar hacia `INACTIVO`.

---

## Decisión 3: Modelar `Movimiento` como clase conceptual en el Modelo del Dominio

### 1. Qué se decidió
En [`docs/modeloDelDominio.md`](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md), el autor unificó en la Sección 3 los conceptos *"Rotación/Caída/Desplazamiento &rarr; Movimiento"* y concluyó en la Sección 4 que `Movimiento` sí es una entidad del dominio (*"Verbos asociados: Ejecutarse, validarse &rarr; ¿Es entidad? Sí"*). Finalmente, en el diagrama UML de la Sección 8 se modela como clase conceptual agregada a `Turno` (`Turno o-- Movimiento : "permite"`).

### 2. Qué propongo
**Eliminar la clase `Movimiento` del modelo del dominio** y modelar los desplazamientos y rotaciones como comportamientos dinámicos, eventos u operaciones de la `Pieza` sobre el `Tablero`.

### 3. Argumento técnico
El temario de la asignatura sobre identificación de clases conceptuales ([`00004-MdD.md`](https://github.com/mmasias/IdSw1/blob/main/temario/contenidos/00004-MdD.md)) establece que una clase conceptual debe representar una abstracción con significado, estado duradero o información relevante que el sistema deba recordar (*"información que debe recordarse"*).

En el Tetris tradicional:
* Un movimiento (traslación lateral, rotación o caída) es una **acción instantánea y efímera**. Modifica las coordenadas de la pieza activa en el tablero en una fracción de segundo y concluye inmediatamente.

* No existe persistencia ni historial de movimientos: una vez que la pieza se mueve, el juego no necesita almacenar una colección de objetos `Movimiento` (a diferencia del ajedrez, donde las jugadas se registran en una planilla de partida en notación algebraica para fines de auditoría o repetición).

Modelar `Movimiento` como una entidad del dominio es un antipatrón clásico de modelado orientado a objetos conocido como **cosificación de métodos**: convertir un verbo o una operación en un sustantivo/clase. Esto produce clases anémicas y vacías que complican artificialmente el modelo estático sin añadir riqueza semántica alguna.

> En mi [Modelo del Dominio propuesto](modelosUML/ModeloDominioPropuesto.puml) ([ver SVG](images/ModeloDominioPropuesto.svg)) se elimina la clase `Movimiento`, dejando que el movimiento se exprese de forma natural en los diagramas de interacción y casos de uso (`Mover pieza`, `Rotar pieza`).
