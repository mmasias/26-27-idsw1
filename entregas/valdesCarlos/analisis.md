# Análisis de incoherencias y decisiones discutibles

## 1. Incoherencias y desajustes

### INCOHERENCIA-01. Diferencia en el nombre de la operación para cambiar la velocidad de caída

**Artefactos afectados:**
- [ProcesoRequisitos.md](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md)
- [iniciarPartida.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/iniciarPartida.puml)
- [alternarVelocidadCaida.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/alternarVelocidadCaida.puml)

**Qué aparece en cada artefacto:**

En la tabla general de casos de uso aparece la operación `alternarVelocidadCaida()`. Además, su diagrama específico contempla dos posibilidades: cambiar de velocidad normal a acelerada y volver de acelerada a normal.

Sin embargo, en el diagrama `iniciarPartida.puml` aparece la operación `acelerarCaida()` como una de las acciones que puede solicitar el jugador.

**Por qué lo considero una incoherencia:**

Los dos nombres no representan exactamente lo mismo. Acelerar la caída implica aumentar la velocidad, mientras que alternarla permite tanto acelerarla como volver a la velocidad normal.

Por tanto, los documentos no están utilizando el mismo nombre ni están dejando claro que se trata de la misma operación.

---

### INCOHERENCIA-02. Número de piezas activas en el juego

**Artefactos afectados:**
- [modeloDelDominio.md](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md)
- [iniciarPartida.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/iniciarPartida.puml)
- [descenderPieza.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/descenderPieza.puml)

**Qué aparece en cada artefacto:**

En el modelo del dominio, la relación entre `Juego` y `Pieza (activas)` se define como una agregación de multiplicidad `1:*`. Esto representa que un juego puede contener varias piezas activas.

Sin embargo, en los casos de uso se habla de una única pieza activa. Por ejemplo, en `iniciarPartida()` se indica que el sistema presenta la primera pieza activa y en `descenderPieza()` se describe cómo desciende la pieza activa y cómo se genera la siguiente.

**Por qué lo considero una incoherencia:**

En Tetris solamente hay una pieza activa que el jugador puede controlar en cada momento. Las piezas anteriores quedan fijadas en el tablero y ya no se consideran activas.

Por eso, la multiplicidad indicada en el modelo del dominio no parece representar correctamente el comportamiento que se describe en los casos de uso.

---

### INCOHERENCIA-03. Falta de claridad sobre los niveles de dificultad

**Artefacto afectado:**
- [modeloDelDominio.md](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md)

**Qué aparece en cada apartado:**

En la identificación inicial de conceptos aparece `Nivel`, definido como la dificultad actual del juego.

Más adelante, en el análisis de responsabilidades, se indica que el nivel puede aumentar y determinar la velocidad. También se decide que no será una entidad independiente, sino una propiedad de `Juego`.

Sin embargo, en el apartado de simplificaciones conscientes aparece la frase «Sin niveles de dificultad».

**Por qué lo considero un desajuste:**

No queda claro si el juego contempla un sistema de niveles que aumenta a medida que avanzamos en la partida o si directamente no existe ningún tipo de nivel.

Una cosa es no permitir seleccionar distintos niveles de dificultad al comenzar y otra diferente es que el juego no tenga niveles de progresión.

Esta diferencia debería quedar aclarada para evitar interpretaciones distintas del funcionamiento del juego.

---

### INCOHERENCIA-04. Diferencias entre la tabla de relaciones y el diagrama del dominio

**Artefactos afectados:**
- [modeloDelDominio.md](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md)
- [ModeloDominio.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/ModeloDominio.puml)

**Qué aparece en cada artefacto:**

En el apartado de refinamiento de relaciones del modelo del dominio se especifican seis relaciones, indicando su tipo y justificación.

Sin embargo, en el diagrama UML aparecen diez relaciones. Algunas de las que aparecen en el diagrama y no están recogidas en la tabla son:

- `Juego - Turno`
- `Pieza - Celda`
- `Turno - Pieza`
- `Turno - Movimiento`

Además, el diagrama UML no muestra las multiplicidades de las relaciones.

**Por qué lo considero un desajuste:**

El texto y el diagrama deberían representar el mismo modelo y mantener el mismo nivel de detalle.

Aunque algunas relaciones aparecen explicadas en el apartado de relaciones naturales, no se terminan de concretar sus multiplicidades y tipos en el refinamiento.

Esto hace que no podamos comprobar fácilmente si todas las relaciones del diagrama se han analizado y justificado correctamente.

---

### INCOHERENCIA-05. Falta de un control para cambiar la velocidad en el prototipo

**Artefactos afectados:**
- [ProcesoRequisitos.md](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md)
- [Imagen del prototipo](https://github.com/puntoReflex/pyTetris/blob/main/images/tetris.png)
- [alternarVelocidadCaida.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/alternarVelocidadCaida.puml)

**Qué aparece en cada artefacto:**

En los requisitos aparece el caso de uso `alternarVelocidadCaida()`, que permite al jugador cambiar entre velocidad normal y acelerada.

En el apartado del prototipo se indican las teclas `4`, `6`, `7` y `9` como controles del juego, pero no se especifica ningún control para cambiar la velocidad de caída.

**Por qué lo considero una incoherencia:**

Se está definiendo una funcionalidad que el jugador puede utilizar, pero no se explica cómo puede ejecutarla a través de la interfaz.

Por tanto, el prototipo no termina de representar todas las interacciones que aparecen en los requisitos.

---

### INCOHERENCIA-06. Falta de información en algunas transiciones de los diagramas de contexto

**Artefactos afectados:**
- [DiagramaDeContexto000.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto000.puml)
- [DiagramaDeContexto001-ActorJugador.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorJugador.puml)
- [DiagramaDeContexto001-ActorTiempo.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorTiempo.puml)

**Qué aparece en cada artefacto:**

En el diagrama de contexto del jugador, algunas transiciones están identificadas mediante operaciones como `iniciarPartida()` y `reiniciarPartida()`.

En el diagrama de contexto del tiempo también aparecen transiciones entre los estados `Inactivo`, `EnCurso` y `Finalizado`. Sin embargo, algunas de ellas no indican qué evento las provoca.

Por otro lado, el diagrama general de estados tampoco identifica los eventos que producen las transiciones.

**Por qué lo considero un desajuste:**

Los diagramas no mantienen el mismo nivel de detalle y no queda claro qué acción provoca algunos cambios de estado.

Esto dificulta entender cuándo cambia el sistema de un estado a otro y si ese cambio depende del jugador, del tiempo o de un proceso interno del propio juego.

---

## 2. Decisiones discutibles

### DECISION DISCUTIBLE-01. Utilizar composición entre Tablero-Celda y Línea-Celda

**Artefactos afectados:**
- [modeloDelDominio.md](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md)
- [ModeloDominio.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/ModeloDominio.puml)

**Qué se decidió:**

Se utiliza composición para representar que `Tablero` está formado por `Celda` y que `Linea` contiene `Celda`.

Además, el propio diagrama indica que `Tablero` está compuesto por `Linea`.

**Por qué considero que es discutible:**

La composición en UML representa una relación fuerte entre un elemento y las partes que lo forman.

El problema es que una misma celda aparece formando parte del tablero y también de una línea. Esto puede provocar que una misma celda tenga dos propietarios mediante composición.

Según la especificación UML, una parte puede pertenecer como máximo a un único compuesto a la vez.

Me parece que se está utilizando la composición para representar agrupaciones que no necesariamente implican una propiedad independiente.



---

### DECISION DISCUTIBLE-02. Incluir Turno como concepto del dominio

**Artefactos afectados:**
- [modeloDelDominio.md](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md)
- [ModeloDominio.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/ModeloDominio.puml)
- [ProcesoRequisitos.md](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md)

**Qué se decidió:**

Se incluye `Turno` como una entidad del dominio y se le asignan las responsabilidades de comenzar y finalizar.

También se relaciona con las piezas y los movimientos.

**Por qué considero que es discutible:**

En Tetris no existe un sistema de turnos como en otros juegos. La pieza desciende continuamente y el jugador puede moverla o rotarla mientras está cayendo.

Aunque se podría considerar que el turno representa el tiempo durante el cual se controla una pieza, esto no queda explicado en la documentación.

Además, ninguno de los casos de uso utiliza directamente el concepto de turno.

Por tanto, no tengo claro qué aporta esta entidad al modelo ni qué comportamiento representa exactamente.

---

### DECISION DISCUTIBLE-03. Considerar Tiempo como un actor externo

**Artefactos afectados:**
- [ProcesoRequisitos.md](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md)
- [DiagramaCasosUso.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaCasosUso.puml)
- [DiagramaDeContexto001-ActorTiempo.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorTiempo.puml)

**Qué se decidió:**

Se identifica `Tiempo` como un actor del sistema. Su responsabilidad es solicitar el descenso automático de las piezas según un intervalo configurado.

**Por qué considero que es discutible:**

Un actor normalmente representa algo externo al sistema que interactúa con él.

En este caso, no queda claro si Tiempo representa un temporizador externo o simplemente el mecanismo interno que controla cuándo debe descender una pieza.

Si es el propio sistema el que controla el tiempo, no tendría demasiado sentido representarlo como un actor externo.

Por eso creo que habría que aclarar mejor dónde se encuentra el límite del sistema.

---

### DECISION DISCUTIBLE-04. Utilizar una única imagen como prototipo para todos los casos de uso

**Artefactos afectados:**
- [ProcesoRequisitos.md](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md)
- [Imagen del prototipo](https://github.com/puntoReflex/pyTetris/blob/main/images/tetris.png)
- [reiniciarPartida.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml)
- [arrancarSistema.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/arrancarSistema.puml)

**Qué se decidió:**

Se utiliza una única imagen del juego en funcionamiento como prototipo y se considera que es suficiente para representar los demás casos de uso.

**Por qué considero que es discutible:**

La imagen permite entender cómo se ve el juego durante una partida, pero no representa todos los estados y comportamientos del sistema.

Por ejemplo, no muestra la pantalla inicial, el mensaje de confirmación para reiniciar una partida ni qué ocurre cuando se termina el juego.

Creo que una única captura puede servir para representar la pantalla principal, pero no es suficiente para mostrar todas las interacciones que se describen en los casos de uso.

---

### DECESION DISCUTIBLE-05. Realizar una validación con expertos sin aportar información sobre ella

**Artefacto afectado:**
- [modeloDelDominio.md](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md)

**Qué se decidió:**

Se incluye un apartado llamado «Validación con expertos del dominio», donde se responden varias preguntas sobre si el modelo representa correctamente el funcionamiento de Tetris.

Todas las respuestas son afirmativas.

**Por qué considero que es discutible:**

No se indica quién ha participado en esa validación, cómo se ha realizado ni qué observaciones se han obtenido.

Por tanto, no podemos saber si realmente se ha consultado a alguien que conozca el dominio o si simplemente se trata de una revisión realizada por los propios autores.

Me parece que una validación debería aportar alguna evidencia de que el modelo ha sido revisado por otras personas.

---

### DECISION DISCUTIBLE-06. Unificar los conceptos Juego y Partida

**Artefacto afectado:**
- [modeloDelDominio.md](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md)

**Qué se decidió:**

Durante la depuración de conceptos se decide utilizar `Juego` en lugar de `Partida`, justificándolo con que partida es un término más específico de los juegos multijugador.

**Por qué considero que es discutible:**

No estoy del todo de acuerdo con esa justificación, ya que también se utiliza el término partida en juegos individuales.

Además, ambos conceptos podrían tener significados diferentes: `Juego` puede representar el propio sistema de Tetris, mientras que `Partida` puede representar una ejecución concreta del juego.

Creo que habría que valorar si realmente interesa mantener ambos conceptos separados, especialmente si en el futuro se quieren gestionar varias partidas o guardar sus resultados.