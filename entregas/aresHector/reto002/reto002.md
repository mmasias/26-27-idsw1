# Reto 002 - Tetris!

Revisión de [pyTetris](https://github.com/puntoReflex/pyTetris): [modelo del dominio](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md), [proceso de requisitos](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md) y [prototipo](https://github.com/puntoReflex/pyTetris/blob/main/images/tetris.png).

## Incoherencias

### 1. Juego o Partida

- **Dónde:** modelo del dominio (sinónimos) vs lista de casos de uso y tabla de actores.
- **Qué dicen:** el modelo del dominio junta Juego y Partida y se queda con **Juego**. Pero en los requisitos los casos de uso son `iniciarPartida()` y `reiniciarPartida()`, y el Jugador *"solicita la gestión de la partida"*.
- **Cuál corregiría:** el modelo del dominio. Si los requisitos y el jugador dicen "partida", el modelo del dominio debería usar esa palabra, que para eso es el vocabulario del dominio.

### 2. "Desplazamiento" aparece de la nada

- **Dónde:** modelo del dominio, lista inicial de conceptos vs tabla de sinónimos.
- **Qué dicen:** en la lista de conceptos están Movimiento, Rotación y Caída, pero no Desplazamiento. Luego, en los sinónimos, se agrupan *"Rotación, Caída y Desplazamiento"* en Movimiento.
- **Cuál corregiría:** la lista inicial, añadiendo Desplazamiento. Si no se encontró como concepto, no se puede unificar después.

### 3. Nivel sí, pero sin niveles

- **Dónde:** modelo del dominio, decisiones de modelado vs simplificaciones conscientes.
- **Qué dicen:** en las decisiones, *"Puntuación y Nivel son propiedades del Juego"*. En las simplificaciones pone *"Sin niveles de dificultad"*.
- **Cuál corregiría:** las decisiones de modelado, quitando Nivel. Lo de no tener niveles es una decisión explícita, así que Nivel sobra.

### 4. Verbos de Juego sin caso de uso

- **Dónde:** modelo del dominio (verbos de Juego) vs lista de casos de uso.
- **Qué dicen:** Juego tiene los verbos *iniciar, terminar, pausar*. En los casos de uso hay `iniciarPartida()`, pero no hay nada para pausar ni para terminar/salir.
- **Cuál corregiría:** los requisitos, añadiendo al menos un caso de uso para salir. Pausar se podría dejar fuera, pero entonces habría que ponerlo en las simplificaciones para que se vea que es a propósito.

### 5. El prototipo no tiene todas las teclas

- **Dónde:** imagen del prototipo vs lista de casos de uso y texto de "Prototipar casos de uso".
- **Qué dicen:** en la imagen solo hay teclas para mover a izquierda/derecha y rotar (4, 6, 7 y 9). Pero el Jugador también puede pedir `alternarVelocidadCaida()`, `reiniciarPartida()` e `iniciarPartida()`, y el texto dice que el prototipo cubre esos casos de uso.
- **Cuál corregiría:** el prototipo, añadiendo las teclas que faltan. Si el jugador no tiene forma de pedir un caso de uso, el prototipo no lo está cubriendo.

### 6. Velocidad: ¿atributo o intervalo configurado?

- **Dónde:** modelo del dominio (conceptos descartados) vs tabla de actores.
- **Qué dicen:** el modelo del dominio descarta Velocidad porque es *"atributo de Juego"*, pero luego no aparece entre las propiedades de Juego (solo Puntuación y Nivel). En los requisitos, el actor Tiempo baja la pieza *"según un intervalo configurado"*, pero ningún caso de uso permite configurarlo.
- **Cuál corregiría:** el modelo del dominio, poniendo Velocidad como propiedad de Juego, y en los requisitos quitaría lo de "configurado" (o añadiría quién lo configura).

## Decisiones discutibles

### 1. Turno

- **Qué se decidió:** meter Turno en el modelo del dominio.
- **Qué propongo:** quitarlo.
- **Argumento:** el Tetris no va por turnos. La pieza baja sola por el tiempo y el jugador pulsa cuando quiere. De hecho, en los requisitos ningún caso de uso habla de turnos, aunque digan que parten del modelo del dominio.

### 2. Bloque como parte de la Pieza

- **Qué se decidió:** descartar Bloque porque *"no existe por separado"* de la Pieza.
- **Qué propongo:** mantener Bloque (o al menos explicar qué pasa al colocar la pieza).
- **Argumento:** cuando una pieza se coloca, deja de ser una pieza y sus bloques se quedan en el tablero. Y al eliminar una línea se borra solo una parte de lo que era la pieza. O sea, los bloques sí existen por separado después de caer.

### 3. Columna fuera, Línea dentro

- **Qué se decidió:** descartar Columna porque *"no tiene comportamiento propio"* y mantener Línea.
- **Qué propongo:** usar el mismo criterio para las dos, por ejemplo tratar Línea como algo que se calcula del tablero (una fila de celdas) y no como entidad compuesta por celdas.
- **Argumento:** Línea y Columna son lo mismo: un grupo de celdas del tablero. Lo único que tiene Línea es la regla de "si está llena se borra", y eso se puede ver como una regla del Tablero.

### 4. Jugador fuera del modelo por "no hace falta"

- **Qué se decidió:** excluir Jugador porque el Tetris de un jugador *"no necesita modelarlo"*.
- **Qué propongo:** dejarlo fuera, pero porque el jugador es externo al juego (luego sale como actor en requisitos).
- **Argumento:** "no se necesita" es pensar en el programa, y el modelo del dominio debería describir el mundo real, no lo que hace falta programar.

### 5. Una sola imagen de prototipo

- **Qué se decidió:** usar solo una captura porque *"añadir más sería redundante"*.
- **Qué propongo:** añadir al menos la pantalla de antes de empezar y la de fin de partida.
- **Argumento:** la imagen solo enseña el estado EN_CURSO. Cómo se ve el sistema al arrancar o cuando pierdes no se puede saber con esa captura, así que no es redundante.
