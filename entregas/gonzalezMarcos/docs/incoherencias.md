# Incoherencias

[Presentación](../README.md) · [Fuentes](fuentes.md) · [Decisiones discutibles](decisiones-discutibles.md) · [Propuesta](propuesta.md)

Se registran **tres incoherencias**. Las omisiones que no permiten demostrar dos afirmaciones incompatibles se analizan en el otro flanco, sin inflar este recuento.

## I01 · Se ofrece acelerarCaida, pero se especifica alternarVelocidadCaida

**Dónde y qué dice cada artefacto.** El [catálogo de requisitos, tabla de casos de uso][REQ] y el [diagrama de casos de uso][CU] identifican `alternarVelocidadCaida()`. Su [detalle][VEL] describe tanto el paso de velocidad normal a acelerada como el inverso. En cambio, el [detalle de iniciarPartida, última respuesta del sistema][INI] permite solicitar `acelerarCaida()`, que no figura en el catálogo ni tiene detalle propio.

**Por qué es una incoherencia.** La lista de solicitudes disponibles al entrar en `EN_CURSO` utiliza otra operación y expresa solo uno de los sentidos del cambio. No es una simple diferencia de mayúsculas: acelerar y alternar no prometen el mismo comportamiento cuando la velocidad ya es acelerada.

**Qué corregiría y por qué.** Sustituiría `acelerarCaida()` por `alternarVelocidadCaida()` en INI y regeneraría su SVG. Mantendría el nombre y el comportamiento del catálogo, del contexto del jugador y de VEL, que ya concuerdan. No añadiría un octavo caso de uso para justificar un nombre aislado.

**Comprobación.** Tras iniciar, el jugador puede alternar de normal a acelerada y de acelerada a normal mediante la misma solicitud.

## I02 · El diagrama global omite un cambio de estado admitido por el contexto del jugador

**Dónde y qué dice cada artefacto.** En [EST, conjunto de transiciones][EST], desde `EnCurso` solo se alcanza `EnCurso` o `Finalizado`; el retorno a `Inactivo` parte únicamente de `Finalizado`. En [JUG][JUG] existe además `EnCurso → Inactivo : reiniciarPartida()`. El [detalle REI][REI] admite entrar desde `EN_CURSO` y terminar en `INACTIVO` cuando se confirma.

**Por qué es una incoherencia.** La vista presentada como estados del sistema no recoge una transición entre los mismos estados que sí admiten la vista por actor y el detalle. Una vista global puede omitir notas y pasos internos, pero debería conservar los cambios de estado que resume. EST no se identifica como una vista parcial.

**Qué corregiría y por qué.** Añadiría a EST el retorno de `EN_CURSO` a `INACTIVO` por reinicio confirmado. Mantendría el reinicio durante la partida porque JUG y REI lo especifican de forma coincidente. Las cancelaciones deben conservar el estado de origen; su representación ambigua se trata en D15 y D16.

**Comprobación.** Una partida en curso puede reiniciarse, previa confirmación, sin pasar por `FINALIZADO`. El mismo recorrido debe poder seguirse en las tres vistas.

## I03 · Nivel se conserva como propiedad y después se excluye del alcance

**Dónde y qué dice cada artefacto.** En [MD, §6, «¿Puntuación y Nivel como entidades?»][MD], ambos se excluyen **como entidades**, pero se conservan como propiedades de `Juego`. En la tabla **Simplificaciones conscientes** de la misma página se establece que no hay niveles de dificultad. En §4, el nivel se relaciona precisamente con el aumento de dificultad y la velocidad.

**Por qué es una incoherencia.** No se trata de que un concepto aparezca en la lluvia de ideas y luego se descarte: hay dos decisiones explícitas sobre su inclusión en el modelo vigente. Una lo mantiene como propiedad y la otra elimina la característica, sin declarar que sustituye a la anterior. Un lector no puede determinar si debe representar el nivel.

**Qué corregiría y por qué.** Mantendría la simplificación sin niveles, ya que ningún caso de uso exige progresión de dificultad. Corregiría §6 para conservar solo puntuación y declarar nivel descartado del alcance. Si se desea conservar la decisión antigua como historia, la marcaría como superada. Alternar manualmente dos velocidades no introduce niveles de dificultad.

**Comprobación.** El alcance, el vocabulario vigente y el modelo corregido no incluyen nivel. La velocidad normal o acelerada queda definida por separado.

## Aspectos que no se cuentan como incoherencias

- **Jugador fuera del modelo del dominio y dentro del modelo de casos de uso:** son perspectivas distintas. Se discute la justificación de exclusión en D09, no la presencia del actor.
- **Tiempo como actor:** es una convención admitida por el material docente.
- **Cardinalidades y atributos ausentes del dibujo:** el dibujo pierde información, pero no afirma necesariamente valores contrarios a la tabla; véanse D02–D04.
- **Giro con dos sentidos en el prototipo y sin parámetro en el detalle:** el detalle es incompleto, no niega expresamente uno de los sentidos; véase D21.
- **Velocidad descrita solo como aumento en el texto del prototipo:** es una explicación parcial del caso alternar, tratada en D26.
- **Diferencias entre versiones históricas:** son evolución legítima cuando están identificadas como tales.

[MD]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/docs/modeloDelDominio.md
[REQ]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/docs/ProcesoRequisitos.md
[DOM]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/modelosUML/ModeloDominio.puml
[CU]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/modelosUML/DiagramaCasosUso.puml
[EST]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/modelosUML/DiagramaDeContexto000.puml
[JUG]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/modelosUML/DiagramaDeContexto001-ActorJugador.puml
[TMP]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/modelosUML/DiagramaDeContexto001-ActorTiempo.puml
[IMG]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/images/tetris.png
[DES]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/modelosUML/detalleCasosUso/descenderPieza.puml
[INI]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/modelosUML/detalleCasosUso/iniciarPartida.puml
[MOV]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/modelosUML/detalleCasosUso/moverPieza.puml
[REI]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/modelosUML/detalleCasosUso/reiniciarPartida.puml
[ROT]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/modelosUML/detalleCasosUso/rotarPieza.puml
[VEL]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/modelosUML/detalleCasosUso/alternarVelocidadCaida.puml
[ARR]: https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/modelosUML/detalleCasosUso/arrancarSistema.puml
