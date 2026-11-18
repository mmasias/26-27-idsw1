# Propuestas de corrección de los hallazgos

## 1. Propuestas para las incoherencias y desajustes

### INCOHERENCIA-01. Diferencia en el nombre de la operación para cambiar la velocidad de caída

**Propuesta:**

Cambiar el nombre `acelerarCaida()` que aparece en `iniciarPartida.puml` por `alternarVelocidadCaida()`.

Mantendría este último nombre en todos los documentos, ya que es el que aparece en la tabla general de casos de uso y en su propio diagrama.

**Justificación:**

De esta manera, todos los artefactos hacen referencia a la misma operación y queda claro que el jugador puede tanto acelerar la caída como volver a la velocidad normal.

---

### INCOHERENCIA-02. Número de piezas activas en el juego

**Propuesta:**

Modificar la relación entre `Juego` y `Pieza` para que represente que solamente puede existir una pieza activa en cada momento.

En lugar de utilizar una multiplicidad `1:*`, utilizaría una relación que permita una única pieza activa, por ejemplo, `0..1` durante el ciclo de vida del juego.

También diferenciaría las piezas activas de las piezas que ya están fijadas en el tablero.

**Justificación:**

En Tetris solamente se controla una pieza a la vez. Cuando una pieza termina de descender, queda fijada y aparece una nueva.

Por eso, considero que el modelo del dominio debería representar este comportamiento y no dar a entender que pueden existir varias piezas activas simultáneamente.

---

### INCOHERENCIA-03. Falta de claridad sobre los niveles de dificultad

**Propuesta:**

Aclararía qué significa exactamente el concepto `Nivel` dentro del juego.

Si el juego tiene una progresión automática, mantendría el nivel como una propiedad de `Juego` y modificaría la simplificación para indicar que no existen niveles de dificultad seleccionables, pero sí niveles que aumentan durante la partida.

Si el juego no contempla ningún tipo de progresión por niveles, eliminaría las referencias a `Nivel` de la identificación inicial de conceptos y del análisis de responsabilidades.

**Justificación:**

Así evitamos que dos apartados del mismo documento puedan interpretarse de maneras diferentes.

Además, dejamos claro qué funcionalidades forman parte del juego y cuáles quedan fuera de su alcance.

---

### INCOHERENCIA-04. Diferencias entre la tabla de relaciones y el diagrama del dominio

**Propuesta:**

Actualizaría la tabla de refinamiento de relaciones para incluir todas las relaciones que aparecen en el diagrama UML.

En concreto, añadiría las siguientes:

- `Juego - Turno`
- `Pieza - Celda`
- `Turno - Pieza`
- `Turno - Movimiento`

También completaría la tabla indicando el tipo de relación y su multiplicidad.

Por último, trasladaría esa información al diagrama UML, incluyendo las multiplicidades en los extremos de las asociaciones.

**Justificación:**

El objetivo es que el texto y el diagrama representen exactamente el mismo modelo.

No basta con que aparezcan las relaciones, sino que también debe quedar claro cómo se relacionan los conceptos y cuántos elementos pueden participar en cada relación.

---

### INCOHERENCIA-05. Falta de un control para cambiar la velocidad en el prototipo

**Propuesta:**

Añadiría al prototipo una indicación del control que permite alternar la velocidad de caída de las piezas.

Por ejemplo, se podría establecer una tecla específica para esta operación e incluirla en la leyenda de controles.

También actualizaría el caso de uso para que quede claro qué acción realiza el jugador.

**Justificación:**

De esta forma, el prototipo representaría una funcionalidad que ya está definida en los requisitos.

Además, cualquier persona que consulte la interfaz podría entender cómo utilizar todas las operaciones disponibles.

---

### INCOHERENCIA-06. Falta de información en algunas transiciones de los diagramas de contexto

**Propuesta:**

Añadiría etiquetas a las transiciones que actualmente no indican qué evento las provoca.

Por ejemplo:

- `Inactivo -> EnCurso`: `iniciarPartida()`.
- `EnCurso -> Finalizado`: `descenderPieza()` cuando no hay espacio para una nueva pieza.
- `Finalizado -> Inactivo`: `reiniciarPartida()`.

También mantendría el mismo criterio en los diagramas de contexto del jugador, del tiempo y en el diagrama general de estados.

**Justificación:**

Así quedaría más claro qué provoca cada cambio de estado y qué actor o proceso interviene.

También conseguiríamos que los diagramas mantuvieran un nivel de detalle más uniforme.

---

## 2. Propuestas para las decisiones discutibles

### DECISIONES DISCUTIBLES-01. Utilizar composición entre Tablero-Celda y Línea-Celda

**Propuesta:**

Cambiaría la estructura de composición para evitar que una misma celda tenga dos propietarios mediante composición.

Mi propuesta sería mantener `Tablero` como propietario de las celdas y representar la relación entre `Linea` y `Celda` mediante una asociación normal, ya que una línea puede entenderse como una agrupación lógica de las celdas que forman una fila.

De esta manera, mantendría:

- `Tablero *-- Celda`
- `Tablero *-- Linea`
- `Linea --> Celda`

**Justificación:**

La composición debería representar una relación de propiedad entre un elemento y sus partes.

Las celdas pertenecen al tablero, mientras que las líneas representan agrupaciones de esas celdas.

Así evitamos duplicar la propiedad de una misma celda y mantenemos una estructura más clara.

---

### DECISIONES DISCUTIBLES-02. Incluir Turno como concepto del dominio

**Propuesta:**

Primero definiría qué representa exactamente un turno dentro de Tetris.

Una posibilidad sería considerar que un turno empieza cuando aparece una nueva pieza y termina cuando esa pieza queda fijada en el tablero.

En ese caso, mantendría `Turno` en el modelo del dominio y actualizaría la documentación para explicar sus responsabilidades y su relación con las piezas y los movimientos.

Si no se necesita representar ese ciclo de ninguna manera, directamente eliminaría `Turno` del modelo.

**Justificación:**

Considero que no tiene sentido mantener una entidad si no tenemos claro qué representa ni qué responsabilidad tiene dentro del sistema.

Definiendo su función podríamos decidir si realmente aporta algo al modelo o si simplemente estamos añadiendo un concepto innecesario.

---

### DECISIONES DISCUTIBLES-03. Considerar Tiempo como un actor externo

**Propuesta:**

Aclararía primero cómo funciona el control temporal del juego.

Si existe un temporizador externo que solicita el descenso de las piezas, mantendría el actor, pero lo llamaría `Temporizador` para que su función sea más concreta.

En cambio, si el control del tiempo forma parte del propio sistema, eliminaría `Tiempo` como actor y representaría el descenso automático como un evento interno del juego.

**Justificación:**

Los actores deben representar elementos externos que interactúan con el sistema.

Por eso, considero importante diferenciar entre un temporizador externo y un mecanismo interno que simplemente controla cuándo debe descender una pieza.

---

### DECISIONES DISCUTIBLES-04. Utilizar una única imagen como prototipo para todos los casos de uso

**Propuesta:**

Mantendría la imagen actual como representación de una partida en funcionamiento, pero añadiría algunos prototipos sencillos para representar los casos de uso que implican cambios importantes en la interfaz.

Por ejemplo:

- Una pantalla inicial para `arrancarSistema()`.
- Una pantalla de confirmación para `reiniciarPartida()`, mostrando las opciones de aceptar o cancelar.
- Una representación del estado de partida finalizada.
- Una indicación visual del cambio de velocidad de caída.

No sería necesario crear una imagen para cada movimiento, pero sí para aquellos comportamientos que no se pueden apreciar en la captura actual.

**Justificación:**

Creo que los prototipos sirven para entender cómo interactúa el usuario con el sistema.

Una única imagen puede explicar perfectamente la pantalla principal, pero no permite representar todas las situaciones que aparecen en los casos de uso.

---

### DECISIONES DISCUTIBLES-05. Realizar una validación con expertos sin aportar información sobre ella

**Propuesta:**

Ampliaría el apartado de validación indicando cómo se ha realizado.

Incluiría información como:

- Qué personas o perfiles han participado.
- Qué aspectos del modelo se han revisado.
- Qué observaciones se han realizado.
- Qué cambios se han llevado a cabo después de la revisión.

En caso de que no se haya realizado ninguna validación externa, cambiaría el nombre del apartado a `Autoevaluación del modelo`.

**Justificación:**

Me parece importante diferenciar entre una revisión realizada por los propios autores y una validación realizada por otras personas que conocen el dominio.

De esta forma, el documento sería más claro y permitiría comprobar cómo se ha revisado el modelo.

---

### DECISIONES DICUTIBLES-06. Unificar los conceptos Juego y Partida

**Propuesta:**

Revisaría si realmente interesa mantener ambos conceptos diferenciados.

Una posible solución sería utilizar:

- `Juego` para representar el propio sistema de Tetris.
- `Partida` para representar cada ejecución concreta del juego.

Si el objetivo es únicamente representar una partida sencilla de Tetris, también sería válido mantener un único concepto, pero utilizaría una justificación más adecuada para explicar por qué se han unido.

**Justificación:**

Considero que `Juego` y `Partida` pueden representar cosas diferentes.

Separarlos permitiría, por ejemplo, gestionar varias partidas, guardar sus resultados o consultar el historial de puntuaciones.

De todas formas, si esas funcionalidades quedan fuera del alcance del proyecto, se podría mantener un único concepto para simplificar el modelo.