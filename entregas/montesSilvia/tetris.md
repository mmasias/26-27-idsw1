
# RETO 002 – Tetris

## Análisis de incoherencias y decisiones discutibles

### Introducción

En este reto he revisado la documentación del proyecto Tetris que hemos utilizado como ejemplo en clase. Me he centrado principalmente en el modelo del dominio, los casos de uso, los diagramas de contexto y el prototipo.

El objetivo ha sido encontrar posibles contradicciones entre los distintos documentos y decisiones que, aunque puedan ser válidas, considero que podrían haberse planteado de otra manera.

Para organizar el análisis, he separado los problemas en dos apartados:

- **Incoherencias:** diferencias entre artefactos que deberían explicar el mismo funcionamiento. En cada caso indico dónde aparece el problema, qué dice cada documento y qué corregiría.
- **Decisiones discutibles:** aspectos del modelo o de los requisitos que yo resolvería de otra manera. En estos casos explico qué se decidió, qué alternativa propongo y por qué.

He intentado distinguir las contradicciones de las simples mejoras, porque no todo lo que cambiaría tiene que ser necesariamente un error.

---

## 1. Incoherencias

### 1.1. Falta una transición de reinicio en el diagrama general

**Dónde aparece:**

- [Diagrama general de estados](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto000.puml).
- [Diagrama de contexto del Jugador](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorJugador.puml).

**Qué dice cada uno:**

Al comparar los dos diagramas, he observado que en el diagrama general no aparece la posibilidad de pasar directamente del estado `EN_CURSO` al estado `INACTIVO`.

Sin embargo, en el diagrama del Jugador sí se contempla esta transición cuando se utiliza `reiniciarPartida()`.

Esto me parece una incoherencia porque los dos diagramas representan los estados del mismo sistema. Si el jugador puede reiniciar mientras está jugando, ese cambio también debería aparecer en el diagrama general.

**Qué corregiría y por qué:**

Añadiría la transición de `EN_CURSO` a `INACTIVO` en el diagrama general, indicando que se produce cuando se confirma el reinicio de la partida.

De esta forma, los dos diagramas mostrarían los mismos cambios de estado y sería más fácil entender el funcionamiento completo del juego.

### 1.2. Una misma acción recibe dos nombres diferentes

**Dónde aparece:**

- [Tabla de casos de uso](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#casos-de-uso).
- [Detalle de iniciarPartida()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/iniciarPartida.puml).
- [Detalle de alternarVelocidadCaida()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/alternarVelocidadCaida.puml).

**Qué dice cada uno:**

En la tabla de casos de uso aparece la operación `alternarVelocidadCaida()`, mientras que en el detalle de iniciar una partida se utiliza el nombre `acelerarCaida()`.

Aunque las dos operaciones están relacionadas con la velocidad de caída de las piezas, no significan exactamente lo mismo.

Acelerar indica que la pieza empieza a caer más rápido, mientras que alternar significa cambiar entre dos velocidades, pudiendo volver a la velocidad normal.

Además, el caso de uso detallado confirma que se puede pasar de velocidad normal a acelerada y viceversa.

**Qué corregiría y por qué:**

Utilizaría `alternarVelocidadCaida()` en todos los documentos, ya que es el nombre que mejor representa el comportamiento descrito.

Considero que mantener el mismo nombre es importante para evitar confusiones cuando se relacionen los requisitos con el desarrollo del programa.

### 1.3. No queda claro si el juego tiene niveles de dificultad

**Dónde aparece:**

- [Modelo del dominio: identificación de conceptos](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#2-extracción-de-sustantivos-identificación-de-conceptos).
- [Modelo del dominio: decisiones críticas](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#6-decisiones-críticas-de-modelado).
- [Modelo del dominio: simplificaciones conscientes](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#simplificaciones-conscientes).

**Qué dice cada uno:**

Al principio del modelo del dominio se identifica el concepto Nivel, relacionado con la dificultad actual del juego.

Más adelante se decide que Nivel no necesita ser una entidad independiente porque puede representarse como una propiedad de Juego.

Sin embargo, en el apartado de simplificaciones conscientes se indica que el sistema no contempla niveles de dificultad.

Esto puede resultar contradictorio, porque primero se mantiene el nivel como una propiedad y después parece que se elimina esa funcionalidad.

También podría tratarse de dos conceptos distintos: el nivel que aumenta durante la partida y la dificultad que se elige antes de jugar, pero esa diferencia no está explicada.

**Qué corregiría y por qué:**

Aclararía si el nivel aumenta automáticamente durante la partida o si se ha decidido no incluir niveles en esta versión.

Si no se va a utilizar, eliminaría la referencia a Nivel como propiedad del juego. Si se mantiene, explicaría qué representa.

Creo que es importante porque, tal como está escrito, no queda claro qué comportamiento debería tener el sistema.

### 1.4. La tabla de relaciones y el modelo final no contienen la misma información

**Dónde aparece:**

- [Modelo del dominio, apartado 7](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#7-refinamiento-de-relaciones).
- [Modelo del dominio, apartado 8](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#8-modelo-del-dominio-resultante).
- [Diagrama UML final](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/ModeloDominio.puml).

**Qué dice cada uno:**

En el apartado de refinamiento de relaciones se explican seis relaciones entre los conceptos del dominio.

Sin embargo, en el modelo final aparecen otras relaciones que no se han desarrollado en esa tabla.

Por ejemplo, el diagrama incluye relaciones entre Juego y Turno, Pieza y Celda, Turno y Pieza, y Turno y Movimiento.

Por tanto, la tabla no recoge todas las relaciones que finalmente aparecen representadas.

No significa necesariamente que las relaciones añadidas sean incorrectas, pero sí existe una diferencia entre lo que se explica en el texto y lo que muestra el modelo final.

**Qué corregiría y por qué:**

Completaría la tabla incorporando las relaciones que faltan, indicando el tipo de relación y sus multiplicidades.

Creo que, si se presenta un modelo del dominio final, debería poder entenderse cómo se ha llegado hasta él a partir del análisis anterior.

Así se mantendría una mejor relación entre la explicación escrita y el diagrama.

### 1.5. El regreso desde FINALIZADO no se explica igual en los diagramas

**Dónde aparece:**

- [Diagrama de contexto del Tiempo](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorTiempo.puml).
- [Diagrama de contexto del Jugador](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorJugador.puml).
- [Detalle de reiniciarPartida()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml).

**Qué dice cada uno:**

En el diagrama del Tiempo aparece una transición desde `FINALIZADO` hasta `INACTIVO`, pero no se explica qué acción provoca ese cambio.

En cambio, en el diagrama del Jugador esa transición aparece asociada a `reiniciarPartida()`.

Además, en el caso de uso detallado se explica que el jugador debe solicitar y confirmar el reinicio para que el sistema vuelva a INACTIVO.

Por eso, al comparar los diagramas, no queda claro si el cambio se realiza automáticamente o si depende de una decisión del jugador.

**Qué corregiría y por qué:**

Eliminaría esa transición del diagrama del Tiempo si no representa ninguna acción relacionada con ese actor, o añadiría una explicación indicando que el cambio depende del reinicio solicitado por el Jugador.

Creo que sería conveniente aclararlo porque el Tiempo se encarga del descenso automático de las piezas, pero no debería decidir por sí mismo cuándo reiniciar una partida.

### 1.6. El descenso y el movimiento de las piezas validan la posición de forma diferente

**Dónde aparece:**

- [Detalle de descenderPieza()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/descenderPieza.puml).
- [Detalle de moverPieza()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/moverPieza.puml).

**Qué dice cada uno:**

En el caso de uso `moverPieza()` se distingue si la nueva posición es válida o inválida. Si existe una colisión o se alcanza un límite, la pieza permanece donde estaba.

Sin embargo, en `descenderPieza()` primero se indica que el sistema presenta la pieza en una nueva posición y después se diferencia si ha llegado al fondo o se encuentra en una posición intermedia.

Esto hace que la explicación del descenso no siga el mismo criterio que el movimiento lateral.

En un caso se deja claro que no debe producirse un movimiento inválido, mientras que en el otro no se explica de forma tan precisa cuándo se comprueba que la pieza puede bajar.

**Qué corregiría y por qué:**

Haría que el descenso comprobara primero si la pieza puede avanzar una fila hacia abajo.

Si puede, se actualiza su posición. Si no puede porque existe una colisión o ha llegado al fondo, se deja en la última posición válida y se fija al tablero.

Me parece más lógico mantener la misma regla de validación para todos los movimientos.

### 1.7. El prototipo no representa todas las interacciones que se consideran explicadas

**Dónde aparece:**

- [Apartado Prototipar casos de uso](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#prototipar-casos-de-uso).
- [Tabla de casos de uso](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#casos-de-uso).
- [Detalle de reiniciarPartida()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml).
- [Imagen del prototipo](https://github.com/puntoReflex/pyTetris/blob/main/images/tetris.png).

**Qué dice cada uno:**

En el apartado de prototipado se afirma que una única imagen resulta suficiente para comprender los casos de uso, porque las demás acciones se consideran implícitas.

Sin embargo, los requisitos describen comportamientos que no se pueden comprobar solamente con una captura del juego en funcionamiento.

Por ejemplo, el reinicio requiere una confirmación y permite cancelar la operación, pero esa interacción no aparece representada en el prototipo.

También se describe el cambio de velocidad, aunque una imagen estática no permite observar cómo se produce.

Por tanto, el problema no es que esas funciones no existan, sino que la imagen no muestra toda la información que el texto considera suficientemente explicada.

**Qué corregiría y por qué:**

Añadiría al menos un prototipo de la confirmación del reinicio y una explicación de cómo se cambia la velocidad.

Creo que esto ayudaría a entender mejor las acciones disponibles y evitaría tener que suponer cómo funcionan determinadas partes de la interfaz.

---

## 2. Decisiones discutibles

### 2.1. La cantidad de piezas activas no está bien representada

**Dónde aparece:**

- [Modelo del dominio, apartado 7](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#7-refinamiento-de-relaciones).
- [Detalle de descenderPieza()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/descenderPieza.puml).

**Qué se ha decidido:**

Se establece una relación de uno a muchos entre Juego y las piezas activas.

**Qué cambiaría y por qué:**

Yo distinguiría las piezas que ya se han colocado en el tablero de la pieza que está cayendo en ese momento.

En una partida normal de Tetris solamente hay una pieza activa al mismo tiempo, aunque a lo largo del juego aparezcan muchas.

Por eso, cambiaría la multiplicidad o especificaría que solamente una de las piezas está activa en cada momento.

Creo que así el modelo representaría mejor el funcionamiento del juego y evitaría confundir las piezas activas con las que ya se han fijado.

### 2.2. Una misma celda aparece en dos relaciones de composición

**Dónde aparece:**

- [Modelo del dominio, apartado 7](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#7-refinamiento-de-relaciones).
- [Diagrama UML final](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/ModeloDominio.puml).

**Qué se ha decidido:**

Se utiliza composición tanto entre Tablero y Celda como entre Línea y Celda.

**Qué cambiaría y por qué:**

Yo mantendría la composición entre Tablero y Celda, pero utilizaría una asociación entre Línea y Celda.

El motivo es que una celda pertenece al tablero y también puede formar parte de una línea, pero en UML no debería pertenecer simultáneamente a dos composiciones.

Me parece más lógico que el tablero sea el propietario de las celdas y que las líneas simplemente agrupen las que les corresponden.

Otra posibilidad sería representar que el tablero está compuesto por líneas y cada línea por sus celdas, pero habría que elegir una estructura y mantenerla.

### 2.3. No distinguir entre Juego y Partida

**Dónde aparece:**

- [Modelo del dominio, apartado 3](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#3-primera-depuración-consolidación-de-sinónimos).
- [Tabla de casos de uso](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#casos-de-uso).

**Qué se ha decidido:**

Se decide agrupar los conceptos Juego y Partida bajo un único concepto llamado Juego.

La justificación es que Partida es un término más específico de los juegos multijugador.

**Qué cambiaría y por qué:**

Yo mantendría los dos conceptos diferenciados.

Juego podría representar las reglas y el funcionamiento general del Tetris, mientras que Partida sería cada intento concreto del jugador, con su tablero, puntuación y estado.

Aunque solo juegue una persona, puede realizar muchas partidas diferentes.

Por eso, no me convence la justificación para eliminar Partida. Creo que distinguir ambos conceptos facilitaría entender qué ocurre cuando se empieza o reinicia una partida.

No considero que utilizar `iniciarPartida()` sea por sí mismo una contradicción, pero sí creo que mantener ambos conceptos sería una mejor decisión de modelado.

### 2.4. Utilizar Turno como una entidad del juego

**Dónde aparece:**

- [Modelo del dominio resultante](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#8-modelo-del-dominio-resultante).
- [Diagrama UML final](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/ModeloDominio.puml).

**Qué se ha decidido:**

Se incorpora Turno como una entidad del modelo y se relaciona con las piezas y los movimientos.

**Qué cambiaría y por qué:**

En mi opinión, eliminaría este concepto si solamente representa el tiempo durante el que cae una pieza.

En Tetris el funcionamiento es continuo, ya que las piezas van descendiendo mientras el jugador puede moverlas y girarlas.

No existe un sistema de turnos tan definido como en otros juegos, por lo que introducir este concepto podría complicar el modelo.

Si se decide mantenerlo, al menos explicaría que un turno comienza cuando aparece una pieza y termina cuando queda fijada.

De esa manera quedaría claro qué representa.

### 2.5. Considerar el Tiempo como un actor externo

**Dónde aparece:**

- [Tabla de actores](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#actores).

**Qué se ha decidido:**

Se considera que el Tiempo es un actor encargado de provocar el descenso automático de las piezas.

**Qué cambiaría y por qué:**

Yo lo representaría como un mecanismo interno del juego, siempre que el temporizador forme parte de la propia aplicación.

En los casos de uso, los actores representan entidades externas que interactúan con el sistema.

En este caso, el Tetris puede controlar internamente cuánto tiempo transcurre entre un descenso y otro.

Por eso, creo que sería más sencillo tratarlo como parte del funcionamiento del juego y no como un actor independiente.

Si realmente existiera un reloj externo que enviara eventos al programa, entonces sí tendría sentido representarlo como actor.

### 2.6. Volver a INACTIVO después de reiniciar

**Dónde aparece:**

- [Detalle de reiniciarPartida()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml).

**Qué se ha decidido:**

Cuando el jugador confirma que quiere reiniciar, se elimina el estado de la partida y el juego vuelve a `INACTIVO`.

**Qué cambiaría y por qué:**

Yo haría que, después de confirmar el reinicio, comenzara directamente una nueva partida.

Creo que cuando alguien pulsa la opción de reiniciar normalmente espera empezar otra vez desde cero, con el tablero vacío y la puntuación reiniciada.

Volver a INACTIVO obliga a realizar una acción adicional para empezar de nuevo.

Por eso, me parecería más cómodo diferenciar entre dos opciones: Reiniciar partida y Volver al inicio.

### 2.7. La cancelación del reinicio no está suficientemente representada en el contexto

**Dónde aparece:**

- [Diagrama de contexto del Jugador](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorJugador.puml).
- [Detalle de reiniciarPartida()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml).

**Qué se ha decidido:**

En el diagrama de contexto se representa `reiniciarPartida()` como una transición hacia INACTIVO.

En cambio, el caso de uso detallado incluye una confirmación que el jugador puede aceptar o cancelar.

**Qué cambiaría y por qué:**

Yo aclararía en el diagrama de contexto que el paso a INACTIVO solamente se produce cuando el jugador confirma el reinicio.

Si lo cancela, debe mantenerse en el estado anterior, ya sea EN_CURSO o FINALIZADO.

No considero que sea necesariamente una contradicción, porque un diagrama general puede omitir detalles, pero creo que añadir esta información evitaría interpretaciones equivocadas.

### 2.8. No especificar claramente el sentido de giro de las piezas

**Dónde aparece:**

- [Detalle de rotarPieza()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/rotarPieza.puml).
- [Imagen del prototipo](https://github.com/puntoReflex/pyTetris/blob/main/images/tetris.png).

**Qué se ha decidido:**

En el caso de uso se indica que el jugador puede rotar una pieza y que el sistema comprueba si esa rotación es válida.

Sin embargo, no se especifica claramente el sentido del giro.

**Qué cambiaría y por qué:**

Añadiría si las piezas pueden girar hacia la derecha, hacia la izquierda o en ambos sentidos, y explicaría qué control corresponde a cada opción.

Lo considero importante porque una misma pieza puede acabar en posiciones distintas dependiendo del sentido en el que gire.

Además, cuando una pieza está cerca de una pared, una rotación puede ser válida y otra no.

Creo que dejarlo especificado ayudaría tanto a implementar la funcionalidad como a comprobar que funciona correctamente.

### 2.9. Tratar el arranque del programa como un caso de uso principal

**Dónde aparece:**

- [Tabla de casos de uso](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#casos-de-uso).
- [Detalle de arrancarSistema()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/arrancarSistema.puml).

**Qué se ha decidido:**

Se incluye `arrancarSistema()` como uno de los casos de uso asociados al Jugador.

**Qué cambiaría y por qué:**

Yo separaría el arranque del programa de las acciones relacionadas directamente con jugar al Tetris.

Es evidente que el programa tiene que iniciarse para poder utilizarlo, pero considero que arrancar el sistema es una acción previa y no el objetivo principal del jugador.

El jugador quiere empezar una partida, mover las piezas, girarlas y conseguir puntos.

Por eso, trataría el arranque como una condición previa o como un comportamiento independiente de los casos de uso principales.

### 2.10. Los casos de uso tienen nombres de funciones

**Dónde aparece:**

- [Tabla de casos de uso](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#casos-de-uso).
- [Diagrama de casos de uso](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaCasosUso.puml).

**Qué se ha decidido:**

Los casos de uso reciben nombres como `moverPieza()`, `rotarPieza()` o `reiniciarPartida()`.

**Qué cambiaría y por qué:**

Yo utilizaría nombres como Mover pieza, Rotar pieza y Reiniciar partida.

Creo que los casos de uso deberían expresar las acciones que quiere realizar el usuario, sin parecer métodos que ya se van a programar.

La disciplina de requisitos se centra en definir qué debe hacer el sistema y no cómo se implementará.

Por eso, considero que utilizar nombres más naturales ayudaría a diferenciar los requisitos del código.

### 2.11. No se especifica suficientemente el sistema de puntuación

**Dónde aparece:**

- [Modelo del dominio](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md).
- [Detalle de descenderPieza()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/descenderPieza.puml).

**Qué se ha decidido:**

El modelo contempla la puntuación del jugador y el caso de uso de descenso indica que se actualiza cuando se eliminan líneas.

Sin embargo, no se explica cuántos puntos se obtienen al eliminar una, dos o más líneas.

**Qué cambiaría y por qué:**

Añadiría una regla que indicara cómo se calcula la puntuación.

Por ejemplo, podría establecerse una cantidad de puntos diferente dependiendo del número de líneas eliminadas simultáneamente.

Creo que es importante porque, sin esa información, dos personas podrían desarrollar el mismo juego aplicando puntuaciones diferentes.

Además, no habría una forma clara de comprobar si el sistema está calculando los puntos correctamente.

### 2.12. No se explica cómo se generan las nuevas piezas

**Dónde aparece:**

- [Modelo del dominio](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md).
- [Detalle de descenderPieza()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/descenderPieza.puml).

**Qué se ha decidido:**

El sistema genera una nueva pieza cuando la anterior queda fijada en el tablero.

Además, en el modelo del dominio se contemplan los distintos tipos de piezas.

Sin embargo, no se explica cómo se decide cuál será la siguiente pieza.

**Qué cambiaría y por qué:**

Especificaría cómo se seleccionan los tipos de piezas y si la selección es aleatoria o sigue alguna regla.

También aclararía si pueden aparecer varias piezas iguales seguidas.

Creo que sería útil porque la forma en la que aparecen las piezas influye directamente en la dificultad y en el desarrollo de la partida.

### 2.13. La pausa aparece en el modelo del dominio, pero no se desarrolla en los requisitos

**Dónde aparece:**

- [Modelo del dominio, apartado 4](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#4-análisis-de-responsabilidades-verbos).
- [Tabla de casos de uso](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#casos-de-uso).

**Qué se ha decidido:**

En el modelo del dominio se menciona pausar como una de las responsabilidades de Juego.

Sin embargo, en los requisitos no aparece ningún caso de uso relacionado con pausar o reanudar una partida.

**Qué cambiaría y por qué:**

Yo aclararía si la posibilidad de pausar forma parte del proyecto o si se ha decidido dejarla fuera.

Si se quiere incluir, añadiría un caso de uso para pausar y reanudar, junto con un estado PAUSADO.

Si no se quiere implementar, indicaría expresamente que esa funcionalidad no forma parte de esta versión.

No creo que sea obligatorio añadir una pausa, pero sí considero importante que quede claro el alcance del sistema.

### 2.14. No se definen las velocidades de caída con suficiente precisión

**Dónde aparece:**

- [Tabla de casos de uso](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#casos-de-uso).
- [Detalle de alternarVelocidadCaida()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/alternarVelocidadCaida.puml).

**Qué se ha decidido:**

Se establece que el jugador puede alternar entre una velocidad normal y una velocidad acelerada.

Sin embargo, no se explica con precisión cómo se diferencian esas velocidades ni qué sucede cuando aparece una nueva pieza.

**Qué cambiaría y por qué:**

Definiría los intervalos de caída para cada velocidad o, al menos, la relación entre ambas.

También aclararía si la velocidad vuelve a ser normal cuando aparece una pieza nueva o si se mantiene la velocidad seleccionada.

Creo que estas reglas son necesarias para saber cómo debería comportarse el juego y poder comprobarlo mediante pruebas.

### 2.15. No concretar las dimensiones del tablero en el modelo final

**Dónde aparece:**

- [Modelo del dominio, identificación de conceptos](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#2-extracción-de-sustantivos-identificación-de-conceptos).
- [Diagrama UML final](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/ModeloDominio.puml).

**Qué se ha decidido:**

En la identificación inicial de conceptos se menciona un tablero de 10 columnas y 20 filas.

Sin embargo, el diagrama final no representa esas dimensiones.

**Qué cambiaría y por qué:**

Yo indicaría las dimensiones del tablero como una restricción del dominio, siempre que ese sea el tamaño fijado para esta versión del juego.

No creo que sea necesario explicar cómo se almacenarán las celdas, pero sí dejar claro cuántas filas y columnas tiene el tablero.

Considero que esto ayudaría a comprender sus reglas y a comprobar que la versión desarrollada respeta las dimensiones previstas.

### 2.16. El modelo del dominio elimina al Jugador

**Dónde aparece:**

- [Modelo del dominio, apartado 6](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#6-decisiones-críticas-de-modelado).
- [Tabla de actores](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#actores).

**Qué se ha decidido:**

Se decide no incluir Jugador en el modelo del dominio porque se considera que, al tratarse de un juego individual, no es necesario representarlo.

**Qué cambiaría y por qué:**

Yo valoraría mantener el concepto Jugador si se quisiera guardar información sobre sus puntuaciones, estadísticas o partidas anteriores.

Que solo participe una persona no significa que ese concepto no pueda ser útil.

De todas formas, no considero que sea un error que aparezca como actor en los requisitos y no como clase del dominio, porque los actores y las entidades tienen funciones diferentes.

Lo que cuestiono es que se descarte únicamente por tratarse de un juego individual.

### 2.17. Los diagramas de contexto representan principalmente estados

**Dónde aparece:**

- [Apartado Diagramas de contexto](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#presentar-el-modelo-de-casos-de-uso-como-un-todo).
- [Diagrama general de estados](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto000.puml).

**Qué se ha decidido:**

En el apartado de diagramas de contexto se utilizan diagramas que muestran los estados INACTIVO, EN_CURSO y FINALIZADO, junto con las transiciones entre ellos.

**Qué cambiaría y por qué:**

Yo los identificaría como diagramas de estados, ya que representan principalmente los cambios de estado del sistema.

Para mostrar el contexto del juego utilizaría también un diagrama que representara los actores externos y sus interacciones con el Tetris.

Creo que sería más claro porque un diagrama de contexto y un diagrama de estados tienen objetivos diferentes y no ofrecen exactamente la misma información.

### 2.18. No se establecen criterios de aceptación claros para los casos de uso

**Dónde aparece:**

- [Apartado Detallar casos de uso](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#detallar-casos-de-uso).

**Qué se ha decidido:**

Los casos de uso se explican principalmente mediante diagramas de estados y transiciones.

Estos diagramas describen las acciones que realiza el jugador y las respuestas del sistema, pero no siempre establecen todas las condiciones necesarias para comprobar su funcionamiento.

**Qué cambiaría y por qué:**

Yo añadiría una pequeña descripción escrita para cada caso de uso, indicando sus condiciones previas, el resultado esperado y las situaciones alternativas.

Por ejemplo, en el movimiento de una pieza se podría indicar qué ocurre cuando intenta salir del tablero o chocar con otra pieza.

Creo que así sería más fácil preparar pruebas y comprobar si el programa cumple todos los requisitos.

---

## 3. Conclusión

Después de revisar la documentación del Tetris, considero que el proyecto está bastante desarrollado y permite entender cómo se han aplicado el modelo del dominio y la disciplina de requisitos. Sin embargo, también he encontrado algunos aspectos que se podrían mejorar.

Lo que más me ha llamado la atención es que hay elementos que no se explican exactamente igual en todos los documentos. Por ejemplo, algunas transiciones entre estados, los nombres de ciertas operaciones o las relaciones que aparecen en el modelo final.

También he encontrado decisiones con las que no estoy del todo de acuerdo, como considerar el Tiempo un actor externo, utilizar Turno como una entidad o hacer que el jugador tenga que volver a iniciar después de reiniciar una partida.

En algunos casos creo que existe una diferencia clara entre los documentos y, en otros, simplemente pienso que se podría haber elegido una solución más sencilla o haber explicado mejor el funcionamiento.

En mi opinión, lo más importante de este reto es entender que no basta con realizar un diagrama o escribir unos requisitos por separado. Todos los documentos tienen que estar relacionados y permitir comprender el mismo sistema.

Además, considero que justificar las decisiones que se toman durante el modelado es importante, porque no siempre existe una única solución correcta. Lo fundamental es que las decisiones tengan sentido y se puedan defender.

En definitiva, este ejercicio me ha servido para comprender mejor la importancia de revisar los modelos, comparar la información entre los distintos documentos y detectar posibles problemas antes de comenzar con el desarrollo de un programa.
