# Reto 002 - Análisis de pyTetris

## 1. Incoherencias entre artefactos

### 1.1. Juego o Partida

En la consolidación de sinónimos se decide quedarse con **Juego** y descartar Partida, con el argumento de que "Partida" es más específico para juegos multijugador. Sin embargo, en los requisitos se usa siempre "partida": los casos de uso son `iniciarPartida()` y `reiniciarPartida()`, el actor Jugador "solicita gestión de partida" y los detalles hablan de "partida iniciada" o "continuar partida".

**Qué propongo:** usar Partida en el modelo del dominio. Es la palabra del jugador y la que ya usan los requisitos. Además, el argumento no se sostiene: una partida en solitario también es una partida. Juego sería el Tetris en general, y lo que empieza y termina es una partida.

### 1.2. `acelerarCaida()` y `alternarVelocidadCaida()`

Al terminar `iniciarPartida()`, el sistema "permite solicitar" `acelerarCaida()`, un caso de uso que no está en la lista ni en el diagrama de casos de uso; el que existe es `alternarVelocidadCaida()`. Además, en la carpeta de imágenes sigue habiendo un `acelerarCaida.svg` que no tiene su `.puml`, y el texto del prototipo dice que con este caso de uso "la pieza cae más rápido", cuando el detalle explica que alterna entre velocidad normal y acelerada.

**Qué propongo:** cambiar `acelerarCaida()` por `alternarVelocidadCaida()` en el detalle de iniciarPartida, borrar el SVG que ya no corresponde a nada y corregir el texto del prototipo para que diga que se alterna entre las dos velocidades. Me quedo con "alternar" porque es el único que tiene su fuente PlantUML y coincide con el diagrama de casos de uso.

### 1.3. Puntuación y velocidad: atributos que no aparecen

El texto decide que Puntuación no es una entidad porque es una "propiedad de Juego", y en los descartados se dice lo mismo de Velocidad ("atributo de Juego"). Pero en el diagrama la clase Juego no tiene ningún atributo. Mientras tanto, los requisitos sí los usan: "presenta puntuación inicial (0)", "presenta puntuación actualizada", velocidad normal y acelerada, y "Score: 0" en el prototipo.

**Qué propongo:** añadir `puntuacion` y `velocidadCaida` como atributos de Partida. Si el texto dice que son atributos, el diagrama tiene que mostrarlos; si no, lo que presenta el sistema no se puede relacionar con nada del modelo.

### 1.4. Nivel: incluido y descartado a la vez

En el apartado 6 se agrupa Nivel con Puntuación y se concluye que los dos son "propiedades de Juego", es decir, que Nivel se queda como atributo. Sin embargo, en las simplificaciones conscientes aparece "Sin niveles de dificultad". No puede ser las dos cosas.

**Qué propongo:** dejar Nivel fuera y quitarlo de la decisión del apartado 6. La simplificación es la que encaja con los requisitos, porque ningún caso de uso cambia de nivel.

### 1.5. Turno está en el modelo pero no en los requisitos

Turno tiene tres relaciones en el diagrama (con Juego, con Pieza y con Movimiento), y los requisitos lo citan en el "punto de partida". Pero después ningún caso de uso, estado ni detalle lo menciona: quien marca el ritmo es el actor Tiempo con `descenderPieza()`. Además, la versión 0 ya incluye Turno en la lista, y la versión 2 dice "se agregan TipoPieza y Turno", como si no hubiera estado antes.

**Qué propongo:** quitar Turno del modelo (lo argumento en el punto 2.3) y corregir el historial de versiones.

### 1.6. Movimiento une lo que los requisitos separan

En el modelo se unifican Rotación, Caída y Desplazamiento en un único concepto, Movimiento, porque "son variantes del mismo concepto". En los requisitos, en cambio, son tres casos de uso distintos (`moverPieza()`, `rotarPieza()` y `descenderPieza()`), con actores distintos, y "mover" solo significa desplazar a izquierda o derecha.

**Qué propongo:** quitar Movimiento del modelo del dominio. Las acciones sobre la pieza ya están recogidas en los casos de uso, y así "mover" significa lo mismo en todos los documentos.

### 1.7. Multiplicidades que se anuncian pero no se dibujan

El apartado 7 se titula "Análisis de multiplicidades y tipos" y la versión 2 dice que "se refinan multiplicidades", pero en el diagrama final no hay ninguna. Además, la tabla del apartado 7 justifica 6 relaciones y el diagrama tiene 10: Juego–Turno, Pieza–Celda, Turno–Pieza y Turno–Movimiento no se explican en ningún sitio.

**Qué propongo:** poner las multiplicidades en los dos extremos de cada relación y justificar las cuatro relaciones que faltan, o quitarlas si no se pueden justificar.

### 1.8. Una o varias piezas activas

En el modelo, Juego "contiene piezas activas" con multiplicidad 1 a muchos. En todos los detalles se habla siempre en singular: "presenta primera pieza activa", "presenta la pieza activa en nueva posición", "presenta nueva pieza activa".

**Qué propongo:** que una partida tenga como máximo una pieza activa (0..1, porque cuando la partida ha terminado no hay ninguna). Es lo que dicen los detalles y lo que pasa en el juego.

### 1.9. Los estados se llaman distinto según el documento

En los diagramas de contexto los estados son `Inactivo`, `EnCurso` y `Finalizado`; en los detalles, `INACTIVO`, `EN_CURSO` y `FINALIZADO`. Pasa algo parecido con el vocabulario: el modelo y los requisitos dicen "puntuación", pero el prototipo muestra "Score", y el detalle de descenderPieza habla de "Game Over" en lugar de partida finalizada.

**Qué propongo:** usar un único nombre para cada cosa en todos los artefactos. El modelo del dominio debería servir de glosario, y si cada documento usa su propio nombre el lector tiene que adivinar que se refieren a lo mismo.

### 1.10. Transiciones que no provoca ningún caso de uso

En un diagrama de contexto las transiciones son los casos de uso. Sin embargo, en el diagrama general hay una salida de `Finalizado` al estado final, y no existe ningún caso de uso para salir del programa. En el del Tiempo aparecen `Inactivo → EnCurso` y `Finalizado → Inactivo` sin etiqueta, aunque esas transiciones no las provoca el Tiempo sino el Jugador.

**Qué propongo:** añadir un caso de uso "salir" para el Jugador, que haría pareja con `arrancarSistema()`, y en cada diagrama de actor dejar solo sus transiciones o marcar claramente las que provoca otro actor.

### 1.11. El prototipo no cubre lo que dice cubrir

El texto dice que la imagen muestra "cómo interactúa el usuario (teclas 4, 6, 7, 9)" y termina diciendo que "todo está implícito y claro con esa única imagen". Pero esas cuatro teclas solo sirven para mover y rotar: no hay forma de iniciar, reiniciar ni cambiar la velocidad, que son la mayoría de casos de uso del Jugador. Además, el propio texto reconoce que la imagen es del estado EN_CURSO, cuando los detalles piden presentar una pantalla inicial, una confirmación de reinicio y un final de partida que no aparecen en ningún sitio.

**Qué propongo:** completar la línea de comandos con todas las acciones del Jugador y añadir un prototipo sencillo del estado inactivo, de la confirmación de reinicio y de la partida finalizada.

### 1.12. Rotar en dos sentidos

El prototipo tiene dos teclas para rotar, una en cada sentido (7 y 9). El detalle de `rotarPieza()` solo dice "Jugador solicita rotar pieza", sin indicar el sentido, mientras que el de `moverPieza()` sí especifica "izquierda o derecha".

**Qué propongo:** que el detalle de rotarPieza diga "solicita rotar pieza (sentido horario o antihorario)", igual que se hace con mover.

### 1.13. La validación dice que no falta ni sobra nada

En la validación se responde que no falta ningún concepto esencial y que no sobra ninguno, y en la conclusión se dice que el modelo "es completo". Sin embargo, faltan la puntuación y la velocidad (punto 1.3) y el estado de la partida (inactiva, en curso o finalizada), que los requisitos sí usan, y sobran Turno y Movimiento, que no usa nadie (puntos 1.5 y 1.6).

**Qué propongo:** repetir la validación después de corregir lo anterior. Tal como está, responde por un modelo que no cumple lo que dice.

### 1.14. Pausar aparece en el modelo pero no en los requisitos

Entre los verbos de Juego están "iniciar, terminar, pausar". Los requisitos presentan su tabla de casos de uso como lo que "surge" del modelo del dominio, pero no hay ningún caso de uso para pausar ni para terminar la partida voluntariamente. Y al revés, `arrancarSistema()` y `alternarVelocidadCaida()` no salen de ningún verbo del modelo.

**Qué propongo:** decidir si pausar entra o no. Si entra, añadir el caso de uso y un estado de partida pausada; si no, quitarlo del apartado 4. En cualquier caso, la tabla de requisitos no debería decir que surge del modelo si no es así.

### 1.15. Línea se define de una forma y se usa de otra

En la identificación de conceptos, Línea es una "fila horizontal completa". Sin embargo, en el diagrama el tablero "se organiza en" líneas, lo que incluye todas las filas, completas o no, y en el apartado 7 se describen como "agrupaciones lógicas de celdas". Además, Columna se define como "fila vertical", mezclando los dos términos.

**Qué propongo:** llamar Fila a cada una de las filas del tablero y dejar "línea completa" como la situación de una fila que hace que se elimine y sume puntos.

## 2. Decisiones discutibles

Son decisiones que encajan con el resto de la documentación, pero que en mi opinión deberían haberse tomado de otra forma.

### 2.1. No incluir al Jugador en el modelo del dominio

Se excluye Jugador porque "el sistema no necesita modelar múltiples jugadores". Creo que el criterio no es correcto: en el modelo del dominio se recogen los conceptos que usa la gente del dominio, no se decide en función de cuántos necesita el sistema. Que solo haya un jugador no hace que deje de ser un concepto, y aparece desde las primeras observaciones hasta en la propia pregunta de validación ("¿Un jugador de Tetris reconoce...?").

**Qué propongo:** incluir Jugador con una asociación "juega" hacia Partida. Así el modelo también deja claro quién es el actor principal de los requisitos.

### 2.2. Celda pertenece por composición a dos sitios

El modelo dice que el tablero está compuesto por celdas y que la línea también está compuesta por celdas. En una composición, la parte pertenece a un único todo y desaparece con él, así que una celda no puede tener dos.

**Qué propongo:** hacer una cadena: el tablero se compone de 20 filas y cada fila de 10 celdas. Así se mantiene que el tablero está formado por celdas y además queda reflejado el tamaño de 10×20 que se menciona al principio.

### 2.3. Turno y Movimiento como clases

El Tetris no funciona por turnos: la pieza baja aunque el jugador no haga nada, y eso es justo lo que representan el actor Tiempo y `descenderPieza()`. La idea de turno parece venir de la frase inicial "cada turno mueves la pieza actual", no del juego. Movimiento, por su parte, es una acción del jugador y no un concepto con información propia; es un verbo convertido en clase.

**Qué propongo:** eliminar las dos clases. Las acciones ya están cubiertas por los casos de uso de mover, rotar y descender.

### 2.4. Descartar Bloque

Bloque se descarta porque "los bloques no existen independientemente" de la pieza. Pero cuando una pieza se fija y después se eliminan filas, en el tablero quedan bloques sueltos: una pieza a la que le quitan dos de sus cuatro celdas ya no es una pieza. En el prototipo se ve que cada celda ocupada muestra la letra de su tipo (L, T, O, I), y con el modelo actual esa información solo se puede sacar de una pieza que ya no debería existir.

**Qué propongo:** incluir Bloque. Una pieza estaría formada por 4 bloques, cada bloque ocuparía una celda y conservaría su tipo cuando la pieza se fija.

### 2.5. Agregación entre Juego y Pieza

La relación se modela como agregación con el argumento de que las piezas "pueden existir conceptualmente sin un juego". No lo veo así: la pieza que está cayendo no existe fuera de una partida. Además, no es una relación de todo y parte, porque la partida no está formada por piezas, sino que en cada momento tiene una activa. Curiosamente, para descartar Bloque se usa el argumento contrario.

**Qué propongo:** una asociación simple "tiene como activa" entre Partida y Pieza, con multiplicidad 0..1.

### 2.6. TipoPieza como clase sin contenido

TipoPieza se incluye precisamente porque "hay exactamente 7 tipos bien definidos", pero en el diagrama aparece como una clase vacía y no se ve cuáles son.

**Qué propongo:** representarlo como un enumerado con los valores I, O, T, S, Z, J y L. Haría lo mismo con el estado de la partida y con la velocidad de caída, que también son listas cerradas de valores.

### 2.7. Asociaciones con dirección en el modelo del dominio

Algunas relaciones tienen flecha (`Pieza --> TipoPieza`, `Pieza --> Celda`, `Turno --> Pieza`). Indicar qué clase conoce a cuál es una decisión de diseño, y el propio documento dice que el modelo debe ser "neutral respecto a la implementación".

**Qué propongo:** usar asociaciones sin dirección, con el nombre del verbo y el sentido de lectura, y con sus multiplicidades.

### 2.8. Descartar la pieza siguiente por ser interfaz

Se descarta "Preview/Siguiente" como un "detalle de interfaz". Creo que se confunde el concepto con su representación: saber qué pieza viene después es información del juego con la que el jugador planifica, igual que la puntuación. Lo que es interfaz es la forma de mostrarla.

**Qué propongo:** si no se quiere incluir, pasarlo a las simplificaciones conscientes, junto a los power-ups o el multijugador. Si se incluye, sería una segunda relación entre Partida y Pieza ("siguiente") y habría que mostrarla en el prototipo.

### 2.9. Casos de uso con nombre de función

Todos los casos de uso tienen forma de operación, con paréntesis: `moverPieza()`, `iniciarPartida()`... Un caso de uso es un objetivo del actor, no una función de una clase, y el documento quiere que los requisitos estén "sin contaminación de decisiones de implementación".

**Qué propongo:** nombrarlos con verbo y objeto en lenguaje del jugador, como "Mover pieza" o "Iniciar partida". Como etiqueta en los diagramas de contexto puede servir el formato corto, pero en la lista y en el diagrama de casos de uso no.

### 2.10. Lenguaje de interfaz e implementación en los detalles

En los detalles aparecen expresiones como "presenta pantalla inicial", "presenta mensaje de confirmación" o "Sistema limpia estado de partida". "Pantalla" y "mensaje" son interfaz, y "limpia estado" describe lo que hace el sistema por dentro, que el jugador no observa.

**Qué propongo:** redactarlos como vimos en clase, donde el sistema solo permite introducir, permite solicitar o presenta información. Por ejemplo, "permite confirmar o cancelar el reinicio" o "presenta la partida finalizada con su puntuación".

### 2.11. Reiniciar partida no reinicia

Al confirmar el reinicio, el sistema vuelve al estado inactivo, y para volver a jugar hace falta además `iniciarPartida()`. Es decir, "reiniciar" funciona en realidad como "abandonar". También se pide confirmación cuando la partida ya está finalizada, donde ya no hay nada que se pueda perder.

**Qué propongo:** que reiniciar empiece directamente una partida nueva, pidiendo confirmación solo si hay una partida en curso. Si se quiere mantener la vuelta al estado inactivo, cambiaría el nombre del caso de uso por algo como "Abandonar partida".

### 2.12. Un intervalo configurado que nadie configura

En la tabla de actores, el Tiempo "solicita descenso automático de piezas según intervalo configurado". Sin embargo, no hay ningún caso de uso que lo configure, la velocidad no aparece en el modelo y no hay niveles. "Configurado" promete algo que no está en ningún requisito.

**Qué propongo:** quitar "configurado" y fijar los intervalos como requisito suplementario: cada cuánto baja la pieza en velocidad normal y cada cuánto en acelerada.

## 3. Conclusión

La mayoría de problemas que he encontrado vienen de que el modelo del dominio y los requisitos no se revisaron juntos. El modelo incluye conceptos que los requisitos no usan (Turno, Movimiento) y le faltan otros que sí usan (puntuación, velocidad, el fin de la partida). Además, cada documento llama a las cosas de una manera distinta (Juego o Partida, `acelerarCaida()` o `alternarVelocidadCaida()`, Finalizado o Game Over).

Las incoherencias que me parecen más claras son la del caso de uso `acelerarCaida()`, que ya no existe, la de los atributos que se anuncian pero no se dibujan, y la del prototipo, que dice cubrir toda la interacción cuando solo cubre mover y rotar. Entre las decisiones discutibles, destacaría la doble composición de Celda y el descarte de Bloque, porque afectan a cómo se representa lo que pasa en el tablero al eliminar filas.

En general, no hace falta añadir funcionalidades nuevas. Lo importante es que los conceptos que ya existen se llamen igual y signifiquen lo mismo en todos los artefactos.
