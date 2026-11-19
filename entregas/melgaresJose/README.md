# Reto 002 - Análisis de pyTetris


Basandonos en lo existente dentro del repositorio de [pyTetris](https://github.com/puntoReflex/pyTetris). Nos proponemos a encontrar **incoherencias** y/o **decisiones discutibles** dentro de los artefactos y propuestas dentro del mismo.

---

## Incoherencias

### 1. Nivel y Velocidad - Apartados 4 y 6

**Qué dicen**: Por un lado, se decide que Nivel es una propiedad/atributo de Juego y que su verbo asociado es "determinar velocidad". Por otro lado, más abajo se descarta por completo el concepto de niveles de dificultad ("Sin niveles de dificultad").

**Qué corregiría**: En los juegos de Tetris la velocidad se determina en base al nivel o puntaje obtenido. Estos dos terminos van fuertemente enlazados entre sí: la velocidad a la cual las piezas bajan es el nivel de dificultad. En el caso de las opciones como "Seleccionar dificultad" suelen ser más determinar la velocidad de bajada inicial. Eliminaría Nivel de la sección 6 y dejar la responsabilidad para Juego o Velocidad. Dejar "Nivel" como propiedad activa en la sección 6 y luego decir "Sin niveles de dificultad" es una contradicción directa, a mi parecer.

### 2. ¿Piezas activas? - Apartados 5 y 7

**Qué dicen**: Se habla de una pieza por turno, y en las simplificaciones se aclara que es un juego de un solo jugador clásico (sin "Preview/Siguiente" ni "Hold piece", que fueron descartados explícitamente). Sin embargo, en la tabla de multiplicidades se modela una relación `1:*` (uno a muchos) entre `Juego` y `Pieza (activas)`.

**Que corregiría**: En cualquier caso, eliminaría la multiplicidad `1:*` por una `1:1`. Solo puede haber una pieza activa para el jugador por la naturaleza explícita limitada del juego. Un jugador solo puede tener una a la vez. Otra opción sería consider la opción de **Bloques Activos**. Esto para diferenciar de aquellos que está modificando activamente el jugador y los asentados, aunque esto también involucraría modificarr la existencia de ´Pieza´.

### 3. Turno y movimiento - Apartados 4, 7 y 5

**Qué dicen**: En la v2 se dice "Se agregan TipoPieza y Turno" y en la sección 4 ambos figuran como entidades confirmadas del dominio. Sin embargo, en la Sección 7 (Refinamiento de relaciones) desaparecen por completo y no tienen ninguna relación ni multiplicidad definida con `Juego` ni con `Pieza`. Además, el concepto de "Turno" choca con un juego en tiempo real (donde la velocidad cambia con el tiempo y la pieza cae continuamente, a diferencia de un juego por turnos como el ajedrez). Lo cual en cierta medida es redudante con lo especificado en la incoherencia 1.

**Que corregiría**: Eliminaría `Turno` como una entidad completamente, o renombrarlo a algo más acorde a lo que sucede entre que una pieza aparece y se asienta. Esto porque Tetris es continuo y no hay "turnos" como tal. Por otra parte, si se va a mantener `Movimiento` (para cubrir la rotación y desplazamientos), hay que incluirlo en la Sección 7 relacionándolo con Pieza o Juego, o este queda huérfano entre el análisis y el diagrama final.

### 4. Descartar Columna pero mantener Tablero - Apartados 2 y 7

**Qué dicen**: Se descarta `Columna` con el argumento de que "no tiene comportamiento propio", pero se mantiene `Línea` (que tampoco tiene comportamiento propio más allá de agrupar celdas horizontales y verificar si están llenas) y se omite cómo el tablero sabe cuándo se pierde el juego (cuando una columna se llena hasta el tope).

**Que corregiría**: No creo que haga falta reincorporar `Columna` como entidad, pero sí ajustar la justificación o reflejar en las responsabilidades de `Tablero y/o Celda `cómo se estructuran las dimensiones (filas y columnas) para que la condición de derrota y la rotación tengan una base sobre la que apoyarse.

## Decisiones discutibles

### 1. La separación de pizas con tipo de Piezas:

