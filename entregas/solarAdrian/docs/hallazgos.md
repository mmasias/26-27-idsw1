# Hallazgos

[README](../README.md) · [Plan](plan.md)

Artefactos revisados en el commit [`92c950a`](https://github.com/puntoReflex/pyTetris/tree/92c950ad9746dc768be130f75c988c0f712d5509) de pyTetris:

- **MdD**: [modeloDelDominio.md](https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/docs/modeloDelDominio.md) y su diagrama ([ModeloDominio.puml](https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/modelosUML/ModeloDominio.puml)).
- **Req**: [ProcesoRequisitos.md](https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/docs/ProcesoRequisitos.md), con sus tablas, diagramas, detalles e imagen del prototipo.

Cada hallazgo tiene un código: **MD** (dentro del modelo del dominio), **MR** (entre el modelo del dominio y los requisitos), **RQ** (dentro de los requisitos y el prototipo) y **DD** (decisiones discutibles).

---

# Incoherencias

## Dentro del modelo del dominio

### MD-1. Los atributos que decide el texto no están en el diagrama

| Artefacto | Qué dice |
|-|-|
| MdD §6, *"¿Puntuación y Nivel como entidades?"* | *"Decisión: excluir como entidades - Son **propiedades de Juego**"* |
| MdD, tabla *"Descartados en el proceso"* | *"**Velocidad**: Atributo de Juego, no entidad"* |
| MdD §8, diagrama resultante | `Juego` aparece **sin ningún atributo**. Ninguna clase del diagrama tiene atributos |

**Qué corrijo:** el diagrama. Añado a `Juego` los atributos `puntuación` y `velocidad`.

**Por qué:** el texto tomó la decisión de forma explícita y razonada, y el diagrama simplemente no la refleja. Descartar algo como entidad no significa eliminarlo del modelo, sino moverlo a un atributo. Tal como está, la puntuación ha desaparecido del modelo, aunque el propio MdD la cita en sus primeras observaciones y en la tabla de sustantivos (*"Puntuación: Puntos acumulados"*).

### MD-2. Las multiplicidades del texto no están en el diagrama, y el texto no justifica todas las relaciones

| Artefacto | Qué dice |
|-|-|
| MdD §7, *"Refinamiento de relaciones"* | Tabla con multiplicidades: Juego-Tablero **1:1**, Juego-Pieza **1:\***, Tablero-Celda **1:\***, Tablero-Línea **1:\***, Pieza-TipoPieza **\*:1**, Línea-Celda **1:\*** |
| MdD, *"Evolución del modelo · Versión 2"* | *"Se **refinan multiplicidades**"* |
| MdD *"Versión 3: Modelo final"* | *"Relaciones **claramente tipadas**"* |
| MdD §8, diagrama resultante | **Ninguna relación tiene multiplicidad.** Además hay 4 relaciones que no aparecen en la tabla de §7 ni se justifican en ningún sitio: `Juego *-- Turno`, `Pieza --> Celda`, `Turno --> Pieza` y `Turno o-- Movimiento` |

**Qué corrijo:** el diagrama, añadiendo las multiplicidades ya decididas. También completo la tabla de §7 con las 4 relaciones que faltan, o las elimino del diagrama si no se pueden justificar (ver DD sobre Turno y Movimiento).

**Por qué:** el texto dice que el modelo final tiene las multiplicidades refinadas y las relaciones claramente tipadas, y el diagrama, que es el resultado que se entrega, no lo cumple. Sin multiplicidades no se puede saber, por ejemplo, si el juego tiene una pieza activa o varias (ver MD-5). Que 4 de las 10 relaciones del diagrama no tengan justificación contradice la metodología que el propio documento describe.

### MD-3. Nivel es a la vez propiedad del juego y una simplificación descartada

| Artefacto | Qué dice |
|-|-|
| MdD §4, tabla de verbos | *"Nivel: Aumentar, **determinar velocidad**"* |
| MdD §6 | *"Puntuación y **Nivel**… Son **propiedades de Juego**"* |
| MdD, *"Simplificaciones conscientes"* | *"**Sin niveles** de dificultad: Mantener el modelo esencial"* |

**Qué corrijo:** el §6. Dejo solo Puntuación como propiedad de Juego y explico que Nivel queda fuera por la simplificación.

**Por qué:** la simplificación es la decisión más específica y la que respeta el resto del trabajo: ningún caso de uso, pantalla ni prototipo usa niveles. Tal como está, el lector no sabe si el juego tiene nivel o no. Además, si Nivel *"determina la velocidad"* y no hay niveles, la velocidad queda sin una regla que la determine.

### MD-4. La Línea se define como "completa", pero se usa como cualquier fila

| Artefacto | Qué dice |
|-|-|
| MdD §2, tabla de sustantivos | *"Línea: Fila horizontal **completa**"* |
| MdD §4, tabla de verbos | *"Línea: **Completarse**, eliminarse, contar puntos"* |
| MdD §7 y diagrama | *"Tablero - Línea: Composición 1:\* - Las líneas son **agrupaciones lógicas de celdas del tablero**"*; `Tablero *-- Linea : "se organiza en"` |

**Qué corrijo:** la definición del §2: *"Línea: fila horizontal del tablero; está **completa** cuando todas sus celdas están ocupadas"*.

**Por qué:** si una línea ya es completa por definición, no puede "completarse", y el tablero no podría "organizarse en" líneas, porque al principio no hay ninguna completa. Las relaciones y los verbos usan Línea como cualquier fila del tablero, y "completa" es una condición que la fila cumple o no. Además así queda más claro el paralelismo con *"Columna: fila vertical"*.

### MD-5. El texto habla de "la pieza actual", en singular, y el modelo de "piezas activas", en plural

| Artefacto | Qué dice |
|-|-|
| MdD §1, primeras observaciones | *"Cada turno mueves **la pieza actual**"* |
| MdD §5, relaciones | *"Cada turno involucra **una pieza**"* |
| MdD §7 | *"Juego - Pieza **(activas)**: Agregación **1:\***"* |
| MdD §8, diagrama | `Juego o-- Pieza : "contiene **piezas activas**"` |

**Qué corrijo:** la tabla de §7 y el diagrama: `Juego 1 — 0..1 Pieza`, con el rol "pieza activa".

**Por qué:** las observaciones del propio dominio, que son la fuente según la metodología del documento, hablan de una única pieza en juego, y esa es la regla del Tetris. Es 0..1 y no 1 porque antes de empezar la partida, y entre fijar una pieza y generar la siguiente, no hay ninguna pieza activa. Los requisitos también lo confirman (ver MR).

### MD-6. Turno no se puede "agregar" en la versión 2 si ya estaba en la versión 0

| Artefacto | Qué dice |
|-|-|
| MdD, *"Versión 0: Lista plana"* | *"Juego, Tablero, Pieza, Celda, Línea, Puntuación, Jugador, Movimiento, Nivel, Velocidad, **Turno**…"* |
| MdD, *"Versión 2: Refinamiento"* | *"Se **agregan** TipoPieza y **Turno**"* |
| MdD §2, tabla de sustantivos | *"Turno: Ciclo de juego"* aparece ya en el brainstorming inicial |

**Qué corrijo:** el texto de la Versión 2: *"Se agrega TipoPieza"*. Si Turno se mantiene, la Versión 2 debería decir que se conserva, no que se agrega.

**Por qué:** el apartado "Evolución del modelo" sirve para seguir cómo cambió el modelo, y aquí cuenta una evolución que no ocurrió. Turno estuvo desde el principio, como muestran la Versión 0 y el §2. Es un detalle menor, pero hace dudar de la fiabilidad del resto de la historia del modelo.

### MD-7. Dos grafías para el mismo concepto

| Artefacto | Qué dice |
|-|-|
| MdD §2 | *"**Tetrimino**: Nombre técnico de las piezas"* |
| MdD §3 | *"Pieza vs **Tetrómino**"* |

**Qué corrijo:** el §3, para que use *Tetrimino* igual que el §2. Es el término de las reglas oficiales que el documento dice haber leído.

**Por qué:** es una incoherencia menor, pero el MdD se presenta como el vocabulario común del proyecto, y el vocabulario común no puede tener dos grafías para la misma palabra, aunque sea un término descartado.