# Incoherencias y Decisiones Discutibles del Modelo del Dominio y Requisitos (`pyTetris`)

Revisión del [Modelo del Dominio](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md) y del [Proceso de Requisitos](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md), tomando como referencia la documentación y la metodología del repositorio [idsw1](https://github.com/mmasias/idsw1).

---

## Incoherencias

### `alternarVelocidadCaida()` vs. `acelerarCaida()` y un prototipo sin control de velocidad

**Referencias**
  * [ProcesoRequisitos.md](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#L52) (tabla de casos de uso), [DiagramaCasosUso.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaCasosUso.puml#L14), [DiagramaDeContexto001-ActorJugador.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorJugador.puml#L15) y [alternarVelocidadCaida.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/alternarVelocidadCaida.puml#L1-L41).
  * [iniciarPartida.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/iniciarPartida.puml#L32) y el archivo huérfano [acelerarCaida.svg](https://github.com/puntoReflex/pyTetris/blob/main/images/modelosUML/detalleCasosUso/acelerarCaida.svg).
  * [ProcesoRequisitos.md](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#L175) y [tetris.png](https://github.com/puntoReflex/pyTetris/blob/main/images/tetris.png).

**Qué ocurre**
  * Mientras que en la mayor parte de la documentación el caso de uso se llama `alternarVelocidadCaida()` (para cambiar entre velocidad normal y rápida), en `iniciarPartida.puml` sigue apareciendo como `acelerarCaida()`. Además, en `images/modelosUML/detalleCasosUso/` queda un `acelerarCaida.svg` suelto que ya no tiene ningún archivo `.puml` detrás.
  * Por otro lado, ni el prototipo (`tetris.png`) ni su explicación incluyen ninguna tecla para controlar la velocidad; solo contemplan movimientos y giros (`4=izq, 6=der, 7=rotar↺, 9=rotar↻`).

**Cómo solucionarlo**
  * Actualizar `iniciarPartida.puml` sustituyendo `acelerarCaida()` por `alternarVelocidadCaida()`, borrar el archivo `acelerarCaida.svg` que quedó obsoleto y añadir el comando de velocidad al prototipo para que todo el modelo hable el mismo idioma.

---

### Falta la transición `EnCurso --> Inactivo` (`reiniciarPartida()`) en el diagrama de contexto general

**Referencias**
  * [DiagramaDeContexto000.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto000.puml#L10-L15).
  * [DiagramaDeContexto001-ActorJugador.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorJugador.puml#L18) y [reiniciarPartida.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml#L5).

**Qué ocurre**
  * Tanto el diagrama de contexto del `Jugador` como el detalle de `reiniciarPartida()` contemplan que se pueda reiniciar una partida a medias (pasando de `EnCurso` a `Inactivo`).
  * Sin embargo, en `DiagramaDeContexto000.puml` —que debería resumir todos los estados y transiciones posibles del sistema— esa flecha no existe: solo se permite volver a `Inactivo` desde `Finalizado`.

**Cómo solucionarlo**
  * Incluir la transición `EnCurso --> Inactivo` en `DiagramaDeContexto000.puml` para que la vista global refleje fielmente lo que ya permiten los diagramas específicos de los actores.

---

### Transición `Finalizado -> [*]` sin nadie que la active

**Referencias**
  * [DiagramaDeContexto000.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto000.puml#L14).
  * [DiagramaDeContexto001-ActorJugador.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorJugador.puml#L20), [DiagramaDeContexto001-ActorTiempo.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorTiempo.puml#L17) y la tabla de [ProcesoRequisitos.md](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#L45-L53).

**Qué ocurre**
  * En `DiagramaDeContexto000.puml` aparece una salida hacia el estado final (`Finalizado -> [*]`).
  * El problema es que no hay ningún caso de uso (tipo `cerrarSistema()` o `salir()`) ni en la tabla de requisitos ni en los diagramas por actor que lleve el sistema hasta ese estado final, ni desde `Finalizado` ni desde ningún otro estado.

**Cómo solucionarlo**
  * Hay dos caminos: o se crea explícitamente el caso de uso `cerrarSistema()` para el `Jugador` (añadiéndolo a su diagrama de contexto y detallándolo), o se elimina la transición `Finalizado -> [*]` de `DiagramaDeContexto000.puml`.

---

### Nombres distintos para los mismos estados según el diagrama

**Referencias**
  * [DiagramaDeContexto000.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto000.puml#L6-L8), [DiagramaDeContexto001-ActorJugador.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorJugador.puml#L5-L7) y [DiagramaDeContexto001-ActorTiempo.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorTiempo.puml#L5-L7).
  * Todos los `.puml` dentro de `modelosUML/detalleCasosUso/` (por ejemplo, [descenderPieza.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/descenderPieza.puml#L5-L76)) y [ProcesoRequisitos.md](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#L174).

**Qué ocurre**
  * En los diagramas de contexto los estados se escriben en formato *PascalCase* (`Inactivo`, `EnCurso`, `Finalizado`).
  * En cambio, en los diagramas de detalle de casos de uso y en el texto del prototipo pasan a escribirse en mayúsculas con guion bajo (`INACTIVO`, `EN_CURSO`, `FINALIZADO`).

**Cómo solucionarlo**
  * Elegir una única convención (por ejemplo, `INACTIVO`, `EN_CURSO`, `FINALIZADO`) y aplicarla en todos los niveles, ya que las precondiciones y postcondiciones de los casos de uso dependen directamente de los estados definidos en el contexto.

---

### Un único prototipo que deja fuera pantallas y flujos clave

**Referencias**
  * [ProcesoRequisitos.md](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#L166-L185) y [tetris.png](https://github.com/puntoReflex/pyTetris/blob/main/images/tetris.png).
  * [arrancarSistema.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/arrancarSistema.puml#L16-L18), [reiniciarPartida.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml#L28-L31), [descenderPieza.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/descenderPieza.puml#L65), [moverPieza.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/moverPieza.puml) y [rotarPieza.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/rotarPieza.puml#L16).

**Qué ocurre**
  * En `ProcesoRequisitos.md` (lín. 185) se justifica tener una sola captura diciendo: *"Todo está implícito y claro con esa única imagen: agregar más sería redundante"*.
  * Sin embargo, `tetris.png` únicamente enseña el juego en marcha (`EN_CURSO`) con cuatro teclas (`4=izq, 6=der, 7=rotar↺, 9=rotar↻`) y el marcador a cero. No se ve cómo es la pantalla inicial (`INACTIVO`), cómo se empieza una partida, cómo se muestra el *Game Over* (`FINALIZADO`), ni cómo funcionan los avisos de confirmación al reiniciar o los controles de reinicio y velocidad.
  * Además, hay un pequeño desajuste en las rotaciones: el prototipo diferencia entre giro horario y antihorario (`7=rotar↺, 9=rotar↻`), pero `rotarPieza.puml` solo menciona *"Jugador solicita rotar pieza"* sin aclarar el sentido (algo que `moverPieza.puml` sí hace al indicar *"(izquierda o derecha)"*).

**Cómo solucionarlo**
  * Completar el prototipado con bocetos sencillos para los estados y diálogos que faltan (`INACTIVO`, confirmación de reinicio y `FINALIZADO`), añadir los controles ausentes y detallar los sentidos de giro en `rotarPieza.puml`.

---

### Desajustes en `ModeloDominio.puml`: doble composición, sin multiplicidades y sin atributos

**Referencias**
  * [ModeloDominio.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/ModeloDominio.puml#L1-L18).
  * [modeloDelDominio.md](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#L122-L129) (Sección 6: `Puntuación` y `Nivel`), [modeloDelDominio.md](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#L146-L154) (Sección 7: tabla de relaciones) y [iniciarPartida.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/iniciarPartida.puml#L23) / [descenderPieza.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/descenderPieza.puml#L51).

**Qué ocurre**
  * En la Sección 7 se especifican multiplicidades (`1:1`, `1:*`, `*:1`) y no aparece ninguna relación entre `Turno` y `Movimiento`. En cambio, `ModeloDominio.puml` (Sección 8) omite todas las multiplicidades pero sí pinta `Turno o-- Movimiento`.
  * El diagrama plantea una doble composición hacia `Celda`: tanto `Tablero *-- Celda` como `Linea *-- Celda`. En UML, un objeto no puede pertenecer en exclusiva (composición fuerte) a dos contenedores distintos a la vez si no hay una jerarquía clara entre ellos.
  * En la Sección 6 se descarta crear una entidad `Puntuación` porque se considera una *"propiedad de Juego"*, pero luego en `ModeloDominio.puml` la clase `Juego` aparece completamente vacía, sin atributos como `puntuación` o `velocidad`, a pesar de que son piezas centrales en los requisitos.

**Cómo solucionarlo**
  * Trasladar las multiplicidades de la Sección 7 a `ModeloDominio.puml`, corregir la doble composición (por ejemplo, haciendo que `Tablero` contenga `Linea` y `Linea` contenga `Celda`, o suavizando una de las relaciones a agregación/asociación) y añadir en `Juego` los atributos que realmente maneja el sistema (`puntuación`, `velocidad`, etc.).

---

### `descenderPieza.puml` da por hecho que siempre se completan líneas

**Referencias**
  * [descenderPieza.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/descenderPieza.puml#L24-L54).

**Qué ocurre**
  * Cuando una pieza llega al fondo (`FijandoPieza`), el flujo pasa siempre por `EliminandoLineas` y muestra *"Sistema presenta tablero sin líneas completas / Sistema presenta puntuación actualizada"*. No contempla el escenario más habitual en una partida: que la pieza se pose sin completar ninguna fila.
  * También hay un problema de orden en `DescendiendoPieza -> c1`: el sistema muestra la pieza en su nueva posición antes de comprobar si realmente podía bajar o si ya había chocado.

**Cómo solucionarlo**
  * Introducir una bifurcación tras `FijandoPieza` para comprobar si hay líneas completas antes de intentar borrarlas o sumar puntos, y validar que el descenso es posible antes de actualizar la posición en pantalla.

---

## Decisiones discutibles

### Tratar la velocidad como un interruptor (`alternarVelocidadCaida()`) en lugar de una caída controlada

**Qué se ha hecho:** Se ha diseñado `alternarVelocidadCaida()` ([alternarVelocidadCaida.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/alternarVelocidadCaida.puml#L10-L32)) como un modo binario que se enciende y se apaga.

**Qué sería mejor:** Plantearlo como una acción puntual de descenso (*soft drop* / `bajarPieza()` o *hard drop* / `soltarPieza()`), o al menos hacer que la velocidad vuelva automáticamente a normal en cuanto la pieza actual toque el fondo.

**Por qué:** En una interfaz basada en comandos de consola (como la de `tetris.png`), dejar el modo rápido activado entre una pieza y la siguiente hace que la nueva pieza salga disparada hacia abajo antes de que al jugador le dé tiempo a desactivarlo, lo que vuelve el control bastante frustrante.

---

### Dejar fuera la `Siguiente pieza` (Preview) por considerarla un "detalle de UI"

**Qué se ha hecho:** En [modeloDelDominio.md](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#L193) se descarta incluir `Preview/Siguiente` bajo el argumento de que es un *"Detalle de interfaz, no del dominio puro"*.

**Qué sería mejor:** Incorporar `SiguientePieza` (o una cola de próximas piezas) dentro de `Juego` en el Modelo del Dominio.

**Por qué:** El Modelo del Dominio debe recoger toda la información relevante que el sistema necesita recordar para que el juego funcione. En Tetris, conocer qué pieza viene después no es un capricho visual como el color del fondo; es una mecánica fundamental que condiciona dónde coloca el jugador la pieza actual. Confundir la existencia de ese dato en las reglas del juego con cómo se dibuja en pantalla deja el modelo incompleto.

---

### Introducir el concepto de `Turno` en un juego continuo como Tetris

**Qué se ha hecho:** Se incluyen las relaciones `Juego *-- Turno` y `Turno o-- Movimiento` en [ModeloDominio.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/ModeloDominio.puml#L5-L14) y se menciona como base en [ProcesoRequisitos.md](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#L20).

**Qué sería mejor:** Eliminar `Turno` del Modelo del Dominio.

**Por qué:** Tetris no funciona por turnos alternos como el ajedrez o las damas; la caída por gravedad (`Tiempo`) y las órdenes del `Jugador` ocurren de manera continua y asíncrona. Meter `Turno` con calzador fuerza una estructura que no encaja con la naturaleza del juego, y la prueba es que después del Modelo del Dominio ese concepto desaparece por completo y no se vuelve a usar en ningún caso de uso.

---

### Pedir confirmación para reiniciar cuando la partida ya ha terminado (`FINALIZADO`)

**Qué se ha hecho:** En [reiniciarPartida.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml#L10-L32), el sistema pide confirmación tanto si la partida está `EN_CURSO` como si ya está en `FINALIZADO`.

**Qué sería mejor:** Pedir confirmación solo cuando se intenta reiniciar una partida `EN_CURSO`, y reiniciar directamente si ya está en `FINALIZADO`.

**Por qué:** La confirmación tiene sentido para evitar que el jugador tire por la borda una partida activa por pulsar una tecla sin querer. Pero una vez que ha saltado el *Game Over*, ya no hay nada que perder; obligar a confirmar ahí solo añade fricción innecesaria a la acción más normal del mundo: echar otra partida.

---

### Modelar `arrancarSistema()` como caso de uso y hacer que `reiniciarPartida()` vuelva a `INACTIVO`

**Qué se ha hecho:** Se define `arrancarSistema()` como un caso de uso del `Jugador` ([ProcesoRequisitos.md](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#L47)) y se hace que `reiniciarPartida()` devuelva el sistema al estado `INACTIVO` en vez de empezar directamente a jugar.

**Qué sería mejor:** Sacar `arrancarSistema()` de los casos de uso (abrir la aplicación es simplemente el paso técnico previo para llegar al estado inicial `INACTIVO`) y lograr que `reiniciarPartida()` lleve directo a `EN_CURSO` con un tablero nuevo.

**Por qué:** Un caso de uso debe aportar un valor real y observable al usuario, y arrancar el programa solo muestra una pantalla de espera sin lógica de negocio. Por otro lado, si reiniciar te devuelve a `INACTIVO`, obligas al jugador a lanzar dos acciones seguidas (`reiniciarPartida()` + `iniciarPartida()`) para lograr lo único que quería hacer: volver a empezar.

---

### Dejar fuera al `Jugador` del Modelo del Dominio por ser un juego individual

**Qué se ha hecho:** En [modeloDelDominio.md](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#L113-L120) se omite `Jugador` argumentando que *"En Tetris single-player es implícito"*.

**Qué sería mejor:** Mantener `Jugador` como entidad del dominio vinculada a `Juego` y a sus puntuaciones.

**Por qué:** La propia conclusión del documento ([lín. 277](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#L277)) destaca que el diseño *"Es extensible para features futuras"*, y en `ProcesoRequisitos.md` el `Jugador` protagoniza 6 de los 7 casos de uso. Borrarlo del dominio solo porque hoy juega una persona rompe la trazabilidad con los requisitos y pone trabas innecesarias si mañana se quieren añadir tablas de récords, perfiles o modo multijugador.

---

### Dar por bueno un único pantallazo como prototipo de toda la aplicación

**Qué se ha hecho:** En [ProcesoRequisitos.md](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#L172-L185) se usa únicamente [tetris.png](https://github.com/puntoReflex/pyTetris/blob/main/images/tetris.png) alegando que con esa imagen sobra cualquier otra explicación.

**Qué sería mejor:** Añadir pequeños bocetos (en texto ASCII o con `salt` de PlantUML) para representar la pantalla inicial, el *Game Over* y el aviso de reinicio.

**Por qué:** En metodologías como RUP, el prototipado de casos de uso sirve para validar con el usuario todas las vías de interacción antes de ponerse a programar. Con una sola captura del tablero en marcha, tanto la gestión de la partida (`iniciarPartida`, `reiniciarPartida`) como el estado `FINALIZADO` se quedan sin ningún soporte visual que revisar.
