# Reto 002 – Tetris: fallos y decisiones discutibles

**Jaime Lomo** · Ingeniería del Software I

He revisado sobre todo los dos documentos del proyecto [pyTetris](https://github.com/puntoReflex/pyTetris): el [modelo del dominio][md] y el [proceso de requisitos][req], además de la [imagen del prototipo][img].

## 1. Incoherencias (cosas que se contradicen)

| # | Dónde están | Qué dice cada uno | Cuál corregiría | Por qué |
|-|-|-|-|-|
| 1 | [Modelo del dominio][md] (§3) y [requisitos][req] | El modelo dice que se queda con «Juego» y descarta «Partida». Los requisitos hablan de `iniciarPartida()`, `reiniciarPartida()` y «partida iniciada» | El **modelo** | Una partida es cada intento de jugar, y es lo que cambia de estado (inactiva, en curso, finalizada). Si el cliente habla de partidas, el modelo debería tenerlas |
| 2 | [Modelo del dominio][md] (§1, §5 y §7) y [requisitos][req] | La tabla de relaciones (§7) dice que un juego tiene **varias** piezas activas. Pero el propio modelo dice «mueves la pieza actual» y «cada turno involucra una pieza», y los requisitos siempre hablan de «la pieza activa» | La **tabla del modelo**: una sola pieza activa a la vez | Todo lo demás da por hecho que el jugador controla una única pieza |
| 3 | [Modelo del dominio][md] (§6 y «Simplificaciones») y [requisitos][req] | El modelo dice que Nivel y Puntuación son datos del Juego, y a la vez que no hay niveles de dificultad. Los requisitos sí usan puntuación (empieza en 0) y dos velocidades | El **modelo**: quitar Nivel y dejar claro que puntuación y velocidad son datos del juego | Se guarda el nivel como dato y a la vez se dice que no existe. Un lector no sabe si hay niveles o no |
| 4 | [Modelo del dominio][md] (§4) y [requisitos][req] | Entre las acciones del juego se menciona «pausar», pero ningún caso de uso ni estado de los requisitos habla de pausa | El **modelo**: quitar «pausar» | El modelo habla de algo que el sistema no hace |
| 5 | [Prototipo (texto e imagen)][img] y [requisitos][req] | El texto dice que la imagen muestra las teclas 4, 6, 7 y 9 y que lo demás está «implícito». En la imagen solo hay mover, y dos rotaciones, y no hay forma de cambiar la velocidad ni de reiniciar | El **prototipo**: añadir esos comandos, o decir que esos casos de uso no están representados | Dos de los siete casos de uso no se pueden pedir desde la pantalla de ejemplo, y el texto da a entender lo contrario |

## 2. Decisiones discutibles (no son errores, pero yo lo habría hecho distinto)

| # | Dónde | Qué decidieron | Qué haría yo | Por qué |
|-|-|-|-|-|
| 1 | [`ProcesoRequisitos.md`][req] | Pasan de buscar los casos de uso a detallarlos todos, sin decidir cuáles van primero | Ordenarlos antes. Primero `arrancarSistema()`, `iniciarPartida()` y `descenderPieza()`; después `rotarPieza()` y mover la pieza; al final cambiar la velocidad y reiniciar | En clase se dice que se empieza por los de más riesgo. En Tetris el riesgo está en los choques, las líneas completas y el fin de partida (todo en `descenderPieza`), no en los menús |
| 2 | [Tabla de actores][req] | El Tiempo pide bajar la pieza «según el intervalo configurado», pero no dice cuál es ni quién lo configura | Añadir un requisito que diga cada cuánto baja la pieza, normal y acelerada | Sin ese dato no se puede comprobar si el descenso funciona como se espera |
| 3 | [Modelo del dominio][md] (§3 y §4) | `Turno` y `Movimiento` aparecen como cosas del dominio | Quitarlas del modelo | Un movimiento solo ocurre cuando el jugador pulsa una tecla y nadie lo guarda: lo que queda es la pieza en su nueva posición, y eso ya está recogido en los casos de uso. Y Tetris no va por turnos: la pieza baja sola con el tiempo, y «turno» se define como un «ciclo de juego», que es una idea del programa |
| 4 | [Modelo del dominio][md] (§4) | Para decidir qué es una clase miran lo que «hace» el programa («validar posiciones», «ejecutarse») | Usar eso solo para encontrar relaciones | El modelo del dominio describe el mundo del cliente, no el programa por dentro |
| 5 | [Modelo del dominio][md] (§9) | La validación con expertos se responde «sí/no», sin decir quién la hizo | Contrastarlo con jugadores y dejarlo escrito | En clase se cuenta que no validar es un fallo típico. Además, decir que «no falta ningún concepto» choca con la incoherencia 3 |

## 3. Otros comentarios (menores, no los cuento como fallos)

- En el modelo, «Rotación, Caída y Desplazamiento» se juntan en «Movimiento» ([§3][md]), pero en los casos de uso `moverPieza()` solo sirve para izquierda y derecha ([detalle][mover]). Se podría llamar igual en los dos sitios.
- `iniciarPartida` ofrece `acelerarCaida()` ([detalle][iniciar]), mientras que en el resto se llama `alternarVelocidadCaida()` ([detalle][alternar]).
- La tabla de relaciones del [modelo del dominio][md] (apartado 7) indica cuántos hay de cada cosa (por ejemplo, un tablero tiene muchas celdas), pero el [diagrama][modeloPuml] no pone esos números ni los datos de cada clase, así que hay que mirar los dos para tener el modelo completo.
- Los diagramas de contexto no coinciden del todo entre sí: el [general][ctx000] no tiene la vuelta de «en curso» a «inactivo» y el [del jugador][ctxJ] no recoge cuando se cancela un reinicio.
- En [`descenderPieza`][descender] un solo caso de uso hace muchas cosas (bajar, fijar, eliminar líneas, puntuar, sacar otra pieza y decidir si se acaba), y siempre elimina líneas sin preguntar si hay alguna completa.
- En [`reiniciarPartida`][reiniciar] se pide confirmación incluso cuando la partida ya ha terminado, y dice que «el Sistema limpia el estado», algo que el jugador no ve.

[md]: https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md
[req]: https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md
[img]: https://github.com/puntoReflex/pyTetris/blob/main/images/tetris.png
[modeloPuml]: https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/ModeloDominio.puml
[ctx000]: https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto000.puml
[ctxJ]: https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorJugador.puml
[mover]: https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/moverPieza.puml
[descender]: https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/descenderPieza.puml
[iniciar]: https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/iniciarPartida.puml
[reiniciar]: https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml
[alternar]: https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/alternarVelocidadCaida.puml