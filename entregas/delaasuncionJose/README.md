# Reto 002 — Análisis de pyTetris

## Incoherencias

---

### INC-01 · `acelerarCaida()` vs `alternarVelocidadCaida()`

El detalle de [`iniciarPartida.puml`](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/iniciarPartida.puml) lista como acción disponible **`acelerarCaida()`**. Sin embargo, en la [tabla de casos de uso](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#casos-de-uso), el [diagrama de casos de uso](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaCasosUso.puml), los [diagramas de contexto](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorJugador.puml) y el propio [detalle del caso de uso](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/alternarVelocidadCaida.puml), el nombre es **`alternarVelocidadCaida()`**.

**Corrección:** cambiar `acelerarCaida()` por `alternarVelocidadCaida()` en `iniciarPartida.puml`, ya que es el único artefacto que usa el nombre divergente.

---

### INC-02 · Artefacto huérfano `acelerarCaida.svg`

Existe un archivo [`acelerarCaida.svg`](https://github.com/puntoReflex/pyTetris/blob/main/images/modelosUML/detalleCasosUso/acelerarCaida.svg) en la carpeta de detalles de casos de uso. Es un diagrama de un caso de uso que no existe en la lista oficial de 7 casos de uso. No se referencia desde ningún documento. Es un residuo de una iteración anterior que contradice la lista cerrada de casos de uso.

**Corrección:** eliminar el archivo.

---

### INC-03 · El prototipo muestra dos rotaciones, los casos de uso definen una

El [prototipo](https://github.com/puntoReflex/pyTetris/blob/main/images/tetris.png) muestra `7=rotarσ, 9=rotarv` (dos teclas de rotación, sentido horario y antihorario). Sin embargo, los requisitos definen un único [`rotarPieza()`](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/rotarPieza.puml) sin distinguir dirección.

**Corrección:** o bien el prototipo debería mostrar una sola rotación, o bien `rotarPieza()` debería recibir la dirección como parámetro y reflejarlo en su detalle.

---

### INC-04 · El texto dice que el prototipo cubre todos los casos de uso, pero no es cierto

La [sección de prototipos](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#prototipar-casos-de-uso) afirma que con la imagen del prototipo *«todo está implícito y claro»* y menciona `alternarVelocidadCaida()` y `reiniciarPartida()`. Pero el prototipo **no muestra ninguna tecla** para esos dos casos de uso. No están ni implícitos ni claros.

**Corrección:** el texto no debería afirmar que el prototipo los cubre cuando no lo hace.

---

### INC-05 · `Finalizado → [*]` en el diagrama de estados sin caso de uso que lo soporte

El [diagrama de estados general](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto000.puml) define una transición `Finalizado -> [*]` (terminación del sistema). Pero no existe ningún caso de uso como `cerrarSistema()` que provoque esa transición. Los diagramas de contexto por actor tampoco la explican.

**Corrección:** eliminar esa transición o crear el caso de uso correspondiente.

---

### INC-06 · El diagrama del Jugador tiene `EnCurso → Finalizado` sin etiqueta

En el [diagrama de contexto del Jugador](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorJugador.puml) aparece `EnCurso --> Finalizado` sin etiquetar. Pero esa transición no la provoca el Jugador: según el [diagrama del Tiempo](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorTiempo.puml), la provoca `descenderPieza [no hay espacio]`. Su presencia sin etiqueta en el diagrama del Jugador confunde sobre quién causa el Game Over.

**Corrección:** quitar esa transición del diagrama del Jugador o etiquetarla para aclarar que es consecuencia del Tiempo.

---

### INC-07 · `Turno` y `Movimiento` están en el modelo del dominio pero no aparecen en requisitos

El [modelo del dominio](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/ModeloDominio.puml) define `Turno` y `Movimiento` como entidades con relaciones. Sin embargo, ningún artefacto de [requisitos](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md) los menciona. Los casos de uso funcionan directamente como `moverPieza()`, `rotarPieza()`, etc., sin concepto intermedio de turno ni movimiento.

**Corrección:** o bien simplificar el modelo del dominio eliminándolos, o bien reflejarlos en los requisitos.

---

### INC-08 · `Puntuación` se excluye del dominio pero se usa en requisitos y prototipo

El [modelo del dominio](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#6-decisiones-cr%C3%ADticas-de-modelado) excluye Puntuación como entidad (la considera propiedad de Juego), pero no la muestra como atributo. Sin embargo, [`iniciarPartida.puml`](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/iniciarPartida.puml) dice *«puntuación inicial (0)»*, [`descenderPieza.puml`](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/descenderPieza.puml) dice *«puntuación actualizada»*, y el [prototipo](https://github.com/puntoReflex/pyTetris/blob/main/images/tetris.png) muestra `Score: 0`.

**Corrección:** incluir `puntuación` como atributo visible de `Juego` en el diagrama del dominio.

---

## Decisiones discutibles

---

### DD-01 · Composición doble sobre `Celda`

En el [modelo del dominio](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/ModeloDominio.puml), `Tablero *-- Celda` y `Línea *-- Celda` son ambas composiciones. En UML, la composición implica propiedad exclusiva: una parte no puede pertenecer a dos "todos" a la vez. Cada celda estaría compuesta simultáneamente por el Tablero y por una Línea, lo cual viola la semántica de composición. `Línea` debería usar agregación o asociación con `Celda`.

---

### DD-02 · Unificar rotación, caída y desplazamiento como `Movimiento`

La [tabla de sinónimos](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#3-primera-depuraci%C3%B3n-consolidaci%C3%B3n-de-sin%C3%B3nimos) unifica `Rotación/Caída/Desplazamiento → Movimiento`. Pero los propios requisitos los separan en casos de uso distintos (`moverPieza()`, `rotarPieza()`, `descenderPieza()`) con validaciones diferentes. La unificación en el dominio contradice la separación que luego se necesita.

---

### DD-03 · Un solo prototipo para todos los estados

La [sección de prototipos](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#prototipar-casos-de-uso) usa una única imagen del estado `EN_CURSO`. No se prototipia qué ve el usuario al arrancar (pantalla inicial), al perder (Game Over), ni el diálogo de confirmación de reinicio que el [detalle de `reiniciarPartida()`](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml) describe. Prototipar sirve para hacer explícito lo implícito; con una sola imagen se dejan sin validar varias interacciones.

---

### DD-04 · Confirmación de reinicio como requisito

El detalle de [`reiniciarPartida.puml`](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml) modela un diálogo de confirmación (el sistema pregunta, el jugador confirma o cancela). Esto es un detalle de interfaz de usuario metido en requisitos. El propio proyecto [descarta conceptos como "Preview"](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#descartados-en-el-proceso) por ser *"detalle de interfaz, no del dominio puro"*. Un diálogo de confirmación es igualmente un detalle de UI.

---

### DD-05 · Excluir `Jugador` del modelo del dominio

Se [excluye `Jugador`](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#incluir-jugador) porque *"en Tetris single-player es implícito"*. Sin embargo, toda la [disciplina de requisitos](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#actores) pivota sobre el actor `Jugador`. Si es un concepto lo suficientemente real como para ser actor principal de los requisitos, es un concepto del dominio.

---

### DD-06 · Multiplicidad `1:*` en `Juego o-- Pieza` ("piezas activas")

La [tabla de relaciones](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#7-refinamiento-de-relaciones) define `Juego - Pieza (activas)` como `1:*`. Pero en Tetris clásico solo hay **una** pieza activa a la vez. El proyecto descarta explícitamente preview/siguiente. `1:*` sugiere múltiples piezas activas simultáneas, lo cual no ocurre en este dominio.

---

### DD-07 · Descartar `Nivel` como simplificación

Se [descarta Nivel](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#simplificaciones-conscientes) como *"simplificación consciente"*. Pero la dificultad progresiva (niveles → mayor velocidad) es la mecánica central que da al Tetris su curva de dificultad y su final inevitable. Sin ella, el juego tiene dificultad constante: más que una simplificación, es una alteración fundamental de la mecánica core del dominio.
