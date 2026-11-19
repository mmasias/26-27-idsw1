# Análisis Global del Modelo UML — `pyTetris`

## 1. Contexto e hipótesis del modelo

Por la simplicidad de los estados y casos de uso en [`modelosUML/DiagramaDeContexto000.puml`](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto000.puml) y [`modelosUML/DiagramaCasosUso.puml`](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaCasosUso.puml), se infiere que el objeto de estudio son las **máquinas arcade originales de Tetris** y no una versión moderna. El sistema carece de pausa, reserva de piezas o caída instantánea, planteando un flujo continuo hasta el *Game Over*. Aunque [`docs/modeloDelDominio.md`](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#L85) menciona *"pausar"* como responsabilidad de `Juego`, la fase de requisitos lo omite, reforzando este enfoque clásico.

Bajo esta premisa, el análisis no cuestiona la ausencia de mecánicas modernas, sino que se centra en las incoherencias reales entre artefactos y en las decisiones de modelado inconsistentes con las propias reglas del repositorio.

---

## 2. Incoherencias globales

Existen tres problemas transversales que afectan a varios artefactos del repositorio original y que se desarrollan aquí para evitar repetirlos en cada ficha individual.

### 2.1. Nomenclatura inconsistente de los estados

En los diagramas de contexto —[`DiagramaDeContexto000.puml`](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto000.puml#L6-L8), [`DiagramaDeContexto001-ActorJugador.puml`](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorJugador.puml#L5-L7) y [`DiagramaDeContexto001-ActorTiempo.puml`](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorTiempo.puml#L5-L7)— los estados del sistema se denominan `Inactivo`, `EnCurso` y `Finalizado`. Sin embargo, al pasar a la documentación de requisitos en [`docs/ProcesoRequisitos.md`](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#L174) y a los siete diagramas de `modelosUML/detalleCasosUso/*.puml`, los mismos estados pasan a escribirse en mayúsculas con guion bajo: `INACTIVO`, `EN_CURSO` y `FINALIZADO`.

Aunque un lector humano puede deducir que se refieren a lo mismo, en UML los identificadores de estado deben mantenerse idénticos entre el nivel de contexto y el nivel de detalle para garantizar la trazabilidad del modelo. Como solución global, **se unifica toda la nomenclatura al formato `Inactivo`, `EnCurso` y `Finalizado`** en todos los `.puml` corregidos, priorizando la convención establecida en los diagramas de contexto.

### 2.2. Conflicto de vocabulario entre Modelo del Dominio y Requisitos (`Juego` vs. `Partida`)

En [`docs/modeloDelDominio.md`](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#L70) se descarta explícitamente el concepto `Partida` en favor de `Juego`, argumentando que *"Partida es más específico para juegos multijugador"*, y posteriormente se define ese documento como el *"contrato conceptual"* para el resto del desarrollo ([línea 283](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#L283)). En consecuencia, [`modelosUML/ModeloDominio.puml`](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/ModeloDominio.puml) omite `Partida`.

Sin embargo, toda la fase de requisitos contradice esa decisión y utiliza `Partida` de forma sistemática: aparece en [`docs/ProcesoRequisitos.md`](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#L32), en [`DiagramaCasosUso.puml`](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaCasosUso.puml#L9-L10), en [`DiagramaDeContexto001-ActorJugador.puml`](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorJugador.puml#L11) y en los detalles de `iniciarPartida.puml` y `reiniciarPartida.puml`.

En lugar de renombrar toda la fase de requisitos, se opta por **corregir el Modelo del Dominio reincorporando `Partida`**. Ambos conceptos no son sinónimos, sino entidades con responsabilidades distintas que encajan con el ciclo de vida del sistema: `Juego` representa la aplicación en ejecución y `Partida` cada sesión concreta que se inicia, se juega y se reinicia ([ver `ModeloDominio.md`](./correcciones/ModeloDominio.md)).

### 2.3. Artefacto sobrante sin código fuente (`acelerarCaida.svg`)

En el repositorio existe la imagen [`images/modelosUML/detalleCasosUso/acelerarCaida.svg`](https://github.com/puntoReflex/pyTetris/blob/main/images/modelosUML/detalleCasosUso/acelerarCaida.svg), pero **no cuenta con un archivo fuente `.puml` asociado** en `modelosUML/detalleCasosUso/`, ni figura en [`DiagramaCasosUso.puml`](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaCasosUso.puml) ni en [`DiagramaDeContexto001-ActorJugador.puml`](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorJugador.puml), donde el caso de uso definido es `alternarVelocidadCaida()`.

Además, el diagrama presenta dos diferencias claras respecto al resto del bloque: funcionalmente solo contempla acelerar la caída (sin mecanismo para volver a la velocidad normal) y, a nivel de estilo, retorna en bucle al mismo estado `EN_CURSO` superior en lugar de duplicarlo al final. Dado que la referencia `acelerarCaida()` también aparece dentro de [`iniciarPartida.puml`](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/iniciarPartida.puml#L32), todo apunta a que se trata de un caso de uso descartado que no llegó a limpiarse por completo del repositorio. La corrección consiste en **eliminar `acelerarCaida.svg`** y sustituir su referencia en `iniciarPartida`.

---

## 3. Índice de correcciones

### Modelo del Dominio

| Artefacto original | Corrección aplicada | Detalle |
| :--- | :--- | :--- |
| `modelosUML/ModeloDominio.puml` | Se añade `Partida` gestionada por `Juego` (`Juego *-- Partida`). | [ModeloDominio.md](./correcciones/ModeloDominio.md) |

### Diagramas de Contexto y de Estado

| Artefacto original | Corrección aplicada | Detalle |
| :--- | :--- | :--- |
| `modelosUML/DiagramaDeContexto000.puml` | Se elimina la terminación automática `Finalizado -> [*]`. | [DiagramaDeContexto000.md](./correcciones/DiagramaDeContexto000.md) |
| `modelosUML/DiagramaDeContexto001-ActorJugador.puml` | Se elimina el reinicio desde `EnCurso`. | [DiagramaDeContexto001-ActorJugador.md](./correcciones/DiagramaDeContexto001-ActorJugador.md) |
| `modelosUML/DiagramaDeContexto001-ActorTiempo.puml` | Se añaden guardas explícitas en `descenderPieza()` y se corrige su firma. | [DiagramaDeContexto001-ActorTiempo.md](./correcciones/DiagramaDeContexto001-ActorTiempo.md) |

### Detalle de Casos de Uso

| Artefacto original | Estado / Corrección aplicada | Detalle |
| :--- | :--- | :--- |
| `arrancarSistema.puml` | Se añade el nodo inicial `[*]` y su transición entrante. | [arrancarSistema.md](./correcciones/detalleCasosUso/arrancarSistema.md) |
| `iniciarPartida.puml` | Se sustituye `acelerarCaida()` por `alternarVelocidadCaida()` y se elimina `reiniciarPartida()`. | [iniciarPartida.md](./correcciones/detalleCasosUso/iniciarPartida.md) |
| `moverPieza.puml` | Se unifica la doble salida redundante hacia `EnCurso`. | [moverPieza.md](./correcciones/detalleCasosUso/moverPieza.md) |
| `rotarPieza.puml` | Se unifica la doble salida redundante hacia `EnCurso`. | [rotarPieza.md](./correcciones/detalleCasosUso/rotarPieza.md) |
| `descenderPieza.puml` | Se añade comprobación condicional antes de `EliminandoLineas`. | [descenderPieza.md](./correcciones/detalleCasosUso/descenderPieza.md) |
| `reiniciarPartida.puml` | Se elimina `EnCurso`, la confirmación y el retorno a `Finalizado`. | [reiniciarPartida.md](./correcciones/detalleCasosUso/reiniciarPartida.md) |
| `alternarVelocidadCaida.puml` | **Correcto desde el inicio** (solo le aplica la unificación global de nombres de estado). | — |
| `acelerarCaida.svg` | **Sobra totalmente** (imagen sin `.puml` ni caso de uso asociado; eliminar). | Sección 2.3 |