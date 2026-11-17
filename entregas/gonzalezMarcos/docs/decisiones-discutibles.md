# Decisiones discutibles

[Presentación](../README.md) · [Fuentes](fuentes.md) · [Incoherencias](incoherencias.md) · [Propuesta](propuesta.md)

Se documentan **28 decisiones discutibles**. Se incluyen elecciones de representación que dejan comportamientos sin determinar. Cada ficha separa lo que se observa, la alternativa propuesta y el argumento; ninguna presupone un fallo del programa.

## Dominio

### D01 · Una celda tiene dos propietarios por composición

**Decisión y ubicación.** [MD §7][MD] y [DOM][DOM] usan composición tanto entre `Tablero–Celda` como entre `Linea–Celda`, a la vez que el tablero contiene líneas. Las líneas agrupan las mismas celdas del tablero.

**Propuesta.** Mantener `Tablero *-- Celda`; representar `Linea -- Celda` como agrupación mediante asociación, indicando que pertenecen al mismo tablero.

**Argumento.** Una celda concreta no puede pertenecer simultáneamente por composición a dos propietarios. Es una elección repetida de forma consistente en tabla y dibujo, pero con una semántica inadecuada. Cambiar el rombo de Línea permite compartir la referencia sin duplicar la celda. La restricción procede de [UML 2.5.1, §9.5.3, p. 112](https://www.omg.org/spec/UML/2.5.1/PDF).

### D02 · Las multiplicidades refinadas no aparecen en el resultado

**Decisión y ubicación.** [MD §7][MD] especifica relaciones `1:1`, `1:*` y `*:1`; [DOM][DOM] dibuja las asociaciones sin multiplicidades.

**Propuesta.** Incorporar a la vista final multiplicidades explícitas y restricciones que distingan el tablero completo, las filas y la pieza activa. Para la variante propuesta: un tablero por partida, 200 celdas y 20 filas de 10 celdas.

**Argumento.** El artefacto que se presenta como resultado no permite recuperar el refinamiento anunciado. No deduzco una multiplicidad uno de la ausencia de etiquetas: señalo información perdida. Las dimensiones propuestas concretan el ejemplo 10×20 de MD §2; no se presentan como una obligación de toda variante de Tetris.

### D03 · La relación de piezas activas admite una colección sin precisar su ciclo de vida

**Decisión y ubicación.** [MD §7][MD] rotula `Juego–Pieza (activas)` como agregación `1:*`; [DOM][DOM] mantiene la etiqueta de piezas activas. Los [detalles INI][INI] y [DES][DES] operan sobre una pieza activa cada vez.

**Propuesta.** Expresar `0..1` pieza activa, con exactamente una mientras se juega, y aclarar qué ocurre con sus bloques cuando queda fijada. Definir también el turno: empieza al aparecer una pieza y termina al fijarla; no equivale a cada pulsación.

**Argumento.** Una relación genérica no explica las fases sin pieza ni limita las simultáneas. Que una pieza pueda imaginarse fuera de una partida no justifica por sí solo la agregación. Una asociación con un rol preciso comunica mejor la regla que necesita el descenso. La tabla no obliga a tener varias piezas activas, por eso se trata como imprecisión, no como contradicción.

### D04 · Puntuación y velocidad se declaran atributos, pero quedan invisibles

**Decisión y ubicación.** [MD §6 y tabla de descartados][MD] conserva puntuación y velocidad como atributos de Juego. [DOM][DOM] no muestra atributos en ninguna clase.

**Propuesta.** Mostrar al menos puntuación y modo de caída, además de los datos conceptuales necesarios para expresar posiciones y ocupación. No incluir métodos, tipos de programación ni estructuras de almacenamiento.

**Argumento.** Excluir una entidad no debería borrar la información que se decidió conservar como atributo. Estos datos son necesarios para entender las respuestas observables de INI, DES y VEL. Un diagrama sin atributos puede servir como primera aproximación; cuesta sostenerlo como modelo final suficiente para esas reglas.

### D05 · Juego y Partida se unifican con una justificación débil

**Decisión y ubicación.** [MD §3][MD] elige Juego porque considera Partida más específico del multijugador. El catálogo utiliza nombres como `iniciarPartida` y `reiniciarPartida`.

**Propuesta.** Usar **Partida** para una sesión concreta y **Sistema Tetris** para la aplicación, o declarar explícitamente Juego como sinónimo de Partida. Mantener un glosario que unifique también `EnCurso` y `EN_CURSO`.

**Argumento.** Una sesión individual también necesita un comienzo, un resultado y un fin. El número de jugadores no resuelve esa distinción. Nombrarla con precisión evita confundir arrancar la aplicación con empezar una partida; no afirmo que usar la palabra Juego sea incorrecto en sí mismo.

### D06 · Descartar Bloque dificulta explicar la eliminación parcial de una pieza

**Decisión y ubicación.** [MD §3 y descartados][MD] elimina Bloque porque sería parte inseparable de Pieza; [DOM][DOM] relaciona Pieza directamente con Celda.

**Propuesta.** Distinguir el espacio del tablero (celda) de la ocupación fijada (bloque), o documentar expresamente que, al fijarse, la pieza deja de conservar su identidad y se transforma en ocupaciones independientes.

**Argumento.** Al borrar una fila pueden desaparecer algunos bloques de una pieza y permanecer otros. La afirmación de que los bloques no existen independientemente deja sin explicar ese caso básico. No es obligatorio añadir una clase Bloque: lo obligatorio para un modelo comprensible es explicar qué queda después del borrado.

### D07 · Línea confunde una fila con la condición de estar completa

**Decisión y ubicación.** [MD §2][MD] define Línea como fila horizontal completa; §7 la trata como una agrupación de celdas del tablero y [DOM][DOM] la conserva como parte estructural.

**Propuesta.** Definir **Fila** como agrupación de celdas de igual altura, y **fila completa** como la condición de que todas estén ocupadas por bloques fijados. Usar esos términos de manera uniforme.

**Argumento.** Una fila existe aunque esté vacía. Si Línea solo existe cuando está completa, hace falta explicar cuándo aparece y deja de existir, en vez de utilizarla indistintamente para ambas cosas. La ambigüedad impide decidir qué representa la composición Tablero–Línea, aunque podría resolverse con cualquiera de las dos convenciones.

### D08 · Tener verbos no basta para decidir qué es una entidad

**Decisión y ubicación.** [MD §4][MD] clasifica conceptos a partir de verbos; en los descartados se excluye Columna por carecer de comportamiento propio.

**Propuesta.** Decidir la inclusión por su significado, sus relaciones y la información necesaria para las reglas. Conservar Columna como coordenada si no necesita identidad; explicar por qué Movimiento y Turno merecen conceptos separados.

**Argumento.** Un modelo conceptual no es una lista de clases que deban tener métodos. Una posición puede ser relevante sin ejecutar acciones, y una acción no necesita convertirse siempre en entidad. El criterio actual anticipa responsabilidades de software y no explica suficientemente la granularidad elegida.

### D09 · Ser monojugador no es razón suficiente para excluir al jugador del dominio

**Decisión y ubicación.** [MD §6 y descartados][MD] excluye Jugador por ser implícito al no haber varios jugadores.

**Propuesta.** Mantener su exclusión en este ejemplo, pero justificarla por no necesitar identidad, perfil, resultados por persona ni otras relaciones persistentes del participante.

**Argumento.** Puede haber conceptos relevantes con una sola instancia. La exclusión es defendible por el alcance de información, no por la cardinalidad. Esta corrección de razonamiento es compatible con mantener Jugador como actor externo.

### D10 · TipoPieza no concreta los siete tipos ni las reglas geométricas

**Decisión y ubicación.** [MD §6][MD] justifica TipoPieza por la existencia de siete tipos; [DOM][DOM] solo incluye el nombre del concepto. [ROT][ROT] habla de rotación válida sin una regla geométrica asociada.

**Propuesta.** Enumerar los siete tipos y definir sus cuatro posiciones relativas, orientaciones permitidas y regla de giro en una tabla conceptual. Documentar la regla de aparición de cada tipo.

**Argumento.** La etiqueta TipoPieza no permite determinar si un giro o una aparición son válidos. No hace falta introducir matrices de implementación ni adoptar un sistema de rotación avanzado, pero sí una geometría verificable para la variante elegida.

### D11 · Siguiente pieza y sombra se descartan únicamente por su presentación

**Decisión y ubicación.** [MD, descartados y lecciones][MD] clasifica Preview/Siguiente y Sombra/Ghost como cuestiones de interfaz ajenas al dominio.

**Propuesta.** Mantenerlas fuera de este ejercicio por alcance, distinguiendo entre la representación visual y la información o regla que representarían: próxima pieza o posición de aterrizaje.

**Argumento.** Que algo aparezca en la interfaz no demuestra que carezca de significado conceptual. La puntuación también se presenta al jugador. Esta justificación evita convertir una simplificación válida en una regla general incorrecta sobre el modelado.

### D12 · Pausar aparece como responsabilidad sin una decisión posterior de alcance

**Decisión y ubicación.** [MD §4][MD] asocia pausar a Juego. El [catálogo][REQ], los contextos y los siete detalles no desarrollan una solicitud de pausa.

**Propuesta.** Registrar pausar como candidato descartado o pospuesto para esta versión. Si se desea incorporarlo, actualizar conjuntamente estados, casos de uso, tiempo y prototipo.

**Argumento.** Los conceptos iniciales no obligan a implementar todas las funciones imaginadas. El problema es dejar sin rastro la decisión: no se distingue una exclusión consciente de un requisito olvidado. No se cuenta como incoherencia porque §4 forma parte del proceso exploratorio.

### D13 · Se declara validación completa sin evidencias reproducibles

**Decisión y ubicación.** [MD §1, §9 y conclusión][MD] menciona reglas oficiales, validación con expertos y cobertura completa, pero no identifica versión de reglas, participantes, escenarios ni resultados concretos.

**Propuesta.** Identificar la variante de Tetris y registrar escenarios revisados, dudas y decisiones. Si no se dispone de esa información, describir la tabla como autoevaluación pendiente de contraste.

**Argumento.** Una serie de respuestas afirmativas no permite repetir la validación ni conocer qué significa completo. No se afirma que la validación no se hiciera; se cuestiona la evidencia documental disponible para sostenerla.

## Contextos y detalles de casos de uso

### D14 · Las fronteras del sistema y la terminación quedan implícitas

**Decisión y ubicación.** [CU][CU] no dibuja el límite del sistema. [EST][EST] incluye `Finalizado → [*]` y `Finalizado → Inactivo` sin eventos ni condiciones. [ARR][ARR] describe el arranque, pero no hay un caso de cierre.

**Propuesta.** Identificar el límite Sistema Tetris y distinguir fin de partida de fin de aplicación. Para este alcance, mantener `FINALIZADO` como estado estable desde el que se solicita reinicio y quitar la salida terminal no explicada. Indicar que el cierre de la aplicación queda fuera del modelo.

**Argumento.** Un final de partida no debería confundirse con la desaparición del sistema. Las flechas sin condición admiten lecturas diferentes sobre qué sucede después de perder. No se exige inventar un caso de uso de salida; se exige declarar qué se está representando.

### D15 · Las salidas de los detalles dependen del color y no de condiciones explícitas

**Decisión y ubicación.** [DES][DES], [REI][REI], [MOV][MOV] y [ROT][ROT] llevan alternativas al mismo final interno y después muestran varias salidas del estado compuesto, diferenciadas por colores y notas. En REI, dos salidas de cancelación llevan a estados diferentes sin indicar el origen.

**Propuesta.** Etiquetar guardas de salida por resultado y, en el reinicio, por estado de origen; o separar gráficamente recorridos completos. Usar etiquetas autosuficientes, sin depender del color.

**Argumento.** El color facilita la lectura humana, pero no conecta formalmente el resultado interno con un destino único. Cancelar desde `FINALIZADO` debe volver a `FINALIZADO`, nunca reactivar una partida perdida. Es un defecto de precisión de la representación, no una prueba de que el sistema realmente permita esa reactivación.

### D16 · El contexto no distingue reiniciar de confirmar el reinicio

**Decisión y ubicación.** [JUG][JUG] rotula las transiciones hacia `Inactivo` solo con `reiniciarPartida()`. [REI][REI] desarrolla una confirmación y una cancelación.

**Propuesta.** Declarar que el contexto resume casos de uso completados y añadir `[confirmado]` al retorno a `INACTIVO`. Representar la cancelación como conservación del estado de origen, mediante nota o bucles etiquetados.

**Argumento.** El contexto podría estar mostrando únicamente el resultado satisfactorio, por lo que no se registra una contradicción segura. Sin esa convención, se interpreta que basta solicitar el reinicio para abandonar la partida, antes de conocer la respuesta del jugador.

### D17 · El descenso presenta una nueva posición antes de comprobar si puede alcanzarla

**Decisión y ubicación.** En [DES, primera respuesta y primer punto de decisión][DES], el sistema presenta la pieza en una nueva posición y después distingue fondo o piezas fijadas frente a posición intermedia.

**Propuesta.** Comprobar primero si bajar una celda es válido. Si no lo es, fijar en la posición actual. Si lo es, presentar la nueva posición y precisar si se fija al tocar apoyo o en el siguiente evento; en la propuesta se conserva la fijación inmediata al quedar apoyada.

**Argumento.** Una pieza puede aparecer ya apoyada cuando la zona superior está ocupada y aun así caber. También puede desplazarse lateralmente hasta un apoyo. El detalle debe cubrir ese estado sin presentar una posición superpuesta o exterior al tablero. No se presupone que la implementación cometa esa colisión; el orden documental no la descarta.

### D18 · Eliminar líneas no especifica simultaneidad ni recolocación

**Decisión y ubicación.** [DES, respuesta tras fijar][DES] presenta el tablero sin líneas completas. [MD §1][MD] solo indica que desaparecen.

**Propuesta.** Determinar todas las filas completas tras la fijación, retirarlas conjuntamente, desplazar las ocupaciones superiores el número de filas eliminadas por debajo y dejar vacías las posiciones superiores. No contar la pieza activa como ocupación fijada antes de su fijación.

**Argumento.** Borrar ocupaciones sin compactar o eliminar solo una fila también satisface una lectura superficial de desaparecer. El caso de dos o más filas completas debe producir un tablero inequívoco y una única base para puntuar.

### D19 · La puntuación se actualiza sin una regla de cálculo

**Decisión y ubicación.** [INI][INI] fija la puntuación inicial a cero; [DES][DES] promete una puntuación actualizada después de eliminar líneas, sin definir cuánto aumenta ni por qué acciones.

**Propuesta.** Añadir una tabla o fórmula de puntuación para cero, una, dos, tres y cuatro filas, y aclarar si mover, girar o acelerar da puntos. Para la alternativa didáctica se propone `100 × filas eliminadas`, sin bonificaciones.

**Argumento.** Dos sistemas podrían presentar valores diferentes y ambos cumplir el texto. La fórmula propuesta no se atribuye al Tetris oficial: permite convertir la respuesta en un criterio de aceptación que el responsable del ejercicio puede aceptar o sustituir.

### D20 · El desplazamiento horizontal carece de magnitud

**Decisión y ubicación.** [MOV][MOV] solicita izquierda o derecha y distingue posición válida e inválida, sin definir la distancia de una solicitud.

**Propuesta.** Cada solicitud desplaza una celda en el sentido indicado. Si cualquier bloque quedaría fuera del tablero o sobre ocupación fijada, conservar la posición completa.

**Argumento.** Desplazar una casilla, continuar hasta soltar una tecla o saltar al borde son comportamientos diferentes. La unidad del movimiento pertenece al requisito; el mecanismo de repetición del teclado puede decidirse en el prototipo.

### D21 · La rotación no recoge el sentido que sí ofrece el prototipo

**Decisión y ubicación.** [ROT][ROT] solo solicita rotar. La [leyenda de IMG][IMG] ofrece dos comandos de giro, uno para cada sentido.

**Propuesta.** Incluir el sentido horario o antihorario como dato de la solicitud, un giro de 90 grados y una regla de pivote por tipo. Mantener el rechazo por colisión y no añadir desplazamientos correctores si quedan fuera de alcance.

**Argumento.** No hace falta crear dos casos de uso, pero el contrato debe distinguir las dos entradas que ya presenta la interfaz. Sin geometría y sentido no se puede decidir qué nueva ocupación debe verse tras la solicitud; D10 completa la regla geométrica necesaria.

### D22 · Los dos modos de caída no tienen un contrato temporal ni de persistencia

**Decisión y ubicación.** [VEL][VEL] alterna normal y acelerada; [REQ, actores][REQ] menciona un intervalo configurado. [INI][INI] y [REI][REI] no fijan el modo inicial ni lo que se restablece.

**Propuesta.** Definir los intervalos, el modo inicial, la persistencia al aparecer otra pieza y el efecto sobre el próximo descenso. Como alternativa: normal al empezar, modo conservado entre piezas y cambio de intervalo a partir de la solicitud, sin descenso adicional instantáneo.

**Argumento.** La aceleración puede durar una pieza, toda la partida o depender de mantener una tecla. El detalle adopta alternancia, pero no resuelve esos bordes. Además, la presentación de una pieza descendiendo no debe confundirse con ejecutar un descenso extra: esa responsabilidad ya está asignada a Tiempo.

### D23 · Reiniciar lleva a espera y su respuesta no describe lo que ve el jugador

**Decisión y ubicación.** [REI][REI] usa el nombre reiniciar, termina en `INACTIVO` y, al confirmar, dice que el sistema limpia el estado de partida.

**Propuesta.** Conservar el destino existente, pero definir reiniciar como descartar la partida y volver a la pantalla inicial, con tablero sin ocupaciones, puntuación cero, modo normal y opción de iniciar. Otra opción sería llamarlo volver al inicio, actualizando todos los artefactos.

**Argumento.** El nombre suele sugerir una nueva partida inmediata, mientras que aquí exige solicitar iniciar después. No es una contradicción entre diagramas: todos coinciden en el destino. Precisar la respuesta observable evita que limpiar estado sea una instrucción interna imprecisa y permite verificar la postcondición.

### D24 · El fin de partida muestra un mensaje, pero no ofrece el siguiente paso

**Decisión y ubicación.** La rama sin espacio de [DES][DES] muestra Game Over y termina en `FINALIZADO`. [JUG][JUG] permite reiniciar desde allí, pero DES no presenta esa posibilidad ni declara qué se conserva.

**Propuesta.** Mostrar resultado y tablero final, conservar la puntuación y permitir solicitar reinicio. Declarar que mover, girar y alternar ya no afectan a la partida finalizada.

**Argumento.** Una postcondición útil incluye tanto el estado alcanzado como la información y acciones disponibles. Se evita que el usuario quede ante un mensaje sin saber cómo continuar y que se interprete Game Over como cierre automático.

### D25 · No se decide qué pasa con el tiempo durante una confirmación

**Decisión y ubicación.** [REI][REI] espera una respuesta del jugador; [TMP][TMP] permite descensos durante `EnCurso`. No se precisa la interacción entre ambos mientras la confirmación está abierta.

**Propuesta.** Suspender descensos y manipulación durante la confirmación, sin acumular eventos pendientes. Cancelar conserva el estado de origen y reanuda un intervalo completo si procedía de `EN_CURSO`.

**Argumento.** Si se sigue jugando detrás de la confirmación, la partida podría terminar mientras el jugador responde y el retorno al origen quedaría mal definido. Elegir esta suspensión transitoria no obliga a añadir un caso de uso general de pausa. También debe resolverse el orden de solicitudes simultáneas mediante una regla observable.

## Prototipo y calidad del contrato

### D26 · Una captura estática se considera suficiente para todas las interacciones

**Decisión y ubicación.** [REQ, Prototipar casos de uso][REQ] afirma que más imágenes serían redundantes. [IMG][IMG] muestra una partida en curso; no muestra inicio, confirmación, cancelación ni Game Over. La explicación de alternar solo menciona caer más rápido.

**Propuesta.** Añadir bocetos de inicio, confirmación y final de partida, y un pequeño guion temporal para descenso y alternancia en ambos sentidos. Pueden reutilizar el mismo tablero y limitarse a los cambios relevantes.

**Argumento.** Una imagen sirve para discutir disposición y parte de los controles, pero no representa por sí sola una secuencia ni permite medir velocidad. No se exige una pantalla por cada caso de uso: se exige cubrir las interacciones distintas que el detalle ya promete.

### D27 · La leyenda de controles no explica cómo realizar varias solicitudes ni entradas no válidas

**Decisión y ubicación.** [IMG][IMG] enumera únicamente 4, 6, 7 y 9, con una línea de comando. [REQ][REQ] añade iniciar, reiniciar y alternar. No hay correspondencia documentada para ellas ni respuesta ante un comando desconocido.

**Propuesta.** Completar la correspondencia entre solicitudes y controles por estado, explicar si el comando se ejecuta al introducirlo o tras confirmarlo, y mostrar ayuda ante una entrada no admitida sin alterar la partida. Mantener estas teclas concretas en el prototipo, no dentro de los flujos abstractos.

**Argumento.** La ausencia de controles no prueba que el programa carezca de las operaciones, pero impide validar cómo se solicitan. Una interfaz mínima debe permitir descubrir sus acciones y corregir errores de entrada; no es necesario rediseñar la estética ni imponer una interfaz gráfica.

### D28 · No hay criterios medibles de respuesta para las interacciones

**Decisión y ubicación.** Los [siete detalles de REQ][REQ] describen resultados, pero no requisitos especiales medibles de respuesta. La única referencia temporal general es el intervalo del actor Tiempo, sin valores ni tolerancia.

**Propuesta.** Añadir un pequeño apartado suplementario con intervalos de caída y tolerancia, tiempo máximo de respuesta visible a solicitudes válidas y condiciones de medida. Presentar valores propuestos para acordar, no como observaciones de un sistema que no se ha ejecutado.

**Argumento.** Responder después de varios segundos podría satisfacer literalmente presenta la pieza, pero arruinaría la interacción. El material docente incluye requisitos no funcionales al detallar casos de uso. No se añaden exigencias de seguridad o disponibilidad ajenas al juego: se concretan las necesarias para validar su comportamiento temporal.

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
