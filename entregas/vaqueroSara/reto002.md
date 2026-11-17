# Reto 002 - Tetris!

Revisión de los artefactos de [pyTetris](https://github.com/puntoReflex/pyTetris): [modelo del dominio](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md), [proceso de requisitos](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md) (casos de uso, diagramas de contexto, detalle de casos de uso) y [prototipo](https://github.com/puntoReflex/pyTetris/blob/main/images/tetris.png).

## Incoherencias

### I1. Juego vs Partida

- **Dónde:** modelo del dominio, apartado 3 (tabla de consolidación de sinónimos) vs diagrama de casos de uso y tabla de actores.
- **Qué dice cada uno:** el modelo del dominio unifica *"Juego vs Partida → **Juego**"*. Los requisitos usan `iniciarPartida()` y `reiniciarPartida()`, y el actor Jugador *"solicita ... gestión de partida"*.
- **Corrección:** corregiría el modelo del dominio para recuperar **Partida**, porque es el término que usan los requisitos y el que usa el jugador de forma natural.

### I2. `acelerarCaida()` vs `alternarVelocidadCaida()`

- **Dónde:** detalle de `iniciarPartida()` vs diagrama de casos de uso y detalle de `alternarVelocidadCaida()`.
- **Qué dice cada uno:** el detalle de `iniciarPartida()` dice que el sistema *"permite solicitar: ... acelerarCaida()"*. Ese caso de uso no existe en el diagrama de casos de uso, donde aparece `alternarVelocidadCaida()`. En el repo queda además un `acelerarCaida.svg` sin `.puml` asociado, que es un resto de una versión anterior.
- **Corrección:** corregiría el detalle de `iniciarPartida()`, porque el diagrama de casos de uso es la referencia.

### I3. El prototipo no permite solicitar todos los casos de uso

- **Dónde:** imagen del prototipo y texto de "Prototipar casos de uso" vs diagrama de casos de uso.
- **Qué dice cada uno:** el prototipo solo ofrece `Comando (4=izq, 6=der, 7=rotar↺, 9=rotar↻)`. El diagrama de casos de uso incluye también `alternarVelocidadCaida()` y `reiniciarPartida()` como solicitudes del Jugador, y el texto afirma que en `alternarVelocidadCaida()` *"la pieza cae más rápido (observable en velocidad)"*.
- **Corrección:** corregiría el prototipo añadiendo las teclas que faltan, porque un caso de uso que el jugador no puede solicitar no existe en la práctica.

### I4. Rotación con sentido vs rotación sin sentido

- **Dónde:** imagen del prototipo vs detalle de `rotarPieza()` y de `moverPieza()`.
- **Qué dice cada uno:** el prototipo tiene dos rotaciones (`7=rotar↺`, `9=rotar↻`). El detalle de `rotarPieza()` solo dice *"Jugador solicita rotar pieza"*, sin sentido. En cambio, `moverPieza()` sí especifica *"(izquierda o derecha)"*.
- **Corrección:** corregiría el detalle de `rotarPieza()` indicando el sentido (horario / antihorario), igual que `moverPieza()` indica la dirección.

### I5. `reiniciarPartida()`: inmediato vs con confirmación

- **Dónde:** texto de "Prototipar casos de uso" vs detalle de `reiniciarPartida()`.
- **Qué dice cada uno:** el texto dice *"reiniciarPartida(): vuelve al inicio"* y que *"todo está implícito y claro con esa única imagen"*. El detalle incluye un mensaje de confirmación con las opciones *confirmar reinicio* / *cancelar reinicio*, que no aparece prototipado en ningún sitio.
- **Corrección:** corregiría el texto y añadiría un prototipo de la confirmación, porque el detalle del caso de uso es más preciso que el resumen.

### I6. Transición de salida sin caso de uso

- **Dónde:** `DiagramaDeContexto000` vs diagrama de casos de uso y diagramas de contexto de Jugador y Tiempo.
- **Qué dice cada uno:** `DiagramaDeContexto000` tiene la transición `Finalizado → [*]`. Ningún caso de uso la dispara (no existe `salir()` ni `cerrarSistema()`) y no aparece en los diagramas de contexto por actor.
- **Corrección:** añadiría un caso de uso de salida del sistema, porque el usuario necesita poder cerrar la aplicación.

### I7. Una pieza activa vs varias piezas activas

- **Dónde:** modelo del dominio (tabla del apartado 7 y diagrama) vs detalle de casos de uso.
- **Qué dice cada uno:** el modelo del dominio dice *"Juego - Pieza (activas) | Agregación 1:*"* y *"contiene piezas activas"*. Todos los detalles de casos de uso hablan de **la** pieza activa, en singular (*"presenta primera pieza activa"*, *"presenta nueva pieza activa"*).
- **Corrección:** corregiría el modelo del dominio (multiplicidad 1:1 para la pieza activa), porque en Tetris solo hay una pieza activa a la vez.

### I8. Celda con dos composiciones

- **Dónde:** modelo del dominio, tabla del apartado 7 y diagrama.
- **Qué dice cada uno:** *"Tablero - Celda | Composición 1:*"* y *"Línea - Celda | Composición 1:*"*. Una composición implica un único dueño, así que una misma celda no puede pertenecer por composición a un Tablero y a una Línea a la vez.
- **Corrección:** cambiaría Línea–Celda por una asociación (o trataría Línea como concepto derivado del tablero), porque así se respeta la semántica de la composición.

### I9. Tabla de relaciones vs diagrama del dominio

- **Dónde:** modelo del dominio, tabla del apartado 7, apartado "Evolución del modelo" y diagrama resultante.
- **Qué dice cada uno:** la tabla no recoge Juego–Turno, Turno–Pieza, Turno–Movimiento ni Pieza–Celda, que sí aparecen en el diagrama. Además, la Versión 2 dice *"Se refinan multiplicidades"* y la Versión 3 dice *"Relaciones claramente tipadas"*, pero el diagrama no muestra ninguna multiplicidad.
- **Corrección:** completaría la tabla y añadiría las multiplicidades al diagrama, porque el diagrama es el artefacto final y debe reflejar lo que se dice que se decidió.

### I10. Nivel: propiedad vs simplificación

- **Dónde:** modelo del dominio, apartado 6 vs "Simplificaciones conscientes".
- **Qué dice cada uno:** en el apartado 6, Puntuación y Nivel *"Son propiedades de Juego"*. En las simplificaciones aparece *"Sin niveles de dificultad"*.
- **Corrección:** eliminaría Nivel como propiedad de Juego, porque se ha decidido explícitamente no tener niveles.

### I11. Turno en la evolución del modelo

- **Dónde:** modelo del dominio, apartado "Evolución del modelo".
- **Qué dice cada uno:** la Versión 0 ya incluye Turno en la lista plana. La Versión 2 dice *"Se agregan TipoPieza y Turno"*.
- **Corrección:** corregiría la Versión 2 para que solo mencione TipoPieza.

### I12. Pausar sin caso de uso

- **Dónde:** modelo del dominio, tabla de verbos (apartado 4) vs diagrama de casos de uso y estados.
- **Qué dice cada uno:** el modelo asigna a Juego el verbo *"pausar"*. No existe un caso de uso `pausarPartida()` ni un estado de pausa, y la omisión tampoco aparece entre las simplificaciones.
- **Corrección:** añadiría la pausa a las "Simplificaciones conscientes" (o crearía el caso de uso), para que quede explícito que se ha dejado fuera.

### I13. Nombres de los estados

- **Dónde:** diagramas de contexto vs detalle de casos de uso y texto del prototipo.
- **Qué dice cada uno:** los diagramas de contexto usan `Inactivo`, `EnCurso`, `Finalizado`. Los detalles y el texto usan `INACTIVO`, `EN_CURSO`, `FINALIZADO`.
- **Corrección:** unificaría la nomenclatura en todos los artefactos, porque son los mismos estados.

## Decisiones discutibles

### D1. Turno como entidad del dominio

- **Qué se decidió:** incluir Turno, a partir de la observación *"Cada turno mueves la pieza actual"*.
- **Qué propongo:** eliminar Turno del modelo.
- **Argumento:** Tetris es un juego en tiempo real, no por turnos. Los propios requisitos lo confirman: el descenso lo dispara el actor Tiempo y el jugador actúa cuando quiere, sin ningún turno.

### D2. Movimiento como entidad

- **Qué se decidió:** unificar rotación, caída y desplazamiento en la entidad Movimiento, con `Turno o-- Movimiento`.
- **Qué propongo:** sacar Movimiento del modelo del dominio.
- **Argumento:** mover, rotar y descender son acciones (verbos), sin estado propio. Ya quedan recogidas como casos de uso (`moverPieza()`, `rotarPieza()`, `descenderPieza()`).

### D3. Justificación de "Juego vs Partida"

- **Qué se decidió:** quedarse con Juego porque *"Partida es más específico para juegos multijugador"*.
- **Qué propongo:** distinguir **Juego** (las reglas) de **Partida** (una sesión concreta, con su tablero y su puntuación).
- **Argumento:** el argumento es falso: se habla de "una partida de solitario" sin problema. Son dos conceptos distintos, no sinónimos.

### D4. Justificación de la exclusión de Jugador

- **Qué se decidió:** excluir Jugador porque *"El sistema no necesita modelar múltiples jugadores"*.
- **Qué propongo:** mantener la exclusión, pero con otro argumento: el Jugador es externo al sistema y aparecerá como actor en los requisitos.
- **Argumento:** "lo que el sistema necesita" es un criterio de software, y el propio documento dice que el modelo del dominio debe estar *"sin contaminación tecnológica"*.

### D5. `reiniciarPartida()` lleva a INACTIVO y pide confirmación desde FINALIZADO

- **Qué se decidió:** al reiniciar se vuelve a INACTIVO, y siempre se pide confirmación, también desde FINALIZADO.
- **Qué propongo:** no pedir confirmación desde FINALIZADO, y que "reiniciar" empiece una partida nueva (EN_CURSO), o bien renombrar el caso de uso (por ejemplo, `volverAlInicio()`).
- **Argumento:** en FINALIZADO no hay nada que perder, así que la confirmación sobra. Además, "reiniciar" promete una partida nueva y lo que hace es volver al estado inactivo.

### D6. `alternarVelocidadCaida()` como interruptor

- **Qué se decidió:** una orden que alterna entre velocidad normal y acelerada.
- **Qué propongo:** caída rápida mientras se mantiene la orden (o una caída instantánea), como en el Tetris clásico.
- **Argumento:** el interruptor deja un estado oculto (normal / acelerada) que el prototipo no muestra en ningún sitio, así que el jugador no sabe en qué modo está.

### D7. Prototipar con una sola imagen

- **Qué se decidió:** una única imagen, porque *"agregar más sería redundante"*.
- **Qué propongo:** añadir como mínimo la pantalla inicial, el mensaje de Game Over y la confirmación de reinicio.
- **Argumento:** son interacciones que aparecen en los detalles de casos de uso (`arrancarSistema()`, `descenderPieza()`, `reiniciarPartida()`) y que la imagen no muestra.

### D8. Descartar "Pieza siguiente" como detalle de interfaz

- **Qué se decidió:** descartar *"Preview/Siguiente"* por ser *"Detalle de interfaz, no del dominio puro"*.
- **Qué propongo:** incluir la pieza siguiente en el modelo del dominio.
- **Argumento:** forma parte de las reglas del juego (el jugador planifica sus movimientos con ella). No es un adorno visual como la sombra (*ghost*).

### D9. Validación con expertos del dominio

- **Qué se decidió:** dar el modelo por validado con preguntas que se responden a sí mismas (*"¿Hay conceptos que sobran? No"*).
- **Qué propongo:** hacer una validación real (con jugadores o con las reglas oficiales) o retirar la afirmación.
- **Argumento:** no hay ninguna evidencia de la validación, y la conclusión "no sobra nada" no se sostiene si Turno y Movimiento sobran (ver D1 y D2).