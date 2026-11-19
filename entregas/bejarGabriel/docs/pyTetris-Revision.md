# Reto 002 - Revisión de `pyTetris`

## Incoherencias

### 1. `Juego` en el dominio frente a `Partida` en requisitos
- En el modelo del dominio se descarta el término `Partida` diciendo que es propio de juegos multijugador y se unifica todo bajo `Juego`. Sin embargo, en requisitos desaparece `Juego` y todos los casos de uso y transiciones usan `Partida` (`iniciarPartida()`, `reiniciarPartida()`, "partida iniciada", "continuar partida").
- Corregiría el modelo del dominio para usar `Partida`. El vocabulario del dominio debería mantenerse correspondiente a requisitos.

### 2. `Turno` en el dominio frente a un juego en tiempo real en requisitos
- El modelo del dominio afirma que "cada turno mueves la pieza actual" e incluye la clase `Turno`. Luego, en requisitos, el actor `Tiempo` hace caer la pieza de forma continua mientras el jugador actúa cuando quiere; `Turno` no vuelve a aparecer en ningún artefacto.
- Eliminaría `Turno` del modelo del dominio. El Tetris no es un juego por turnos, así que ese concepto sobra y no tiene reflejo en el comportamiento del sistema.

### 3. `Nivel` como propiedad de `Juego` y luego descartado
- Se excluye `Nivel` como entidad diciendo que es una propiedad de `Juego`, pero más abajo, en "Simplificaciones conscientes", se indica "Sin niveles de dificultad". Por otro lado, `Puntuación` y `Velocidad` sí se definen como atributos de `Juego` y se usan en requisitos (`iniciarPartida`, `descenderPieza`, `alternarVelocidadCaida`), pero en el diagrama no aparecen.
- Descartaría `Nivel` del alcance y añadiría los atributos `puntuación` y `velocidad` a la entidad `Juego` en el diagrama del dominio.

---

## Decisiones discutibles

### 1. Modelar `Movimiento` como clase del dominio
- Propongo quitar `Movimiento` del modelo del dominio y dejar esas acciones únicamente como casos de uso (`desplazarPieza`, `rotarPieza`, `descenderPieza`).
- En Tetris un movimiento no es una entidad con datos propios ni un historial que haya que guardar; es una acción puntual del jugador. Sin `Turno` (que tampoco aplica) queda suelto y solo añade ruido al diagrama.

### 2. Usar sintaxis de programación y acciones internas en requisitos
- Se escriben los casos de uso con paréntesis (`iniciarPartida()`, `moverPieza()`) y describir en `reiniciarPartida.puml` una acción interna como *"Sistema limpia estado de partida"*.
- Propongo nombrar los casos de uso en lenguaje natural (`Iniciar partida`, `Mover pieza`) y reemplazar *"limpia estado de partida"* por algo observable por el usuario, como *"Sistema presenta tablero vacío y puntuación inicial (0)"*.