# Reto 002 - Auditoría de Modelado y Requisitos (`pyTetris`)

Revisión crítica del repositorio [pyTetris](https://github.com/puntoReflex/pyTetris) enfocada en detectar **incoherencias** entre artefactos y **decisiones discutibles** de diseño en el modelo del dominio y la especificación de requisitos.

---

## 1. Incoherencias

*Artefactos que afirman cosas distintas o contradictorias sobre un mismo elemento del sistema.*

### 1.1. Nombre de la acción de cambio de velocidad
* **Dónde están:** 
  * [ProcesoRequisitos.md (Lista de casos de uso)](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#casos-de-uso)
  * [iniciarPartida.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/iniciarPartida.puml)
* **Qué dice cada uno:** El listado de requisitos define el caso de uso `alternarVelocidadCaida()`, mientras que el detalle de inicio de partida invoca `acelerarCaida()`.
* **Corrección propuesta:** Unificar ambos artefactos bajo `alternarVelocidadCaida()`.
* **Por qué:** «Acelerar» implica un incremento puntual o progresivo, mientras que «alternar» refleja fielmente el comportamiento real del sistema (conmutar entre velocidad normal y caída rápida).

### 1.2. Existencia de niveles de dificultad
* **Dónde están:** 
  * [modeloDelDominio.md — Sección *Identificación de conceptos*](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md)
  * [modeloDelDominio.md — Sección *Simplificaciones / Decisiones de modelado*](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md)
* **Qué dice cada uno:** En la identificación inicial se incluye el concepto `Nivel` asociado a la dificultad actual de la partida, pero posteriormente en las simplificaciones se indica que el juego no cuenta con niveles de dificultad.
* **Corrección propuesta:** Eliminar el concepto `Nivel` de la identificación inicial (o bien eliminar la simplificación si se decide implementar progresión).
* **Por qué:** El modelo del dominio resultante no debe arrastrar conceptos descartados explícitamente en el alcance del sistema.

### 1.3. Doble propiedad por composición sobre `Celda`
* **Dónde están:** 
  * [modeloDelDominio.md — *Tabla de relaciones*](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md)
  * [ModeloDominio.svg](https://github.com/puntoReflex/pyTetris/blob/main/images/modelosUML/ModeloDominio.svg)
* **Qué dice cada uno:** Se establece simultáneamente que `Tablero` está compuesto (rombo negro) por `Celda` y que `Linea` también está compuesta por las mismas instancias de `Celda`.
* **Corrección propuesta:** Mantener la composición únicamente entre `Tablero` y `Linea`, y de `Linea` a `Celda` (o usar agregación/asociación simple desde `Linea`).
* **Por qué:** En UML, la composición fuerte implica propiedad exclusiva y ciclo de vida dependiente; una misma instancia de `Celda` no puede pertenecer por composición a dos contenedores distintos a la vez.

### 1.4. Flujo de reinicio de partida (directo vs. con confirmación)
* **Dónde están:** 
  * [DiagramaDeContexto001-ActorJugador.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorJugador.puml)
  * [reiniciarPartida.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml)
* **Qué dice cada uno:** El diagrama de contexto muestra una transición directa del estado `EnCurso` a `Inactivo` al ejecutar `reiniciarPartida()`. En cambio, el detalle del caso de uso introduce un paso intermedio de confirmación que permite cancelar la operación.
* **Corrección propuesta:** Actualizar el diagrama de contexto incorporando una guarda `[reinicio confirmado]` en la transición hacia `Inactivo` y una autotransición en `EnCurso` para `[reinicio cancelado]`.
* **Por qué:** El diagrama de alto nivel debe reflejar todas las salidas posibles detalladas en la especificación del caso de uso.

### 1.5. Orden de validación y movimiento en el descenso frente al desplazamiento lateral
* **Dónde están:** 
  * [descenderPieza.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/descenderPieza.puml)
  * [moverPieza.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/moverPieza.puml)
* **Qué dice cada uno:** En `moverPieza()` primero se verifica si la posición destino es válida antes de mover; en `descenderPieza()` primero se actualiza/muestra la pieza en la nueva posición y después se comprueba si hay colisión o fondo.
* **Corrección propuesta:** Corregir `descenderPieza.puml` para validar la colisión *antes* de desplazar la pieza.
* **Por qué:** Mover primero y validar después rompe las invariantes del tablero (permitiendo solapamientos temporales) y contradice el patrón de diseño aplicado en el resto de movimientos.

### 1.6. Responsabilidad de pausa sin caso de uso asociado
* **Dónde están:** 
  * [modeloDelDominio.md — *Tabla de responsabilidades*](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md)
  * [DiagramaCasosUso.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaCasosUso.puml) y [ProcesoRequisitos.md](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#casos-de-uso)
* **Qué dice cada uno:** El modelo del dominio asigna al sistema la responsabilidad de pausar la partida, pero ningún caso de uso ni diagrama de requisitos contempla esta funcionalidad.
* **Corrección propuesta:** Añadir el caso de uso `pausarPartida()` (y su estado asociado) en requisitos o eliminar dicha responsabilidad del dominio.
* **Por qué:** Todo comportamiento asignado en el dominio debe tener trazabilidad hacia los requisitos funcionales del sistema.

### 1.7. Rotación doble en el prototipo frente a rotación única en requisitos
* **Dónde están:** 
  * [tetris.png (Prototipo)](https://github.com/puntoReflex/pyTetris/blob/main/images/tetris.png)
  * [rotarPieza.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/rotarPieza.puml)
* **Qué dice cada uno:** La interfaz prototipada diferencia dos comandos de giro (`7=rotarS` y `9=rotarU`), mientras que la especificación de `rotarPieza()` describe una única operación genérica sin distinguir sentido horario o antihorario.
* **Corrección propuesta:** Documentar en `rotarPieza.puml` el parámetro o bifurcación de sentido de giro (`horario` / `antihorario`).
* **Por qué:** El prototipo de interfaz y el contrato del caso de uso deben acordar las mismas entradas de usuario.

---

## 2. Decisiones discutibles

*Decisiones coherentes entre artefactos, pero cuestionables desde el punto de vista del análisis y diseño de software.*

### 2.1. Multiplicidad de piezas activas en el tablero
* **Qué se decidió:** Permitir que un juego tenga múltiples piezas activas simultáneamente ([modeloDelDominio.md](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md)).
* **Qué se propone:** Distinguir explícitamente entre `piezaActiva` (multiplicidad `0..1`, la que cae) y `piezasFijadas` (o celdas ocupadas en el tablero).
* **Argumento:** En Tetris solo existe una pieza controlable en caída libre en cada instante; permitir `0..*` piezas activas debilita las reglas del negocio en el modelo.

### 2.2. Inclusión del concepto `Turno`
* **Qué se decidió:** Modelar la clase `Turno` vinculada a piezas y movimientos ([ModeloDominio.svg](https://github.com/puntoReflex/pyTetris/blob/main/images/modelosUML/ModeloDominio.svg)).
* **Qué se propone:** Eliminar la clase `Turno` del modelo del dominio.
* **Argumento:** El concepto de turno pertenece a juegos secuenciales discretos (ajedrez, juegos de cartas). Tetris es un juego en tiempo real continuo; usar «turno» para referirse al ciclo de vida de una pieza introduce ruido semántico innecesario.

### 2.3. Modelar `TipoPieza` como clase independiente
* **Qué se decidió:** Crear una clase `TipoPieza` separada de `Pieza` ([modeloDelDominio.md](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md)).
* **Qué se propone:** Sustituirla por un tipo enumerado (`enum TipoPieza { I, O, T, S, Z, J, L }`).
* **Argumento:** Si las formas de los tetrominós son fijas y no tienen comportamiento dinámico ni atributos variables en tiempo de ejecución, una clase completa sobrecomplica el modelo frente a un enumerado.

### 2.4. El `Tiempo` como actor externo
* **Qué se decidió:** Representar al `Tiempo` como un actor que dispara `descenderPieza()` ([ProcesoRequisitos.md](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#actores)).
* **Qué se propone:** Eliminar el actor `Tiempo` y gestionar el descenso automático como un evento interno del propio sistema.
* **Argumento:** Según la guía de casos de uso (Larman / Cockburn), el reloj del sistema solo debe modelarse como actor externo cuando existe un servicio de temporización externo real o hitos de calendario empresariales (ej. cierres fiscales), no para bucles internos de un videojuego.

### 2.5. Nomenclatura de casos de uso con sintaxis de métodos (`nombre()`)
* **Qué se decidió:** Nombrar los casos de uso en camelCase con paréntesis (`moverPieza()`, `rotarPieza()`) en [DiagramaCasosUso.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaCasosUso.puml).
* **Qué se propone:** Utilizar frases verbales en lenguaje natural (`Mover pieza`, `Rotar pieza`).
* **Argumento:** Los casos de uso pertenecen a la fase de análisis de requisitos (vista del usuario/negocio), no al diseño de clases. La notación de métodos acopla prematuramente los requisitos a la implementación.

### 2.6. Uso de máquinas de estados como «Diagramas de contexto»
* **Qué se decidió:** Llamar *Diagramas de contexto* a diagramas que modelan exclusivamente estados del sistema (`Inactivo`, `EnCurso`, `Finalizado`) en [ProcesoRequisitos.md](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#presentar-el-modelo-de-casos-de-uso-como-un-todo).
* **Qué se propone:** Renombrarlos como *Diagramas de estados de contexto* o complementar el documento con un diagrama de contexto estructural real (sistema como caja negra + actores + flujos de entrada/salida).
* **Argumento:** Un diagrama de contexto clásico delimita fronteras e interfaces externas, mientras que los artefactos presentados describen el ciclo de vida interno del sistema.

### 2.7. Omisión de multiplicidades y dimensiones en el diagrama final del dominio
* **Qué se decidió:** Documentar las multiplicidades en una tabla de texto y las dimensiones del tablero (`10x20`) en las notas previas, omitiéndolas en [ModeloDominio.svg](https://github.com/puntoReflex/pyTetris/blob/main/images/modelosUML/ModeloDominio.svg).
* **Qué se propone:** Incorporar las multiplicidades directamente en los extremos de las asociaciones del diagrama UML y reflejar las dimensiones como atributos/invariantes de `Tablero`.
* **Argumento:** Un diagrama UML debe ser autocontenido; obligar al lector a cruzar el gráfico con tablas externas reduce su legibilidad y aumenta el riesgo de desincronización.

### 2.8. Ausencia de guardas explícitas en bifurcaciones de casos de uso
* **Qué se decidió:** Dejar sin etiquetas las ramas alternativas en [moverPieza.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/moverPieza.puml), [rotarPieza.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/rotarPieza.puml) y [reiniciarPartida.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml).
* **Qué se propone:** Añadir condiciones de guarda explícitas entre corchetes (`[posición válida]`, `[colisión]`, `[confirmado]`) en cada transición saliente de los nodos de decisión.
* **Argumento:** Los nodos de decisión sin guardas producen diagramas ambiguos donde no es posible verificar formalmente qué flujo corresponde a cada condición de negocio.

