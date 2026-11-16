# Incoherencias y Decisiones Discutibles del Modelo del Dominio y Requisitos (`pyTetris`)

Análisis del [Modelo del Dominio](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md) y [Proceso de Requisitos](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md), contrastados en base a la documentación y metodología del repositorio de [idsw1](https://github.com/mmasias/idsw1).

---

## Incoherencias

### `alternarVelocidadCaida()` vs. `acelerarCaida()` vs. prototipo sin control de velocidad

**Referencias**
  * [ProcesoRequisitos.md](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#L52) (tabla de casos de uso), [DiagramaCasosUso.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaCasosUso.puml#L14), [DiagramaDeContexto001-ActorJugador.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorJugador.puml#L15) y [alternarVelocidadCaida.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/alternarVelocidadCaida.puml#L1-L41).
  * [iniciarPartida.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/iniciarPartida.puml#L32) y el archivo huérfano [acelerarCaida.svg](https://github.com/puntoReflex/pyTetris/blob/main/images/modelosUML/detalleCasosUso/acelerarCaida.svg).
  * [ProcesoRequisitos.md](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#L175) y [tetris.png](https://github.com/puntoReflex/pyTetris/blob/main/images/tetris.png).

**Evidencias**
  * Los primeros lo denominan `alternarVelocidadCaida()` (conmuta entre velocidad normal y acelerada).
  * En `iniciarPartida.puml` se lista como caso de uso permitido `acelerarCaida()`, y en `images/modelosUML/detalleCasosUso/` sigue existiendo `acelerarCaida.svg` (sin `.puml` asociado).
  * En el prototipo (`tetris.png`) y en el texto que lo acompaña solo hay teclas para mover y rotar (`4=izq, 6=der, 7=rotar↺, 9=rotar↻`), sin ningún comando para alternar o acelerar la caída.

**Correcciones**
  * Corregir `iniciarPartida.puml` (cambiando `acelerarCaida()` por `alternarVelocidadCaida()`), eliminar el SVG huérfano `acelerarCaida.svg` y añadir el comando de velocidad al prototipo, para mantener trazabilidad única en todo el modelo de requisitos.

---

### Transición `EnCurso --> Inactivo` (`reiniciarPartida()`) ausente en el diagrama de contexto general

**Referencias**
  * [DiagramaDeContexto000.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto000.puml#L10-L15).
  * [DiagramaDeContexto001-ActorJugador.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorJugador.puml#L18) y [reiniciarPartida.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml#L5).

**Evidencias**
  * Tanto el diagrama de contexto de `Jugador` como el detalle de `reiniciarPartida()` permiten reiniciar la partida estando en `EnCurso` para pasar a `Inactivo`.
  * Sin embargo, `DiagramaDeContexto000.puml` (que presenta los estados del sistema como un todo) **no tiene ninguna transición** `EnCurso --> Inactivo` (solo `Finalizado -> Inactivo`).

**Correcciones**
  * Añadir `EnCurso --> Inactivo` en `DiagramaDeContexto000.puml`, ya que la vista global de estados del sistema debe recoger todas las transiciones posibles de los distintos actores.

---

### Transición `Finalizado -> [*]` sin actor ni caso de uso que la dispare

**Referencias**
  * [DiagramaDeContexto000.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto000.puml#L14).
  * [DiagramaDeContexto001-ActorJugador.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorJugador.puml#L20), [DiagramaDeContexto001-ActorTiempo.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorTiempo.puml#L17) y la tabla de [ProcesoRequisitos.md](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#L45-L53).

**Evidencias**
  * `DiagramaDeContexto000.puml` incluye la transición `Finalizado -> [*]`.
  * Ni en los diagramas de contexto por actor ni en la tabla/diagrama de casos de uso existe ningún caso de uso (como `cerrarSistema()` o `salir()`) que permita pasar de `Finalizado` (ni de `Inactivo` o `EnCurso`) al estado final `[*]`.

**Correcciones**
  * O bien añadir explícitamente el caso de uso `cerrarSistema()` para el actor `Jugador` y reflejarlo en `DiagramaDeContexto001-ActorJugador.puml` (y en su detalle), o bien eliminar `Finalizado -> [*]` de `DiagramaDeContexto000.puml`.

---

### Inconsistencia en la nomenclatura de estados entre contexto y detalle

**Referencias**
  * [DiagramaDeContexto000.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto000.puml#L6-L8), [DiagramaDeContexto001-ActorJugador.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorJugador.puml#L5-L7) y [DiagramaDeContexto001-ActorTiempo.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorTiempo.puml#L5-L7).
  * Todos los `.puml` de `modelosUML/detalleCasosUso/` (p. ej. [descenderPieza.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/descenderPieza.puml#L5-L76)) y [ProcesoRequisitos.md](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#L174).

**Evidencias**
  * Los diagramas de contexto usan `Inactivo`, `EnCurso`, `Finalizado`.
  * Los detalles de casos de uso y el texto del prototipo usan `INACTIVO`, `EN_CURSO`, `FINALIZADO`.

**Correcciones**
  * Unificar el identificador de estado en todos los diagramas (por ejemplo, `INACTIVO`, `EN_CURSO`, `FINALIZADO` en ambos niveles), ya que los diagramas de detalle enlazan directamente precondición y postcondición con el diagrama de contexto.

---

### El prototipo único contradice múltiples flujos detallados y estados del sistema

**Referencias**
  * [ProcesoRequisitos.md](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#L166-L185) y [tetris.png](https://github.com/puntoReflex/pyTetris/blob/main/images/tetris.png).
  * [arrancarSistema.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/arrancarSistema.puml#L16-L18), [reiniciarPartida.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml#L28-L31), [descenderPieza.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/descenderPieza.puml#L65), [moverPieza.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/moverPieza.puml) y [rotarPieza.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/rotarPieza.puml#L16).

**Evidencias**
  * `ProcesoRequisitos.md` (lín. 185) afirma: *"Todo está implícito y claro con esa única imagen: agregar más sería redundante"*.
  * Sin embargo, `tetris.png` solo muestra el estado `EN_CURSO` con 4 controles (`4=izq, 6=der, 7=rotar↺, 9=rotar↻`) y `Score: 0`. No muestra la pantalla inicial (`INACTIVO`), ni la petición de `iniciarPartida()`, ni el mensaje de `Game Over` (`FINALIZADO`), ni el diálogo de confirmación/cancelación de `reiniciarPartida()`, ni cómo se solicita `reiniciarPartida()` o `alternarVelocidadCaida()`.
  * Además, `tetris.png` distingue dos rotaciones (`7=rotar↺, 9=rotar↻`), mientras que `rotarPieza.puml` solo dice *"Jugador solicita rotar pieza"* sin indicar dirección (a diferencia de `moverPieza.puml`, que sí especifica *"(izquierda o derecha)"*).

**Correcciones**
  * Ampliar la sección de prototipado con bocetos de las pantallas/estados restantes (`INACTIVO`, confirmación de reinicio, `FINALIZADO`) y los controles faltantes, y especificar los sentidos de rotación en `rotarPieza.puml`.

---

### `ModeloDominio.puml` vs. tabla de relaciones (Sección 7) y requisitos: doble composición y falta de multiplicidades/atributos

**Referencias**
  * [ModeloDominio.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/ModeloDominio.puml#L1-L18).
  * [modeloDelDominio.md](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#L122-L129) (Sección 6: `Puntuación` y `Nivel`), [modeloDelDominio.md](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#L146-L154) (Sección 7: tabla de relaciones) y [iniciarPartida.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/iniciarPartida.puml#L23) / [descenderPieza.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/descenderPieza.puml#L51).

**Evidencias**
  * La Sección 7 define multiplicidades explícitas (`1:1`, `1:*`, `*:1`) y omite la relación `Turno - Movimiento`, mientras que `ModeloDominio.puml` (Sección 8) **no muestra ninguna multiplicidad** pero sí dibuja `Turno o-- Movimiento`.
  * `ModeloDominio.puml` dibuja doble composición sobre `Celda`: `Tablero *-- Celda` y `Linea *-- Celda`. En UML, una clase no puede ser parte compuesta exclusiva de dos todos distintos al mismo tiempo sin jerarquía entre ellos.
  * La Sección 6 excluye `Puntuación` como entidad argumentando que es *"propiedad de Juego"*, pero en `ModeloDominio.puml` `Juego` no tiene ningún atributo (ni `puntuación`, ni `velocidad`), pese a que en `ProcesoRequisitos.md` la puntuación y la velocidad son datos centrales del sistema.

**Correcciones**
  * Corregir `ModeloDominio.puml` añadiendo las multiplicidades de la Sección 7, resolviendo la doble composición (p. ej. `Tablero *-- Linea` y `Linea *-- Celda`, o cambiando una a agregación/asociación), e incorporando los atributos necesarios (`puntuación`, `velocidad`, etc.).

---

### `descenderPieza.puml` asume eliminación de líneas y actualización de puntuación incondicionales

**Referencias**
  * [descenderPieza.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/descenderPieza.puml#L24-L54).

**Evidencias**
  * Tras llegar a `FijandoPieza`, el diagrama pasa incondicionalmente por `EliminandoLineas` y siempre ejecuta *"Sistema presenta tablero sin líneas completas / Sistema presenta puntuación actualizada"*, sin contemplar el camino normal en el que al fijarse una pieza **no** se completa ninguna línea.
  * Asimismo, en `DescendiendoPieza -> c1` presenta *"pieza activa en nueva posición"* antes de comprobar si la pieza ya había alcanzado el fondo o colisionaba abajo.

**Correcciones**
  * Corregir `descenderPieza.puml` añadiendo un nodo de decisión tras `FijandoPieza` (¿hay líneas completas? sí/no) y validando si el descenso es posible antes de presentar la pieza en la nueva posición.

---

## Decisiones discutibles

### Modelar la velocidad como un *toggle* (`alternarVelocidadCaida()`) en lugar de caída controlada o reiniciable

**Cambios:** Crear el caso de uso `alternarVelocidadCaida()` ([alternarVelocidadCaida.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/alternarVelocidadCaida.puml#L10-L32)) que conmuta un modo binario entre velocidad normal y acelerada.

**Propuesta:** Modelarlo como un movimiento de descenso puntual solicitado por el jugador (*soft drop* / `bajarPieza()` o *hard drop* / `soltarPieza()`), o bien que la velocidad acelerada se restablezca automáticamente a normal al fijarse la pieza.

**Justificación:** En un juego con entrada por comandos (como la consola de `tetris.png`), dejar activado un modo de velocidad acelerada entre piezas provoca que la siguiente pieza aparezca cayendo rápido antes de que el jugador pueda reaccionar y desactivar el modo, deteriorando la jugabilidad.

---

### Descartar `Siguiente pieza` (Preview) del Modelo del Dominio calificándolo de "detalle de UI"

**Cambios:** En [modeloDelDominio.md](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#L193) se descarta `Preview/Siguiente` alegando que es *"Detalle de interfaz, no del dominio puro"*.

**Propuesta:** Incluir la `SiguientePieza` (o cola de piezas próximas) dentro de `Juego` en el Modelo del Dominio.

**Justificación:** El Modelo del Dominio debe recoger los conceptos e información relevante que el negocio/juego debe recordar. En Tetris, saber cuál es la siguiente pieza que va a entrar no es un adorno visual de la interfaz (como un color o una fuente), sino una regla del juego que determina la estrategia del jugador al colocar la pieza actual. Confundir la *existencia* del concepto en el dominio con su *renderizado* en pantalla empobrece el modelo.

---

### Incluir `Turno` en el Modelo del Dominio de Tetris

**Cambios:** Modelar `Juego *-- Turno` y `Turno o-- Movimiento` en [ModeloDominio.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/ModeloDominio.puml#L5-L14) (y listarlo como punto de partida en [ProcesoRequisitos.md](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#L20)).

**Propuesta:** Eliminar `Turno` del Modelo del Dominio.

**Justificación:** Tetris no es un juego por turnos discretos (como ajedrez o tres en raya); la gravedad (`Tiempo`) y las acciones del jugador (`Jugador`) ocurren de forma asíncrona y continua durante la partida. Introducir `Turno` fuerza una metáfora ajena al dominio real del Tetris y se demuestra innecesario en la propia documentación: tras el Modelo del Dominio, `Turno` no vuelve a aparecer en ningún caso de uso ni diagrama de requisitos.

---

### Pedir confirmación en `reiniciarPartida()` también cuando la partida está `FINALIZADO`

**Cambios:** En [reiniciarPartida.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml#L10-L32), tanto desde `EN_CURSO` como desde `FINALIZADO`, el sistema solicita confirmación explícita antes de reiniciar.

**Propuesta:** Exigir confirmación únicamente cuando se reinicia desde `EN_CURSO`, y reiniciar directamente cuando se parte de `FINALIZADO`.

**Justificación:** La confirmación protege al usuario de perder una partida activa por error. Cuando el juego ya está en `FINALIZADO` (*Game Over*), no hay progreso en curso que proteger; obligar a confirmar añade un paso superfluo al flujo habitual de "volver a jugar".

---

### Incluir `arrancarSistema()` como caso de uso y hacer que `reiniciarPartida()` vuelva a `INACTIVO`

**Cambios:** Definir `arrancarSistema()` como caso de uso del `Jugador` ([ProcesoRequisitos.md](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#L47)) y hacer que `reiniciarPartida()` transicione a `INACTIVO` en lugar de iniciar una nueva partida.

**Propuesta:** Omitir `arrancarSistema()` como caso de uso (arrancar el programa es el punto de partida técnico para llegar al estado inicial `INACTIVO`) y hacer que `reiniciarPartida()` reinicie directamente a `EN_CURSO` con un tablero limpio.

**Justificación:** Un caso de uso debe dar un resultado observable de valor para el actor; `arrancarSistema()` apenas muestra una pantalla de espera sin lógica de dominio. Por otro lado, que `reiniciarPartida()` devuelva a `INACTIVO` obliga al jugador a ejecutar dos casos de uso (`reiniciarPartida()` + `iniciarPartida()`) para conseguir su único objetivo real: empezar una partida nueva.

---

### Excluir `Jugador` del Modelo del Dominio por ser single-player

**Cambios:** Excluir `Jugador` en [modeloDelDominio.md](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#L113-L120) porque *"En Tetris single-player es implícito"*.

**Propuesta:** Mantener `Jugador` como concepto del dominio (asociado a `Juego` y/o a sus puntuaciones).

**Justificación:** El propio documento afirma en su conclusión ([lín. 277](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#L277)) que el modelo *"Es extensible para features futuras"*, y en `ProcesoRequisitos.md` el `Jugador` es el actor principal de 6 de los 7 casos de uso. Omitirlo del MdD simplemente porque hay una única instancia en ejecución reduce la trazabilidad entre el dominio y los requisitos y complica futuras extensiones naturales (récords, estadísticas por jugador o modo multijugador).

---

### Justificar una única captura de pantalla como prototipo suficiente para todo el sistema

**Cambios:** En [ProcesoRequisitos.md](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#L172-L185), usar solo [tetris.png](https://github.com/puntoReflex/pyTetris/blob/main/images/tetris.png) argumentando que *"Todo está implícito y claro con esa única imagen: agregar más sería redundante"*.

**Propuesta:** Añadir bocetos ligeros (p. ej. texto ASCII o `salt` de PlantUML) para la pantalla inicial, el Game Over y la confirmación de reinicio.

**Justificación:** El objetivo del prototipado de casos de uso en RUP es validar con el usuario todos los elementos de interfaz necesarios para disparar y observar cada caso de uso antes de pasar a diseño/código. Con una sola imagen de `EN_CURSO`, los casos de uso de gestión de partida (`iniciarPartida`, `reiniciarPartida`) y el estado `FINALIZADO` quedan sin representación tangible que validar.
