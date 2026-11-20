# Reto 002 - Análisis del Tetris

## 1. Incoherencias

### 1.1. Falta una transición de reinicio en el diagrama general

**Dónde aparece:**

- Diagrama general de estados (`DiagramaDeContexto000.puml`).
- Diagrama de contexto del Jugador (`DiagramaDeContexto001-ActorJugador.puml`).

**Qué dice cada uno:**

Al comparar ambos diagramas, en el diagrama general de estados no existe ninguna transición que permita pasar directamente del estado `EN_CURSO` al estado `INACTIVO`. Sin embargo, en el diagrama de contexto del Jugador sí se contempla esa transición cuando el usuario ejecuta `reiniciarPartida()`.

Esto es una incoherencia porque el diagrama general debe actuar como la vista completa de todos los estados y transiciones válidas del sistema. Si un actor puede provocar ese cambio de estado durante una partida, el diagrama general tiene que reflejarlo.

**Qué corregiría y por qué:**

Añadiría la transición de `EN_CURSO` a `INACTIVO` en `DiagramaDeContexto000.puml`, dejando claro que ocurre al confirmar un reinicio. De esta forma, ambos diagramas mantienen coherencia entre sí y se puede entender el ciclo de vida real del juego sin contradicciones.

### 1.2. Contradicción entre `Turno` (Dominio) y el actor `Tiempo` (Requisitos)

**Dónde aparece:**

- Modelo del Dominio (`modeloDelDominio.md` y diagrama UML del dominio)[cite: 2].
- Sección *Punto de partida* de `ProcesoRequisitos.md`.
- Actores y Casos de Uso (`DiagramaCasosUso.puml` y detalle de `descenderPieza()`).

**Qué dice cada uno:**

El Modelo del Dominio —tanto en el texto como en el diagrama UML[cite: 2]— y el propio inicio de `ProcesoRequisitos.md` modelan el funcionamiento del juego en torno al concepto de `Turno`. Sin embargo, en los Actores y Casos de Uso, `Turno` desaparece por completo y el sistema pasa a operar mediante eventos automáticos disparados por el actor `Tiempo` (`Tiempo -- descenderPieza`).

**Qué corregiría y por qué:**

Eliminaría `Turno` del Modelo del Dominio y de la lista de *Punto de partida* en `ProcesoRequisitos.md`. El enfoque basado en el actor `Tiempo` refleja fielmente cómo funciona el Tetris en realidad, y retirarlo del dominio mantiene la coherencia entre la conceptualización y los requisitos.

### 1.3. Cancelación de `reiniciarPartida()` ausente en el diagrama de contexto del Jugador

**Dónde aparece:**

- Detalle de `reiniciarPartida()` (`reiniciarPartida.puml`).
- Diagrama de contexto del Jugador (`DiagramaDeContexto001-ActorJugador.puml`).

**Qué dice cada uno:**

En el detalle de `reiniciarPartida()` se modela un paso de confirmación que permite al jugador cancelar la acción y permanecer en `EN_CURSO` o en `FINALIZADO`. En cambio, en el diagrama de contexto del Jugador esa operación aparece únicamente como una transición directa hacia `INACTIVO`, sin reflejar que el estado puede mantenerse sin cambios.

**Qué corregiría y por qué:**

Alinearía ambos diagramas para que cuenten exactamente lo mismo: o bien se añaden las autotransiciones de cancelación en el diagrama de contexto del Jugador, o bien se elimina el paso de confirmación y cancelación del detalle de `reiniciarPartida()`.

---

## 2. Decisiones discutibles

### 2.1. "Turno" como concepto del dominio

**Qué se decidió:**

Modelar `Turno` como una entidad central del modelo del dominio, definida como "Ciclo de juego" (Dominio §2 y §4)[cite: 2].

**Qué propongo:**

Eliminar `Turno` del modelo del dominio y relacionar `Juego` directamente con la `Pieza` activa y sus `Movimiento`.

**Argumento:**

Tetris es un juego continuo en tiempo real, no un juego por turnos, y ningún jugador utiliza ese término de forma natural al explicar las reglas (de hecho, solo aparece forzado en una observación de §1). Además, definirlo como "ciclo de juego" introduce el concepto técnico de *game loop*, contradiciendo el principio del modelo del dominio de mantenerse libre de contaminación tecnológica. Prueba de que sobra es que ninguno de los siete casos de uso ni de sus detalles vuelve a mencionarlo, ni siquiera en `descenderPieza()`, donde el control pasa directamente al actor `Tiempo`.

### 2.2. Pedir confirmación para reiniciar cuando la partida ya ha terminado

**Qué se decidió:**

En el detalle de `reiniciarPartida()`, el sistema muestra un mensaje de confirmación al jugador tanto si viene desde `EN_CURSO` como si viene desde `FINALIZADO`.

**Qué propongo:**

Solicitar confirmación únicamente cuando la partida está en `EN_CURSO`, permitiendo reiniciar de forma directa desde `FINALIZADO`.

**Argumento:**

Los diálogos de confirmación existen para evitar que el usuario pierda progreso por error. Cuando la partida está en `FINALIZADO` (Game Over) ya no hay ninguna jugada en marcha que proteger, por lo que preguntar *"¿estás seguro?"* en ese punto no aporta seguridad y solo añade fricción innecesaria.

### 2.3. `reiniciarPartida()` lleva a `INACTIVO`

**Qué se decidió:**

Confirmar `reiniciarPartida()` transiciona siempre al estado `INACTIVO`, tanto desde `EN_CURSO` como desde `FINALIZADO` (según el diagrama del Jugador y el detalle de `reiniciarPartida()`).

**Qué propongo:**

Hacer que `reiniciarPartida()` inicie directamente una nueva partida en `EN_CURSO`, o bien renombrar el caso de uso a `abandonarPartida()` o `volverAlInicio()`.

**Argumento:**

Semánticamente, "reiniciar" significa volver a empezar de inmediato. Con el diseño actual, si un jugador pierde y quiere echar otra partida, tiene que pasar por tres pasos (`reiniciarPartida()`, confirmar y luego `iniciarPartida()`). El flujo resulta más lento de lo esperado y el nombre del caso de uso no se corresponde con su efecto real.
