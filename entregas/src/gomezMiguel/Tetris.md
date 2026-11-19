# Reto 002 - Revisión de pyTetris

## 1. Incoherencias

### 1.1. Cambio de nombre en la velocidad de caída
- **Dónde:** En la [tabla de casos de uso](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#casos-de-uso) y en el [detalle de inicio de partida](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/iniciarPartida.puml).
- **Qué dice cada uno:** En la tabla general la acción se llama `alternarVelocidadCaida()`, mientras que al detallar qué ocurre al empezar a jugar aparece como `acelerarCaida()`. No son lo mismo una aumenta y la otra cambia de vlocidad de una
- **Qué corregiría y por qué:** Usaría `alternarVelocidadCaida()` en los dos sitios para mantener un único nombre y dejar claro que se puede volver a la velocidad normal.

### 1.2. Confusión entre «Juego» y «Partida»
- **Dónde:** En la [depuración de sinónimos del dominio](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#3-primera-depuración-consolidación-de-sinónimos) y en la [lista de casos de uso](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#casos-de-uso).
- **Qué dice cada uno:** En el modelo del dominio se descarta el término «Partida» alegando que es propio de juegos multijugador y se unifica todo bajo «Juego». Sin embargo, en requisitos desaparece «Juego» y todo gira en torno a «Partida» (iniciar partida, reiniciar partida, partida iniciada).
- **Qué corregiría y por qué:** Mantendría ambos conceptos separados: **Juego** para la aplicación general y sus reglas, y **Partida** para cada intento individual del jugador. Así el glosario queda alineado con requisitos.

### 1.3. Contradicción sobre los niveles de dificultad
- **Dónde:** En el [documento del modelo del dominio](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md) (identificación de conceptos frente a simplificaciones).
- **Qué dice cada uno:** Al principio se excluye el nivel como entidad diciendo que es una propiedad de Juego, pero más abajo, en las simplificaciones conscientes, se afirma directamente «Sin niveles de dificultad».
- **Qué corregiría y por qué:** Eliminaría la referencia al nivel como propiedad si queda fuera del alcance, o aclararía si se refiere al aumento progresivo de velocidad. Esta y como que no esta 

### 1.4. La caída de la pieza se muestra antes de comprobar si es válida
- **Dónde:** En el [detalle de descenso de pieza](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/descenderPieza.puml) comparado con el [detalle de movimiento lateral](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/moverPieza.puml).
- **Qué dice cada uno:** Al mover la pieza hacia los lados, primero se comprueba si la nueva posición está libre antes de dibujarla. Al descender, en cambio, primero se muestra la pieza una fila más abajo y después se verifica si ha chocado o llegado al fondo.
- **Qué corregiría y por qué:** Invertiría el orden en el descenso para validar primero si puede bajar. Si está libre, se actualiza la posición; si no, se queda en su sitio y pasa a fijarse.

### 1.5. El reinicio puede cancelarse, pero los diagramas de contexto no lo reflejan
- **Dónde:** En el [diagrama general de estados](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto000.puml), el [contexto del Jugador](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorJugador.puml) y el [detalle del reinicio](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml).
- **Qué dice cada uno:** El diagrama general omite el paso directo desde partida en curso a inactivo, y el del Jugador pinta el reinicio como un salto automático a inactivo. Por el contrario, el detalle del caso de uso permite cancelar el reinicio y conservar el estado en el que estaba.
- **Qué corregiría y por qué:** Yo Añadiría la transición al diagrama general e indicaría en ambos contextos que volver a inactivo ocurre únicamente si el usuario confirma la acción.

### 1.6. Transición de reinicio atribuida al Tiempo
- **Dónde:** En el [contexto del actor Tiempo](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorTiempo.puml) y en el [contexto del Jugador](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorJugador.puml).
- **Qué dice cada uno:** En el diagrama del Tiempo aparece una flecha sin explicación desde finalizado hacia inactivo, cuando en el diagrama del Jugador ese mismo cambio depende de que el usuario reinicie la partida.
- **Qué corregiría y por qué:** Quitaría esa transición del contexto del Tiempo. El rejo pues bja las piezas pero al final nunca deja reiniciar.

### 1.7. Desfase entre el texto del dominio, los atributos y el diagrama final
- **Dónde:** En el [refinamiento del dominio](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#7-refinamiento-de-relaciones) y en el [diagrama UML final](https://github.com/puntoReflex/pyTetris/blob/main/images/modelosUML/ModeloDominio.svg).
- **Qué dice cada uno:** En el texto se definen los atributos puntuación y velocidad para Juego y se detallan seis relaciones con sus cantidades. En el diagrama final faltan esos atributos, no aparecen las multiplicidades en las líneas y surgen relaciones nuevas (como Turno–Pieza o Turno–Movimiento) ausentes en la tabla previa.
- **Qué corregiría y por qué:** Sincronizaría ambos artefactos añadiendo los atributos y cantidades al dibujo y completando la tabla escrita para que explique exactamente lo mismo.

### 1.8. El prototipo visual no enseña la confirmación de reinicio ni el cambio de velocidad
- **Dónde:** En la [imagen del prototipo](https://github.com/puntoReflex/pyTetris/blob/main/images/tetris.png), el [apartado de prototipado](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#prototipar-casos-de-uso) y los detalles de [reinicio](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml) y [cambio de velocidad](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/alternarVelocidadCaida.puml).
- **Qué dice cada uno:** El documento asegura que esa captura basta para ilustrar todos los casos de uso, pero en la imagen solo se ve una partida en curso; no muestra el aviso de confirmación de reinicio ni la tecla para alternar velocidad.
- **Qué corregiría y por qué:** Incluiría un boceto de la pantalla de confirmación (aceptar o cancelar) y añadiría el control de velocidad en la leyenda de teclas.

---

## 2. Decisiones discutibles

### 2.1. Doble composición sobre una misma Celda
- **Dónde:** En el [refinamiento de relaciones](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#7-refinamiento-de-relaciones) y el [diagrama del dominio](https://github.com/puntoReflex/pyTetris/blob/main/images/modelosUML/ModeloDominio.svg).
- **Qué se decidió:** Representar mediante composición tanto la relación Tablero con Celda como Línea–Celda.
- **Qué propongo y por qué:** Mantendría la composición solo entre Tablero y Celda (que es el contenedor real de la cuadrícula) y usaría una asociación simple entre Línea y Celda. Una misma instancia no debe depender por composición de dos propietarios distintos a la vez.

### 2.2. Permitir varias «Piezas activas» a la vez
- **Dónde:** En la [tabla de relaciones del dominio](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#7-refinamiento-de-relaciones).
- **Qué se decidió:** Asociar Juego con varias piezas activas mediante una relación de uno a muchos.
- **Qué propongo y por qué:** Limitaría la pieza activa a una sola unidad, diferenciándola de las piezas ya fijadas en el tablero. Durante una partida normal el jugador solo controla una pieza cayendo cada vez.

### 2.3. Modelar «Turno» y «Movimiento» como clases del dominio
- **Dónde:** En el [modelo del dominio resultante](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#8-modelo-del-dominio-resultante) y en el [diagrama final](https://github.com/puntoReflex/pyTetris/blob/main/images/modelosUML/ModeloDominio.svg).
- **Qué se decidió:** Incluir las entidades Turno y Movimiento conectadas con Pieza.
- **Qué propongo y por qué:** Eliminaría ambas entidades del dominio. El Tetris es un juego continuo en tiempo real, no por turnos, y los movimientos son acciones puntuales del jugador (ya cubiertas por los casos de uso) sin datos propios ni historial que almacenar.

### 2.4. Considerar el «Tiempo» como un actor externo
- **Dónde:** En la [tabla de actores](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#actores).
- **Qué se decidió:** Definir Tiempo como un actor que dispara el descenso automático de las piezas.
- **Qué propongo y por qué:** Lo gestionaría como un temporizador interno del sistema. Un actor representa un agente externo que interactúa con el software, mientras que la caída periódica forma parte de las reglas internas del juego.

### 2.5. Usar nombres de métodos de código y acciones internas en requisitos
- **Dónde:** En el [diagrama de casos de uso](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaCasosUso.puml) y en el [detalle de reinicio](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml).
- **Qué se decidió:** Nombrar los casos de uso con paréntesis (como `moverPieza()` o `iniciarPartida()`) y describir pasos internos como «Sistema limpia estado de partida».
- **Qué propongo y por qué:** Los nombraría en lenguaje mas natural («Mover pieza», «Iniciar partida») y sustituiría las acciones que ocurren las puede ver el usuario, como mostrar el tablero vacío y la puntuación a cero.

### 2.6. Omitir el sentido de rotación y las reglas de puntuación
- **Dónde:** En los detalles de [rotación](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/rotarPieza.puml) y [descenso](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/descenderPieza.puml), junto con el [prototipo](https://github.com/puntoReflex/pyTetris/blob/main/images/tetris.png).
- **Qué se decidió:** Indicar simplemente que la pieza rota (aunque el prototipo muestra dos teclas distintas de giro) y mencionar que se actualiza la puntuación sin definir cómo se calcula.
- **Qué propongo y por qué:** Especificaría el sentido de giro (horario o antihorario) como dato de entrada y definiría una regla básica de puntos por líneas completadas para evitar implementaciones ambiguas.

### 2.7. Volver a estado inactivo tras confirmar un reinicio y mezclar el arranque del programa
- **Dónde:** En el [detalle de reinicio](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml) y en el [arranque del sistema](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/arrancarSistema.puml).
- **Qué se decidió:** Incluir arrancar el sistema al mismo nivel que jugar y hacer que confirmar un reinicio devuelva el juego al estado inactivo.
- **Qué propongo y por qué:** Separaría abrir la aplicación de los casos de uso propios de la partida, y haría que confirmar un reinicio lanzase directamente una nueva partida jugable desde cero sin obligar a pulsar iniciar después.