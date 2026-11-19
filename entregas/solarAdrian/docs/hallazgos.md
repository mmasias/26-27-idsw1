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

### MR-1. El MdD elige "Juego" y los requisitos usan "Partida"

| Artefacto | Qué dice |
|-|-|
| MdD §3, sinónimos | *"Juego vs Partida → **Juego**. Justificación: 'Partida' es más específico para juegos multijugador"* |
| MdD §8, diagrama | La clase central es `Juego` |
| Req, tabla y diagrama de casos de uso | `iniciarPartida()`, `reiniciarPartida()` |
| Req, detalles de casos de uso | *"Jugador solicita iniciar **partida**"*, *"**partida** iniciada"*, *"Sistema limpia estado de **partida**"*, *"**partida** reiniciada"*, *"continuar **partida**"* |

**Qué corrijo:** el MdD. Renombro `Juego` a `Partida` en el §3, en el diagrama y en las tablas.

**Por qué:**
- El MdD se presenta como un *"contrato conceptual"* que debe mantener *"la trazabilidad desde el vocabulario del jugador hasta el código"*. Los requisitos, que son lo que ve el usuario, eligieron *Partida* en todos sus artefactos, y el vocabulario común tiene que ser uno solo.
- La justificación del §3 no se sostiene: una *partida* es cada sesión de juego, haya uno o varios jugadores. *Juego* es el Tetris en sí, el producto. Que se pueda *"iniciar"* y *"reiniciar"* demuestra que el concepto es una partida, porque el Tetris no se inicia ni se reinicia.

### MR-2. El MdD organiza el juego en turnos y los requisitos lo hacen en tiempo real

| Artefacto | Qué dice |
|-|-|
| MdD §1, observaciones | *"Cada **turno** mueves la pieza actual"* |
| MdD §2 y §4 | *"Turno: Ciclo de juego"*; *"Turno: Comenzar, finalizar. ¿Es entidad? **Sí**"* |
| MdD §8, diagrama | `Juego *-- Turno : "se desarrolla en turnos"`, `Turno --> Pieza : "involucra"`, `Turno o-- Movimiento : "permite"` |
| Req, *"Punto de partida"* | Incluye *Turno* entre los conceptos de los que parten los requisitos |
| Req, tabla de actores | *"**Tiempo**: Disparador temporal. Solicita descenso automático de piezas **según intervalo** configurado"* |
| Req, diagrama de contexto del Jugador | `moverPieza()`, `rotarPieza()` y `alternarVelocidadCaida()` son autotransiciones de `EnCurso`, sin límite ni alternancia |
| Req, todos los demás artefactos | Ningún caso de uso, estado, transición ni pantalla menciona turnos |

**Qué corrijo:** el MdD. Elimino `Turno` del diagrama y de las tablas, y quito *Turno* del *"Punto de partida"* de los requisitos.

**Por qué:**
- El Tetris no es un juego por turnos: la pieza baja a ritmo constante pase lo que pase, y el jugador puede moverla y girarla las veces que quiera entre un descenso y otro. Eso es justo lo que modelan los requisitos con el actor **Tiempo** y con autotransiciones sin límite.
- Un "turno" implicaría alternancia (yo muevo, luego el juego responde) o un número de acciones por ciclo, y nada en los requisitos lo recoge.
- El concepto aparece en el punto de partida de los requisitos, pero no se usa en ninguno de ellos. Eso demuestra que no aporta nada para entender el sistema.

### MR-3. "Movimiento" no significa lo mismo en el MdD y en los requisitos

| Artefacto | Qué dice |
|-|-|
| MdD §2 | *"Movimiento: Acción sobre una pieza"*, *"Rotación: **Tipo de movimiento**"*, *"Caída: **Movimiento** vertical"* |
| MdD §3 | *"Rotación/Caída/Desplazamiento → **Movimiento**: Son variantes del mismo concepto"* |
| Req, detalle de `moverPieza()` | *"Jugador solicita mover pieza **(izquierda o derecha)**"*: solo desplazamiento lateral |
| Req, casos de uso | Rotar (`rotarPieza()`), caer (`descenderPieza()`) y caer rápido (`alternarVelocidadCaida()`) son casos de uso **distintos** de mover |

**Qué corrijo:** el MdD. Retiro la unificación del §3 y, como argumento en DD, `Movimiento` como clase.

**Por qué:** los requisitos reflejan cómo habla el jugador: "mover a un lado", "girar", "bajar". Esas tres acciones tienen reglas distintas: una se pide hacia los lados, otra cambia la orientación y otra la dispara el tiempo. Además, con el vocabulario actual, "mover" en los requisitos es solo **una** de las tres variantes de "Movimiento" en el MdD, así que la misma palabra significa cosas distintas según el documento, que es justo lo que el vocabulario común debería evitar.

### MR-4. Varias piezas activas en el MdD y una sola en los requisitos

| Artefacto | Qué dice |
|-|-|
| MdD §7 y §8 | *"Juego - Pieza **(activas)**: Agregación **1:\***"*; `Juego o-- Pieza : "contiene **piezas activas**"` |
| Req, detalle de `iniciarPartida()` | *"Sistema presenta **primera pieza activa**"* |
| Req, detalle de `descenderPieza()` | *"Sistema presenta **pieza activa** en nueva posición"*, *"Sistema presenta **nueva pieza activa**"* |
| Req, detalles de `moverPieza()`, `rotarPieza()` y `alternarVelocidadCaida()` | *"Sistema presenta **pieza activa**…"*, siempre en singular |
| Req, prototipo | El tablero muestra una sola pieza cayendo (la `I` vertical) |

**Qué corrijo:** el MdD (ver también MD-5): `Juego 1 — 0..1 Pieza`, con el rol "pieza activa".

**Por qué:** los 5 detalles de casos de uso y el prototipo coinciden en que hay una única pieza activa, y es la regla del juego. El MdD es el único artefacto que dice lo contrario, y se contradice con sus propias observaciones.

### MR-5. Los requisitos hablan de "piezas fijadas", pero en el MdD las piezas fijadas no existen

| Artefacto | Qué dice |
|-|-|
| Req, detalle de `descenderPieza()` | *"pieza en posición de fondo o **sobre piezas fijadas**"*, *"Sistema presenta tablero actualizado **con pieza fijada**"* |
| MdD §3 | *"Bloque vs **Celda ocupada** → Parte de Pieza. Los bloques **no existen independientemente**"* |
| MdD §8, diagrama | El juego solo contiene piezas **activas**. Lo que queda en el tablero son `Celda`s, y no hay ningún concepto de pieza fijada |

**Qué corrijo:** el detalle de `descenderPieza()`. Cambio *"sobre piezas fijadas"* por *"sobre celdas ocupadas"* y *"con pieza fijada"* por *"con las celdas de la pieza ocupadas"*.

**Por qué:** el MdD ya decidió que lo que persiste en el tablero son celdas ocupadas, y es la decisión correcta. Cuando se elimina una línea, una pieza fijada puede quedar partida en trozos, y ya no es una "pieza" con su tipo y su forma. Hablar de "piezas fijadas" en los requisitos introduce un concepto que el dominio no tiene y que, además, no se sostiene en cuanto se borra la primera línea.

### MR-6. El MdD dice que el juego se pausa, pero los requisitos no permiten pausar

| Artefacto | Qué dice |
|-|-|
| MdD §4, tabla de verbos | *"Juego: Iniciar, terminar, **pausar**, mantener puntuación"* |
| Req, tabla y diagrama de casos de uso | No existe `pausarPartida()` ni `reanudarPartida()` |
| Req, diagrama de estados del sistema | No existe un estado `Pausado` |
| MdD, *"Simplificaciones conscientes"* | No menciona la pausa como simplificación |

**Qué corrijo:** el MdD, añadiendo a *"Simplificaciones conscientes"*: *"Sin pausa: la partida no se puede detener"*.

**Por qué:** es la corrección más barata y deja la decisión explícita, que es para lo que existe esa tabla. Si se prefiere mantener la pausa, la alternativa es añadir `pausarPartida()` y `reanudarPartida()` con un estado `Pausado`. Lo que no puede ser es que el dominio diga que el juego se pausa y que el sistema no lo permita sin decir por qué.

### MR-7. Los requisitos dicen salir del modelo del dominio, pero sus casos de uso no salen de él

| Artefacto | Qué dice |
|-|-|
| Req, *"Punto de partida"* | *"Los requisitos parten del modelo del dominio: Juego, Tablero, Pieza, TipoPieza, Celda, Línea, Movimiento, Turno"* |
| Req, *"Casos de uso"* | *"**Del modelo del dominio surge:**"*, seguido de la tabla de casos de uso |
| Req, tabla de casos de uso | `arrancarSistema()` (el *Sistema* no está en el MdD); `alternarVelocidadCaida()` (la *velocidad* no está en el diagrama del MdD, ver MD-1); `iniciarPartida()` y `reiniciarPartida()` (*Partida* fue descartada en el MdD, ver MR-1) |
| MdD §8, diagrama | `Turno`, `Movimiento`, `Celda` y `Linea` no dan lugar a ningún caso de uso |

**Qué corrijo:** el MdD, aplicando MD-1 (añadir `velocidad`), MR-1 (`Partida`) y MR-2 (quitar `Turno`). Así la frase *"Del modelo del dominio surge"* pasa a ser cierta.

**Por qué:** la trazabilidad del dominio a los requisitos es lo que el propio proyecto dice defender. Hoy esa frase es falsa en los dos sentidos: casos de uso que usan conceptos que el dominio no tiene, y conceptos del dominio de los que no sale ningún caso de uso.

### MR-8. El MdD elige el vocabulario natural en español, y el prototipo y los requisitos no lo respetan

| Artefacto | Qué dice |
|-|-|
| MdD §3 | *"Pieza vs Tetrómino → **Pieza**: 'Pieza' es más natural **en español**"* |
| MdD, *"Lecciones del proceso"* | *"Empezar con el **vocabulario natural**"*, *"¿Lo explicaría así un jugador?"* |
| MdD §2 y §6 | El concepto es *"**Puntuación**: Puntos acumulados"* |
| Req, imagen del prototipo | `Score: 0` |
| Req, detalle de `descenderPieza()` | *"Sistema presenta mensaje de **Game Over**"* |

**Qué corrijo:** el prototipo y el detalle: `Puntuación: 0` y *"mensaje de fin de partida"*.

**Por qué:** el criterio que eligió el dominio para nombrar los conceptos es el vocabulario natural del jugador en español, y es lo primero que ve el usuario. El prototipo, que es justo el artefacto que valida la interacción con el usuario, usa términos en inglés que no aparecen en ningún otro sitio del modelo.