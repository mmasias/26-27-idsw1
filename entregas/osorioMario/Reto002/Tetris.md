# Reto 002 - Revisión de Tetris

## Incoherencias

* **1. Nombre del caso de uso de velocidad (alternarVelocidadCaida vs acelerarCaida)**
  * **Dónde están y qué dicen:** En ProcesoRequisitos.md (en la tabla, el diagrama de casos de uso y su diagrama de detalle) se llama alternarVelocidadCaida y sirve para pasar de velocidad normal a rápida y viceversa. Pero en el diagrama de detalle de iniciarPartida dice que se puede pedir acelerarCaida, y en la carpeta images/modelosUML/detalleCasosUso hay un diagrama acelerarCaida.svg suelto que solo acelera.
  * **Cuál corregiría y por qué:** Corregiría el diagrama de iniciarPartida y borraría el archivo viejo acelerarCaida.svg para que en todos lados se llame igual, porque se ve que cambiaron el nombre del caso de uso y se olvidaron de actualizar esos dos.

* **2. Teclas del prototipo frente a los casos de uso del jugador**
  * **Dónde están y qué dicen:** En la imagen del prototipo (images/tetris.png) y en el texto final de ProcesoRequisitos.md solo aparecen las teclas 4, 6, 7 y 9 para mover y rotar. Sin embargo, el diagrama de contexto del jugador y el de iniciarPartida dicen que durante la partida también se puede cambiar la velocidad de caída y reiniciar la partida.
  * **Cuál corregiría y por qué:** Corregiría la imagen del prototipo y el texto para agregar las teclas de cambiar velocidad y reiniciar, porque al ser un juego por consola si no tienen una tecla en el menú no hay forma de usarlos.

* **3. Una sola imagen de prototipo frente a las pantallas que piden los diagramas**
  * **Dónde están y qué dicen:** Al final de ProcesoRequisitos.md se dice que con la única imagen del prototipo ya queda todo claro y que poner más sería redundante. Pero el diagrama de arrancarSistema dice que se muestra una pantalla inicial, el de reiniciarPartida pide un mensaje de confirmación con opciones de aceptar o cancelar, y el de descenderPieza pide un mensaje de Game Over.
  
 * **Cuál corregiría y por qué:** Corregiría la parte de prototipado en ProcesoRequisitos.md añadiendo cómo se ven esas tres pantallas, porque tienen textos y opciones que no aparecen por ningún lado en la imagen del tablero.
* **4. Relaciones y multiplicidades entre la tabla y el diagrama del modelo del dominio**
  * **Dónde están y qué dicen:** En modeloDelDominio.md la tabla de la sección 7 solo tiene 6 relaciones con sus multiplicidades, pero el diagrama de la sección 8 (y la imagen adjunta) tiene 10 relaciones (agrega las de Turno, Movimiento y que Pieza ocupa Celda, que sí estaban en la sección 5) y además el diagrama no tiene puesta ninguna multiplicidad aunque en la evolución del modelo dice que se refinaron.
  * **Cuál corregiría y por qué:** Completaría las 4 relaciones que faltan en la tabla de la sección 7 y pondría las multiplicidades en el diagrama del dominio para que la tabla y el dibujo coincidan.
* **5. Piezas activas en plural frente a una sola pieza activa**
  * **Dónde están y qué dicen:** En el diagrama del dominio y en la tabla de la sección 7 dice que el juego contiene piezas activas en plural (1 a muchas). En cambio, en la sección 1 del mismo documento, en todos los diagramas de casos de uso y en la imagen del prototipo siempre se habla de una única pieza activa cayendo.
  * **Cuál corregiría y por qué:** Corregiría el diagrama del dominio y la tabla de la sección 7 para ponerlo en singular (0..1), porque en el Tetris nunca caen varias piezas activas a la vez.
* **6. Juego por turnos y consola esperando comando frente al actor Tiempo**
  * **Dónde están y qué dicen:** El modelo del dominio dice que el juego va por turnos y el prototipo muestra una línea de comando esperando a que el usuario escriba, pero en ProcesoRequisitos.md meten un actor Tiempo que baja la pieza sola automáticamente y el concepto de Turno desaparece de todos los casos de uso.
  * **Cuál corregiría y por qué:** Quitaría Turno del modelo del dominio y la entrada bloqueante del prototipo, porque si el programa se queda parado esperando a que escribas un comando por teclado la pieza no puede ir bajando sola por tiempo.
* **7. Nivel de dificultad y opción de pausar en el modelo del dominio**
  * **Dónde están y qué dicen:** En la sección 6 de modeloDelDominio.md se decide dejar Nivel como propiedad de Juego, pero más abajo en la tabla de simplificaciones dice que el juego es sin niveles de dificultad. Igual pasa en la sección 4, donde se pone pausar como responsabilidad de Juego, pero en ProcesoRequisitos.md no existe ni el estado pausado ni el caso de uso para pausar.
  * **Cuál corregiría y por qué:** Corregiría las secciones 4 y 6 de modeloDelDominio.md quitando el verbo pausar y aclarando desde ahí que el nivel se descarta para que no se contradiga con el resto.
* **8. Estados de los diagramas de contexto y fases de motivación.md**
  * **Dónde están y qué dicen:** El DiagramaDeContexto000 tiene una flecha de Finalizado al cierre del sistema que no existe en los diagramas de los actores ni tiene caso de uso, y escribe los estados en minúsculas mientras los detalles de casos de uso los ponen en mayúsculas. Además, en motivación.md dice que son cuatro fases, en la lista pone cinco (sin Requisitos), en la tabla pone seis y dice que ya están el código en /src/ y el archivo valeLaPenaLaHerencia.md, cuando no existen y Análisis, Decisiones y Diseño solo dicen PENDIENTE.
  * **Cuál corregiría y por qué:** Unificaría los nombres y flechas en los diagramas de contexto y corregiría motivación.md para que refleje las fases reales y lo que todavía falta por hacer.

## Decisiones discutibles

* **1. Doble composición hacia Celda desde Tablero y desde Linea**
  * **Qué se decidió y qué propongo:** En el diagrama del dominio tanto Tablero como Linea tienen composición fuerte (rombo negro) hacia Celda. Propongo dejar la composición fuerte solo entre Tablero y Celda, y que lo de Linea con Celda sea una agregación.
  
* **2. Descartar Bloque diciendo que no existe de forma independiente**
  * **Qué se decidió y qué propongo:** En modeloDelDominio.md se descarta Bloque diciendo que es parte de Pieza y que no existe por su cuenta, dejando solo que Pieza ocupa Celda. Propongo mantener Bloque o aclarar que la Pieza desaparece al tocar fondo dejando ocupadas las celdas.
 
* **3. Usar agregación en Juego con Pieza y dejar Movimiento sin relación con Pieza**
  * **Qué se decidió y qué propongo:** En el diagrama del dominio se puso agregación (rombo blanco) entre Juego y Pieza diciendo que las piezas existen conceptualmente sin un juego, y Movimiento se unió solo a Turno con agregación. Propongo usar composición y unir Movimiento directamente con Pieza.
  
* **4. Cambiar la velocidad como un interruptor fijo en vez de acelerar solo la pieza actual**
  * **Qué se decidió y qué propongo:** En ProcesoRequisitos.md y en el diagrama de alternarVelocidadCaida la velocidad rápida queda activada fija hasta que el jugador la vuelva a cambiar. Propongo que solo acelere la pieza actual y que al salir la siguiente pieza vuelva sola a velocidad normal.
 
* **5. Que reiniciarPartida mande al estado Inactivo y pida confirmación incluso en Game Over**
  * **Qué se decidió y qué propongo:** En el diagrama de reiniciarPartida siempre se pide confirmación al jugador (venga de EnCurso o de Finalizado) y al aceptar lo manda al estado Inactivo. Propongo pedir confirmación solo si está en EnCurso y que al reiniciar empiece directamente una nueva partida en EnCurso.
 
* **6. Pasar siempre por EliminandoLineas al fijar una pieza y decir que la pieza siguiente es solo interfaz**
  * **Qué se decidió y qué propongo:** En el diagrama de descenderPieza, después de fijar la pieza siempre se pasa por EliminandoLineas actualizando la puntuación sin ninguna condición; y en modeloDelDominio.md se descarta la pieza siguiente diciendo que es solo un detalle de interfaz. Propongo poner un rombo de decisión después de fijar la pieza y pasar la pieza siguiente a la tabla de simplificaciones conscientes.
 