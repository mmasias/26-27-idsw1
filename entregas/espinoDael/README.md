# Reto 002 - Análisis de Tetris

Revisión de la documentación y diagramas del repositorio [pyTetris](https://github.com/puntoReflex/pyTetris), dividida en incoherencias entre artefactos y decisiones discutibles.

## 1. Incoherencias

### 1.1. Dos nombres distintos para cambiar la velocidad
* **Dónde está:** En la [lista de casos de uso](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#casos-de-uso) y en el detalle de [iniciarPartida()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/iniciarPartida.puml).
* **Qué dice cada uno:** En la lista general el caso de uso se llama `alternarVelocidadCaida()`, pero en el diagrama de `iniciarPartida()` aparece escrito como `acelerarCaida()`.
* **Qué cambiaría y por qué:** Dejaría `alternarVelocidadCaida()` en ambos lados. Acelerar da a entender que solo se puede ir más rápido, mientras que alternar explica mejor que puedes pasar de velocidad rápida a normal y viceversa.

### 1.2. Contradicción sobre si existen niveles o no
* **Dónde está:** En el documento del [modelo del dominio](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md) (secciones de conceptos, decisiones y simplificaciones).
* **Qué dice cada uno:** Primero se identifica el `Nivel` como concepto y se guarda como un atributo de `Juego`, pero más abajo, en las simplificaciones, se dice que el juego no tendrá niveles de dificultad.
* **Qué cambiaría y por qué:** Quitaría el atributo `nivel` de `Juego` si realmente se decidió simplificar el problema. No tiene sentido modelar un dato que el propio documento dice que no va a existir.

### 1.3. Las celdas tienen dos composiciones a la vez
* **Dónde está:** En la [tabla de relaciones](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md) y en el [diagrama del modelo del dominio](https://github.com/puntoReflex/pyTetris/blob/main/images/modelosUML/ModeloDominio.svg).
* **Qué dice cada uno:** Se indica con rombo negro (composición) que el `Tablero` se compone de `Celda`, y al mismo tiempo que cada `Línea` se compone de `Celda`.
* **Qué cambiaría y por qué:** Haría que el `Tablero` se componga de `Línea` y cada `Línea` de `Celda`. En UML un elemento no puede pertenecer en exclusiva (composición fuerte) a dos clases distintas a la vez.

### 1.4. El reinicio no cuenta con la opción de cancelar en el contexto
* **Dónde está:** En el [diagrama de contexto del Jugador](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorJugador.puml) y en el detalle de [reiniciarPartida()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml).
* **Qué dice cada uno:** El diagrama de contexto muestra que `reiniciarPartida()` lleva siempre al estado `Inactivo`. En cambio, el detalle del caso de uso pide confirmación al jugador y le permite cancelar para seguir jugando.
* **Qué cambiaría y por qué:** Añadiría en el diagrama de contexto que solo se pasa a `Inactivo` si se confirma, y que si se cancela el juego se queda en el estado en el que estaba (`EnCurso` o `Finalizado`).

### 1.5. Las salidas de varios casos de uso se mezclan sin condición
* **Dónde está:** En los diagramas de detalle de [moverPieza()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/moverPieza.puml), [rotarPieza()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/rotarPieza.puml), [descenderPieza()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/descenderPieza.puml) y [reiniciarPartida()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml).
* **Qué dice cada uno:** Dentro de los diagramas se separan los caminos (por ejemplo, si el movimiento es válido o si choca), pero al final se juntan en un mismo punto y luego salen varias flechas finales sin texto que indique cuál es cuál.
* **Qué cambiaría y por qué:** Pondría notas o condiciones en las flechas de salida (`[movimiento válido]`, `[bloqueado]`, `[confirmado]`, etc.) para saber exactamente por qué salida termina cada caso.

### 1.6. Al bajar una pieza primero se dibuja y luego se comprueba si choca
* **Dónde está:** En el detalle de [descenderPieza()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/descenderPieza.puml) comparado con [moverPieza()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/moverPieza.puml).
* **Qué dice cada uno:** En `moverPieza()` primero se mira si el hueco está libre y luego se muestra la pieza. En `descenderPieza()` se hace al revés: primero se muestra la pieza en la nueva posición y después se comprueba si ha chocado o llegado al fondo.
* **Qué cambiaría y por qué:** Cambiaría `descenderPieza()` para comprobar primero si puede bajar antes de dibujarla. Si se dibuja antes de comprobar, se mostraría la pieza pisando otras piezas o saliéndose del tablero.

### 1.7. Al prototipo le faltan controles y pantallas que piden los casos de uso
* **Dónde está:** En los detalles de [alternarVelocidadCaida()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/alternarVelocidadCaida.puml) y [reiniciarPartida()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml) frente a la sección de [prototipado](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#prototipar-casos-de-uso) y la [imagen del prototipo](https://github.com/puntoReflex/pyTetris/blob/main/images/tetris.png).
* **Qué dice cada uno:** El texto dice que la captura del prototipo basta para cubrir los casos de uso, pero en la imagen solo salen las teclas de mover y rotar (`4`, `6`, `7`, `9`). No aparece ninguna tecla para cambiar la velocidad ni tampoco la pantalla de confirmación para reiniciar.
* **Qué cambiaría y por qué:** Añadiría en la leyenda de la imagen las teclas para cambiar velocidad y reiniciar, además de un segundo boceto donde se vea el mensaje de confirmar o cancelar el reinicio.

---

## 2. Decisiones discutibles

### 2.1. Permitir varias piezas activas a la vez
* **Qué se decidió:** En la [tabla de relaciones](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md) se asocia el juego con varias piezas activas.
* **Qué propongo y por qué:** Limitar la relación a una sola pieza activa cada vez, diferenciándola de los bloques que ya cayeron y quedaron fijos. En el Tetris normal el jugador solo maneja una pieza cayendo por turno.

### 2.2. No poner las cantidades ni el tamaño del tablero en el diagrama del dominio
* **Qué se decidió:** En el texto del [modelo del dominio](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md) se indican las cantidades de las relaciones y que el tablero es de 10 × 20, pero en el [diagrama final](https://github.com/puntoReflex/pyTetris/blob/main/images/modelosUML/ModeloDominio.svg) no aparece ningún número.
* **Qué propongo y por qué:** Escribir las multiplicidades (`1`, `4`, `20`, `10`, etc.) directamente en las líneas del diagrama. Así el dibujo se entiende por sí solo sin tener que leer tablas aparte, y deja claras las medidas básicas del Tetris.

### 2.3. Uso del concepto «Turno» en el dominio
* **Qué se decidió:** Incluir la clase `Turno` en el [modelo del dominio](https://github.com/puntoReflex/pyTetris/blob/main/images/modelosUML/ModeloDominio.svg) conectada con pieza y movimiento.
* **Qué propongo y por qué:** Quitar `Turno` o explicar en el documento que se refiere al tiempo desde que sale una pieza hasta que se fija. En juegos en tiempo real donde las piezas caen solas no hay turnos como en un juego de mesa, por lo que el nombre confunde.

### 2.4. Crear una clase `TipoPieza` en vez de una lista cerrada
* **Qué se decidió:** Poner `TipoPieza` como una clase separada en el [modelo del dominio](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md) sin añadirle atributos propios.
* **Qué propongo y por qué:** Representarlo como un enumerado con las 7 formas del Tetris (`I`, `O`, `T`, `S`, `Z`, `J`, `L`). Si la clase no guarda más datos ni reglas, una lista cerrada es mucho más sencilla.

### 2.5. El argumento para quitar `Jugador` del modelo del dominio
* **Qué se decidió:** Descartar la clase `Jugador` en el [modelo del dominio](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md) diciendo que es porque juega una sola persona.
* **Qué propongo y por qué:** Justificar que se quita porque **no se guardan datos del jugador** (como su nombre o historial de récords). Aunque juegue una sola persona, si el juego guardara un ranking haría falta la clase; como aquí no se guarda nada de él, ese es el motivo real para dejarlo solo como actor.

### 2.6. Nombres de casos de uso escritos como funciones de código
* **Qué se decidió:** Llamar a los casos de uso con paréntesis y formato de programación (`moverPieza()`, `rotarPieza()`) en el [diagrama de casos de uso](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaCasosUso.puml).
* **Qué propongo y por qué:** Escribirlos de forma normal: «Mover pieza», «Rotar pieza», «Iniciar partida». En la fase de requisitos se describe qué hace el usuario sin pensar todavía en funciones de código.

### 2.7. Que «Reiniciar partida» lleve al estado `Inactivo`
* **Qué se decidió:** Hacer que el caso de uso [reiniciarPartida()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml) termine en el estado `INACTIVO` al confirmar.
* **Qué propongo y por qué:** Cambiarle el nombre a «Salir al inicio» si lo que hace es volver a la pantalla de espera, o hacer que `reiniciarPartida()` empiece directamente una partida limpia en estado `EN_CURSO`. Cuando un jugador le da a reiniciar, espera empezar a jugar otra vez de inmediato.

### 2.8. Faltan detalles sobre la rotación y el cálculo de puntos
* **Qué se decidió:** En el [prototipo](https://github.com/puntoReflex/pyTetris/blob/main/images/tetris.png) salen dos teclas de giro (`7=rotarS` y `9=rotarU`) que [rotarPieza()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/rotarPieza.puml) no distingue, y en [descenderPieza()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/descenderPieza.puml) se habla de «Actualizar puntuación» sin decir cuántos puntos vale cada línea.
* **Qué propongo y por qué:** Indicar en `rotarPieza()` que se puede girar a izquierda o derecha, y añadir una nota sencilla explicando cuántos puntos da eliminar 1, 2, 3 o 4 líneas. Sin eso, quedan reglas del juego sin definir.

### 2.9. Se menciona la pausa en el dominio pero no en los requisitos
* **Qué se decidió:** En la [tabla de responsabilidades del dominio](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md) se dice que el juego controla si está pausado, pero en los [requisitos](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md) no hay ningún caso de uso para pausar.
* **Qué propongo y por qué:** Quitar la mención a la pausa en la tabla del dominio si no entra en esta versión, o añadir el caso de uso `pausarPartida()`. Lo que se incluye en el dominio debe coincidir con lo que pide la parte de requisitos.
