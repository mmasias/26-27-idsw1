# Incoherencias en pyTetris (dominio y requisitos)

Repo analizado: `puntoReflex/pyTetris`. Cada incoherencia indica **dónde** está, **qué dice cada artefacto** y **qué corregiría y por qué**.

**Artefactos revisados:** `modeloDelDominio.md`, `ProcesoRequisitos.md`, `DiagramaCasosUso`, `DiagramaDeContexto000`, `DiagramaDeContexto001-ActorJugador`, `DiagramaDeContexto001-ActorTiempo` y los detalles de los casos de uso `arrancarSistema`, `iniciarPartida`, `descenderPieza`, `moverPieza`, `rotarPieza`, `alternarVelocidadCaida` y `reiniciarPartida`.

**Nomenclatura:** *Dominio §n* = sección n de `modeloDelDominio.md`; *Req* = `ProcesoRequisitos.md`.

**Límites:** los puntos marcados con ⚠ dependen de `images/tetris.png` o del diagrama de la sección 8 del dominio, que no se han podido revisar y deben verificarse. Los diagramas se han leído por su código `.puml`, salvo el de Tiempo, que se comprobó también en su SVG.

---

## I1. "Juego" vs "Partida"
- **Dónde:** Dominio §3 (tabla de sinónimos); Req (actor Jugador y tabla de casos de uso); detalles de `iniciarPartida` y `reiniciarPartida`.
- **Qué dice cada uno:**
  - El dominio unifica "Juego vs Partida → Juego" y descarta "Partida".
  - Req usa `iniciarPartida()`, `reiniciarPartida()` y "gestión de partida".
  - Dentro del propio detalle de `iniciarPartida` el primer estado se llama `InicializandoJuego`, y en `reiniciarPartida` la acción es "limpia estado de partida".
- **Corrección y por qué:** el vocabulario del dominio manda. Se renombran los casos de uso y estados, o se revierte la decisión del dominio. Ahora la trazabilidad dominio → requisitos se rompe en el primer término.

## I2. "Movimiento" unificado vs tres casos de uso
- **Dónde:** Dominio §2 (Rotación "tipo de movimiento") y §3 ("Rotación/Caída/Desplazamiento → Movimiento, son variantes del mismo concepto"); Req (`moverPieza()`, `rotarPieza()`, `descenderPieza()`); detalles de `moverPieza` y `rotarPieza`.
- **Qué dice cada uno:** el dominio dice que son un solo concepto. Req los separa en tres casos de uso, uno de ellos con otro actor (Tiempo). `moverPieza` (izquierda o derecha) y `rotarPieza` tienen estructura idéntica: una decisión válida/inválida que siempre termina en EN_CURSO.
- **Corrección y por qué:** o el dominio mantiene Rotación y Desplazamiento como conceptos distintos, o Req agrupa en un solo caso de uso "mover pieza" con variantes. Ahora se dice una cosa en un sitio y la contraria en otro.

## I3. "Pausar" está en el dominio y no existe en los requisitos
- **Dónde:** Dominio §4 (Juego: "Iniciar, terminar, pausar, mantener puntuación"); diagramas de estados y lista de casos de uso de Req.
- **Qué dice cada uno:** el dominio atribuye a Juego la pausa. Los estados son solo Inactivo, EnCurso y Finalizado, y no hay caso de uso de pausa.
- **Corrección y por qué:** quitar "pausar" de la tabla de verbos, que encaja con un Tetris básico. Si se mantiene, hay que añadir el estado y el caso de uso. Ver también I14.

## I4. La transición a Finalizado está mal ubicada en los diagramas por actor
- **Dónde:** `DiagramaDeContexto000` y `DiagramaDeContexto001-ActorJugador` (EnCurso → Finalizado sin etiqueta); `DiagramaDeContexto001-ActorTiempo` (`descenderPieza [no hay espacio para nueva pieza]`); detalle de `descenderPieza` (el Game Over sale de `descenderPieza` si no hay espacio disponible tras generar la nueva pieza).
- **Qué dice cada uno:** Tiempo y el detalle de `descenderPieza` atribuyen la transición a `descenderPieza`. El diagrama del Jugador la dibuja sin etiqueta, como si fuera suya, y el 000 también sin causa.
- **Corrección y por qué:** quitarla del diagrama del Jugador, o etiquetarla como efecto de `descenderPieza()` (Tiempo), y etiquetarla igual en el 000. Así cada transición dice quién la provoca.

## I5. Los diagramas de estados no describen la misma máquina
- **Dónde:** `DiagramaDeContexto000`, `DiagramaDeContexto001-ActorJugador`, `DiagramaDeContexto001-ActorTiempo` y detalles de `reiniciarPartida` y `arrancarSistema`.
- **Qué dice cada uno:**
  - EnCurso → Inactivo (`reiniciarPartida()`) está en el diagrama del Jugador y en el detalle de `reiniciarPartida`, pero no en el 000.
  - Finalizado → [*] solo está en el 000. Ni el Jugador, ni Tiempo, ni ningún caso de uso lo contemplan, aunque `arrancarSistema()` sí existe.
  - El detalle de `reiniciarPartida` tiene ramas de cancelación (vuelta a EN_CURSO o a FINALIZADO). Finalizado → Finalizado no aparece en ningún diagrama de estados.
  - El diagrama de Tiempo dibuja sin etiqueta transiciones que no son suyas ([*] → Inactivo, Inactivo → EnCurso, Finalizado → Inactivo).
  - En EnCurso → EnCurso (`descenderPieza()`) la guarda complementaria de "no hay espacio" no está escrita.
- **Corrección y por qué:** el 000 debe ser la unión de los diagramas por actor. Añadir EnCurso → Inactivo y las cancelaciones, decidir si existe cierre del sistema (caso de uso nuevo o eliminar Finalizado → [*]), y escribir ambas guardas ("hay espacio" / "no hay espacio") para que sean complementarias.

## I6. "Línea" definida de tres maneras
- **Dónde:** Dominio §1 ("Cuando completas una línea, desaparece"), §2 (Línea = "Fila horizontal completa") y §7 (Tablero–Línea: "agrupaciones lógicas de celdas").
- **Qué dice cada uno:** §2 define la línea como fila ya completa. §7 la trata como cualquier agrupación de celdas. §1 implica que existe antes de completarse.
- **Corrección y por qué:** línea = fila del tablero, y "completa" es un estado suyo. Así concuerdan las tres.

## I7. Celda en dos composiciones a la vez
- **Dónde:** Dominio §7 (Tablero–Celda "Composición" y Línea–Celda "Composición").
- **Qué dice cada uno:** en UML una parte pertenece a un único todo en composición. Aquí la Celda es parte integral del Tablero y a la vez de la Línea.
- **Corrección y por qué:** Línea–Celda como asociación o agregación, o Línea como concepto derivado.

## I8. Relaciones que aparecen en el análisis y desaparecen en la tabla final ⚠
- **Dónde:** Dominio §5 ("Las piezas ocupan celdas", "Cada turno involucra una pieza", "Un turno permite movimientos"), §6 (v2: "se agregan TipoPieza y Turno"), §7 (tabla final), validación ("no falta ningún concepto") y Req (conceptos de partida: Movimiento, Turno).
- **Qué dice cada uno:** Pieza–Celda, Turno–Pieza y Turno–Movimiento no están en la tabla de §7, aunque Movimiento y Turno son conceptos de partida en Req.
- **Corrección y por qué:** completar la tabla, o retirar los conceptos que no se modelan. ⚠ Comprobar en el diagrama de §8 si las relaciones sí están dibujadas.

## I9. Nombres distintos para lo mismo
- **Dónde:**
  - "Tetrimino" (Dominio §2) frente a "Tetrómino" (Dominio §3).
  - `EnCurso`, `Inactivo`, `Finalizado` (diagramas de contexto) frente a `EN_CURSO`, `INACTIVO`, `FINALIZADO` (detalles y texto del prototipo) y "Partida en curso" (Dominio §1).
  - La guarda "no hay espacio para nueva pieza" (diagrama de Tiempo) frente a "no hay espacio disponible" (detalle de `descenderPieza`).
- **Corrección y por qué:** un solo término por concepto y una sola convención de nombres de estado. El dominio proclama como objetivo un vocabulario único.

## I10. `acelerarCaida()` vs `alternarVelocidadCaida()`
- **Dónde:** detalle de `iniciarPartida` (nota final: "permite solicitar ... acelerarCaida()") frente a Req (tabla de casos de uso, diagrama de casos de uso, diagrama del Jugador, detalle `alternarVelocidadCaida`).
- **Qué dice cada uno:** el mismo caso de uso con dos nombres, y no son sinónimos: "alternar" es un interruptor, "acelerar" solo sube.
- **Corrección y por qué:** `alternarVelocidadCaida()`, porque su detalle describe un interruptor entre velocidad normal y acelerada. Hay que corregir la nota de `iniciarPartida`.

## I11. La página describe los casos de uso de forma incompleta
- **Dónde:** Req, "Prototipar casos de uso" (`alternarVelocidadCaida()`: "la pieza cae más rápido"; `reiniciarPartida()`: "vuelve al inicio"; no menciona `arrancarSistema()`) frente a los detalles de `alternarVelocidadCaida`, `reiniciarPartida` y `arrancarSistema`.
- **Qué dice cada uno:**
  - `alternarVelocidadCaida` tiene dos ramas, y la segunda devuelve la caída a velocidad normal.
  - `reiniciarPartida` pide confirmación y permite cancelar.
  - `arrancarSistema` define una pantalla inicial propia.
- **Corrección y por qué:** que el texto describa lo que los detalles dicen. Además, "observable en velocidad" no tiene apoyo en lo que el propio texto dice que muestra la imagen (tablero y score). ⚠ Verificar con `tetris.png`.

## I12. La velocidad es un estado que ningún modelo recoge
- **Dónde:** detalle de `alternarVelocidadCaida` (guardas "velocidad normal activa" y "velocidad acelerada activa"); diagramas de contexto (solo tres estados; el Jugador tiene un bucle EnCurso → EnCurso); Dominio §6 (Velocidad: "atributo de Juego, no entidad") y simplificación "sin niveles de dificultad"; detalles de `descenderPieza` e `iniciarPartida`.
- **Qué dice cada uno:** el comportamiento depende de una variable (velocidad normal/acelerada) que el dominio no recoge como atributo con valores y los diagramas no muestran. Ningún detalle dice si la velocidad acelerada se mantiene al generar la pieza siguiente (`descenderPieza`), ni si `iniciarPartida` la restablece a normal.
- **Corrección y por qué:** recoger en el dominio que Juego tiene una velocidad con dos valores, reflejarlo como subestado de EnCurso, y escribir como poscondición que `iniciarPartida` la deja en normal.

## I13. El estado Inactivo se define solo en parte
- **Dónde:** detalle de `arrancarSistema` (el Sistema presenta la "pantalla inicial" y permite `iniciarPartida()`), `reiniciarPartida` (termina en INACTIVO tras "limpiar estado de partida", sin presentar nada ni permitir nada) e `iniciarPartida` (presenta un "tablero vacío").
- **Qué dice cada uno:** se llega a INACTIVO por dos caminos y solo uno especifica qué ve y qué puede hacer el Jugador.
- **Corrección y por qué:** mover esas poscondiciones al estado INACTIVO o repetirlas en `reiniciarPartida`.

## I14. Quién arranca y detiene al actor Tiempo
- **Dónde:** detalles de `iniciarPartida` y `reiniciarPartida`; Req (actor Tiempo).
- **Qué dice cada uno:**
  - `iniciarPartida` deja la partida en EN_CURSO y lista las acciones del Jugador, pero no dice que se active el disparo de `descenderPieza`.
  - `reiniciarPartida` pide confirmación estando en EN_CURSO y no dice si Tiempo sigue disparando. Si sigue, la pieza podría fijarse o la partida terminar durante la confirmación, y la cancelación volvería a un estado distinto del que se dejó. Si se detiene, es una pausa (ver I3).
- **Corrección y por qué:** escribir como poscondición cuándo Tiempo empieza y cuándo se detiene.

## I15. El vocabulario de los detalles no coincide con el del dominio
- **Dónde:** detalle de `descenderPieza` (estados "FijandoPieza", "EliminandoLineas", "GenerandoNuevaPieza"; "piezas fijadas"), detalles de `moverPieza` y `rotarPieza` ("colisión o límite") y `descenderPieza` ("posición de fondo o sobre piezas fijadas") frente a Dominio §4 (Pieza: "colocarse"; Línea: "eliminarse").
- **Qué dice cada uno:** el dominio dice "colocarse", los detalles dicen "fijar". El dominio no tiene nada que genere piezas nuevas. La noción de "posición no válida" se expresa de dos maneras distintas.
- **Corrección y por qué:** unificar el verbo, añadir al dominio qué origina las piezas, y definir una única condición ("la pieza no puede ocupar la posición", a partir de Celda ocupada y Tablero) reutilizada en los tres casos.

## I16. El prototipo no respalda lo que dice la página ⚠
- **Dónde:** Req, "Prototipar casos de uso" frente a `images/tetris.png` y los detalles de los casos de uso.
- **Qué dice cada uno:**
  - La página dice que la imagen muestra el estado EN_CURSO, las teclas 4, 6, 7 y 9, tablero y score, y que "todo está implícito" para los demás casos de uso.
  - Los detalles definen interacciones que la imagen no puede mostrar: pantalla inicial (`arrancarSistema`, I13), confirmación de reinicio, mensaje de Game Over, tablero vacío con puntuación 0 y velocidad acelerada.
  - Cuatro solicitudes del Jugador en EN_CURSO (mover a izquierda, mover a derecha, rotar, alternar velocidad) encajan con cuatro teclas. Si la imagen no muestra qué tecla es cada una, la correspondencia queda sin probar.
- **Corrección y por qué:** mostrar o indicar en la imagen qué tecla hace cada cosa y añadir al menos las pantallas de Inactivo y Finalizado. ⚠ Verificar contra la imagen.