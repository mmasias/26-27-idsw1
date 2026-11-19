# INCOHERENCIAS

## 1. El nombre de la acción para acelerar la caída no es el mismo en todos los artefactos

Dónde aparece:

- docs/ProcesoRequisitos.md, apartado «Casos de Uso»: se identifica alternarVelocidadCaida().
- modelosUML/DiagramaCasosUso.puml: también aparece alternarVelocidadCaida().
- modelosUML/detalleCasosUso/iniciarPartida.puml: al terminar el inicio de la partida, el sistema permite solicitar acelerarCaida().

Qué dice cada artefacto:

La lista de requisitos y el diagrama general establecen que el caso de uso se llama alternarVelocidadCaida(). Sin embargo, el diagrama detallado de iniciarPartida() ofrece al jugador una operación llamada acelerarCaida().

No son nombres equivalentes exactamente: «acelerar» indica pasar a una velocidad mayor, mientras que «alternar» implica cambiar entre dos estados, por ejemplo, velocidad normal y velocidad acelerada.

Corrección propuesta:

Utilizar alternarVelocidadCaida() en todos los artefactos. En el detalle de iniciarPartida() habría que sustituir acelerarCaida() por ese nombre.

Por qué:

El caso de uso detallado debe utilizar los mismos nombres que el modelo general. Si se mantienen dos nombres, quien implemente el sistema puede pensar que son dos funcionalidades distintas o que una de ellas no está documentada.

## 2. Los estados del juego se escriben de distintas formas

Dónde aparece:

- modelosUML/DiagramaDeContexto000.puml: Inactivo, EnCurso y Finalizado.
- modelosUML/DiagramaDeContexto001-ActorJugador.puml: vuelve a utilizar Inactivo, EnCurso y Finalizado.
- modelosUML/DiagramaDeContexto001-ActorTiempo.puml: utiliza los mismos nombres con esa convención.
- Los diagramas detallados de casos de uso utilizan identificadores como EN_CURSO, INACTIVO y FINALIZADO.

Qué dice cada artefacto:

Los diagramas representan los mismos tres estados, pero unos utilizan mayúsculas y otros una combinación de mayúsculas y minúsculas.

Aunque se entiende que se refieren a los mismos estados, no existe una convención uniforme.

Corrección propuesta:

Elegir una única nomenclatura y mantenerla en todos los diagramas. Por ejemplo:
- INACTIVO
- EN_CURSO
- FINALIZADO
También se podrían utilizar Inactivo, EnCurso y Finalizado; lo importante es que la elección sea única.

Por qué:

La consistencia de nombres facilita la lectura y evita confusiones cuando se relacionan diagramas generales con diagramas detallados. Además, si los nombres se trasladan al código, una nomenclatura uniforme facilita su implementación.

## 3. La transición al estado final no está explicada de la misma manera en los diagramas de estados

Dónde aparece:
- modelosUML/DiagramaDeContexto000.puml: aparece la transición EnCurso --> Finalizado sin evento ni condición.
- modelosUML/DiagramaDeContexto001-ActorJugador.puml: también aparece EnCurso --> Finalizado sin explicar qué provoca el cambio.
- modelosUML/DiagramaDeContexto001-ActorTiempo.puml: especifica EnCurso --> Finalizado cuando se ejecuta descenderPieza y no hay espacio para una nueva pieza.

Qué dice cada artefacto:

El diagrama asociado al actor Tiempo explica que la partida termina cuando, después del descenso de una pieza, no hay espacio para generar otra. En cambio, los otros dos diagramas muestran el cambio a Finalizado sin indicar el motivo.

Corrección propuesta:

Añadir en los diagramas generales la condición que ya aparece en el diagrama del actor Tiempo. Por ejemplo:

EN_CURSO --> FINALIZADO : descenderPieza() 

Por qué:

El final de una partida es una regla importante del dominio. Debe quedar claro qué acontecimiento provoca ese cambio de estado. Si no se documenta, podrían implementarse condiciones distintas para terminar el juego.

## 4. La definición de «columna» en el modelo del dominio es incorrecta

Dónde aparece:
- docs/modeloDelDominio.md, apartado «Identificación inicial de conceptos».
Qué dice el artefacto:

La tabla define:
Columna: Fila vertical.

Una fila y una columna son conceptos diferentes. Una fila es horizontal y una columna es vertical.

Corrección propuesta:

Cambiar la definición por:
Columna: conjunto vertical de celdas del tablero.

Y mantener la definición de fila como:
Fila: conjunto horizontal de celdas del tablero.

Por qué:

El modelo del dominio debe utilizar correctamente el vocabulario del problema. Esta distinción es especialmente importante en Tetris, porque las líneas que se completan y eliminan son horizontales, no verticales.

## 5. Se afirma que el prototipo permite comprender todas las funcionalidades, pero no se muestran todas ellas

Dónde aparece:
- docs/ProcesoRequisitos.md, apartado «Prototipar casos de uso».
- Imagen del juego mostrada en el README.

Qué dice cada artefacto:

El documento afirma que una única imagen permite entender cómo se ve el juego, cómo interactúa el usuario y qué información presenta el sistema. También considera que añadir más prototipos sería redundante.

Sin embargo, el propio documento enumera varios casos de uso diferentes:
- iniciarPartida()
- descenderPieza()
- moverPieza()
- reiniciarPartida()
- rotarPieza()
- alternarVelocidadCaida()
- arrancarSistema()

Una captura de una partida en curso puede mostrar el tablero, la puntuación y una pieza activa, pero no permite observar por sí sola el inicio, el reinicio, la rotación, el cambio de velocidad ni el comportamiento cuando se termina la partida.

Corrección propuesta:

Mantener la imagen general, pero añadir prototipos o secuencias visuales breves para los comportamientos que no se pueden observar en ella. Como mínimo:
1. Pantalla inicial antes de comenzar.
2. Partida en curso.
3. Ejemplo de movimiento y rotación.
4. Cambio entre velocidad normal y acelerada.
5. Reinicio y finalización de la partida.

Por qué:

Un prototipo debe ayudar a comprender la interacción del usuario con el sistema. No es necesario crear una pantalla distinta para cada caso de uso, pero sí mostrar los estados y comportamientos que no quedan claros con la imagen existente.

# DECISIONES DISCUTIBLES

Estas decisiones no son necesariamente incoherentes con el resto del repositorio. Son decisiones que se pueden defender, pero para las que existe una alternativa que podría resultar más clara o adecuada.

## 1. Excluir «Jugador» del modelo del dominio

Dónde aparece:
- docs/modeloDelDominio.md, apartado «¿Incluir Jugador?».
- docs/ProcesoRequisitos.md, apartado «Actores».

Qué se decidió:

En el modelo del dominio se decide excluir Jugador, porque en un Tetris para un solo jugador se considera implícito. Sin embargo, en los requisitos sí aparece como actor que interactúa con el sistema.

Qué proponemos:

Mantener al jugador como actor externo en los requisitos, pero valorar su representación en el modelo conceptual si se quieren recoger explícitamente sus acciones o características.

Argumento:

La decisión original es defendible: un actor externo no tiene por qué ser una entidad interna del dominio. No obstante, incluirlo en el modelo conceptual puede ayudar a representar mejor quién controla las piezas y quién solicita las operaciones.

La clave es distinguir entre el jugador como persona que utiliza el sistema y el jugador como objeto que el software necesita almacenar o gestionar.

## 2. Considerar «Tiempo» un actor externo

Dónde aparece:
- docs/ProcesoRequisitos.md, apartado «Actores».
- modelosUML/DiagramaCasosUso.puml.
- modelosUML/DiagramaDeContexto001-ActorTiempo.puml.

Qué se decidió:

Se identifica Tiempo como actor porque provoca el descenso automático de las piezas según un intervalo configurado.

Qué proponemos:

Valorar si el tiempo debe aparecer como actor externo o si el descenso automático debería describirse como un comportamiento interno del sistema, activado por un temporizador.

Argumento:

Un actor suele representar algo externo que interactúa con el sistema. Si existe un reloj o servicio externo que envía eventos, Tiempo como actor puede tener sentido.

Sin embargo, si el propio programa controla el temporizador y decide cuándo bajar la pieza, el tiempo no es realmente un usuario externo. En ese caso, sería más claro documentar el descenso como un comportamiento automático del sistema.

La decisión depende de cómo se haya planteado la arquitectura.

## 3. Utilizar una única imagen como prototipo de todos los casos de uso

Dónde aparece:
- docs/ProcesoRequisitos.md, apartado «Prototipar casos de uso».

Qué se decidió:

Se considera que la imagen del README es suficiente para comprender el funcionamiento general y que crear más prototipos sería redundante.

Qué proponemos:

Crear prototipos adicionales para los cambios de estado y las interacciones que no se aprecian en la captura principal.

Argumento:

Una imagen general es útil para conocer el aspecto del juego, pero no permite comprobar cómo se comporta ante todas las acciones. Por ejemplo, no muestra qué sucede al reiniciar ni cómo se comunica al usuario que ha perdido.

Los prototipos adicionales ayudarían a descubrir problemas de usabilidad antes de implementar el sistema. No hace falta crear uno por cada operación si varias comparten la misma pantalla.

## 4. Modelar las piezas activas con multiplicidad 1 a muchos

Dónde aparece:
- docs/modeloDelDominio.md, apartado «Refinamiento de relaciones».

Qué se decidió:

Se define la relación Juego - Pieza (activas) como una agregación de tipo 1:*, porque el juego gestiona piezas que pueden existir conceptualmente sin él.

Qué proponemos:

Si la relación representa exclusivamente las piezas que están cayendo en ese momento, utilizar una multiplicidad 1:1: una partida tiene una única pieza activa.

Las piezas que ya han caído y se han fijado deberían representarse mediante el tablero, no como piezas activas.

Argumento:

En el Tetris clásico solo hay una pieza controlable en cada instante. Puede haber muchos bloques ocupados en el tablero, pero no muchas piezas activas simultáneamente.

La multiplicidad 1:* podría ser válida si se refiere a todas las piezas gestionadas durante la partida, incluidas las ya fijadas. El problema es que el documento habla específicamente de piezas «activas», por lo que convendría aclarar el significado de la relación.

## 5. Considerar «Turno» una entidad del dominio

Dónde aparece:
- docs/modeloDelDominio.md, apartados «Análisis de responsabilidades» y «Identificación de relaciones naturales».
Qué se decidió:

Se incluye Turno como concepto del dominio. Se indica que comienza, finaliza y que cada turno involucra una pieza.

Qué proponemos:

Valorar si realmente hace falta representar un turno como entidad o si sería suficiente con modelar el estado de la partida y la pieza activa.

Argumento:

En un Tetris tradicional de un solo jugador, el juego funciona de forma continua: la pieza baja automáticamente y el jugador puede moverla o rotarla mientras está activa. No existen turnos alternos como en un juego por turnos.

Por eso, incluir Turno puede añadir complejidad sin aportar demasiada información. Podría tener sentido si se define como el ciclo de vida de cada pieza, pero habría que explicarlo claramente para no confundirlo con un turno tradicional.

## 6. Modelar «Línea» como composición del tablero y de sus celdas
Dónde aparece:
- docs/modeloDelDominio.md, apartado «Refinamiento de relaciones».

Qué se decidió:

Se define Tablero - Línea como composición 1:* y Línea - Celda también como composición 1:*.

Qué proponemos:

Valorar si una línea debe ser una entidad independiente o si debería entenderse como una agrupación lógica de las celdas de una misma fila.

Argumento:

El tablero está formado por celdas que permanecen en posiciones concretas. Una línea completa se detecta cuando todas las celdas de una fila están ocupadas y, después, esa fila se elimina.

Por tanto, una línea no necesariamente tiene una existencia independiente: puede ser un concepto derivado de las posiciones del tablero.

Mantenerla como entidad puede facilitar la explicación de la mecánica, pero también puede introducir relaciones redundantes. La elección debería justificarse según cómo se vaya a implementar y utilizar el modelo.

## 7. Excluir niveles, estadísticas y funcionalidades avanzadas

Dónde aparece:
- docs/modeloDelDominio.md, apartados «Conceptos considerados y descartados» y «Simplificaciones conscientes».

Qué se decidió:

Se descartan o simplifican:
- Niveles de dificultad.
- Velocidad como entidad independiente.
- Estadísticas.
- Power-ups y piezas especiales.
- Multijugador.
- Funcionalidades avanzadas como hold piece, T-spins y wall kicks.
Qué proponemos:

Mantener el alcance sencillo para el ejemplo académico, pero dejar documentado qué funcionalidades quedan fuera y cómo se podrían incorporar en una futura ampliación.

Argumento:

Reducir el alcance permite centrarse en las mecánicas fundamentales y evita complicar innecesariamente el modelo. Sin embargo, algunas funcionalidades, como los niveles y las estadísticas, son habituales en Tetris y podrían enriquecer el sistema.

No es necesario implementarlas en este proyecto, pero sería conveniente distinguir entre las características descartadas por falta de alcance y las que se consideran innecesarias para el dominio.

## 8. No desarrollar todavía las disciplinas de análisis, decisiones tecnológicas y diseño

Dónde aparece:
- docs/ProcesoAnalisis.md.
- docs/DecisionesTecnologicas.md.
- docs/ProcesoDiseño.md.

Qué se decidió:

En la versión consultada del repositorio, estos documentos aparecen marcados como pendientes.

Qué proponemos:

Completar esas fases o indicar expresamente que están fuera del alcance de la entrega.

Argumento:

El repositorio presenta una secuencia de trabajo que va desde el modelo del dominio y los requisitos hasta el análisis, las decisiones tecnológicas y el diseño. Sin embargo, no se puede seguir toda esa trazabilidad si algunas fases todavía no están documentadas.

Esto no demuestra que las decisiones técnicas sean incorrectas, porque no están especificadas. El problema es que no se pueden evaluar ni comprobar sus relaciones con los requisitos.
