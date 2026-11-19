# Reto 002 — Análisis de pyTetris

**Repositorio analizado:** [puntoReflex/pyTetris](https://github.com/puntoReflex/pyTetris)  
---

# 1. Incoherencias

## Nombre distinto para la acción de acelerar la caída

**Artefactos y referencias**

- [ProcesoRequisitos.md, líneas 45–53](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#L45-L53): el listado de casos de uso incluye `alternarVelocidadCaida()`.
- [DiagramaCasosUso.puml, líneas 8–14 y 22](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaCasosUso.puml#L8-L14): el diagrama también utiliza `alternarVelocidadCaida()`.
- [iniciarPartida.puml, líneas 26–34](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/iniciarPartida.puml#L26-L34): al presentar las acciones disponibles durante la partida aparece `acelerarCaida()`.

**Problema**

El listado general de requisitos y el diagrama de casos de uso identifican la acción como `alternarVelocidadCaida()`. Sin embargo, el detalle de `iniciarPartida()` ofrece al jugador la acción `acelerarCaida()`. Lo cual provoca que no quede claro si se trata de una única funcionalidad descrita con dos nombres o de dos operaciones distintas. La diferencia es relevante: «acelerar» sugiere pasar a una velocidad mayor, mientras que «alternar» implica cambiar entre dos estados de velocidad.

**Propuesta de corrección**

Unificar el nombre en todos los artefactos. Lo mejor seria utilizar `alternarVelocidadCaida()`, porque el detalle específico de ese caso de uso contempla tanto el paso de velocidad normal a acelerada como el regreso a velocidad normal.

---

## El prototipo asigna la misma descripción a dos teclas

**Artefactos y referencias**

- [ProcesoRequisitos.md, líneas 162–176](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#L162-L176): se presenta el prototipo como referencia de la interacción y se mencionan las teclas 4, 6, 7 y 9.
- [Imagen del prototipo — tetris.png](https://github.com/puntoReflex/pyTetris/blob/main/images/tetris.png): en la parte inferior aparece el texto `Comando (4=izq, 6=der, 7=rotaro, 9=rotaro):`.

**Problema**

La tecla 4 mueve a la izquierda y la tecla 6 a la derecha. Las teclas 7 y 9 aparecen descritas exactamente igual: `rotaro`, lo cual causa que el prototipo no permite saber qué diferencia existe entre las teclas 7 y 9. Podrían realizar la misma rotación o representar dos sentidos diferentes, pero la imagen no lo aclara.

**Propuesta de corrección**

Especificar la función de cada tecla en el prototipo y mantener esa misma descripción en la documentación. Si las dos teclas realizan rotaciones en sentidos opuestos, indicarlo expresamente; si hacen lo mismo, justificar por qué se ofrecen dos controles equivalentes. Los controles son parte de la interfaz con la que interactúa el jugador y si tienen una descripción ambigua impide conocer con precisión cómo utilizar el programa.

---

## La acción disponible tras iniciar la partida no coincide con el caso de uso general

**Artefactos y referencias**

- [ProcesoRequisitos.md, líneas 45–53](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#L45-L53): identifica `alternarVelocidadCaida()` como caso de uso solicitado por el Jugador.
- [iniciarPartida.puml, líneas 26–34](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/iniciarPartida.puml#L26-L34): enumera las acciones que el sistema permite solicitar una vez presentada la primera pieza y utiliza `acelerarCaida()`.

**Problema**
El modelo general reconoce `alternarVelocidadCaida()` como una de las acciones del jugador. El detalle del inicio de partida ofrece `acelerarCaida()` entre las acciones disponibles durante el estado `EN_CURSO`. Además de la diferencia de nombre descrita en la primera incoherencia mencionada, el listado de acciones disponibles en el flujo de inicio no permite establecer una correspondencia directa con el caso de uso general. Esto dificulta comprobar que todos los comportamientos están conectados de manera trazable.

**Propuesta de corrección**

En el detalle de `iniciarPartida()` utilizar exactamente los mismos nombres de los casos de uso definidos en el modelo general. En particular, sustituir `acelerarCaida()` por `alternarVelocidadCaida()`. El detalle de un caso de uso debe enlazar conceptualmente con los comportamientos identificados en el modelo general. La uniformidad facilita la revisión y evita duplicidades aparentes.

---

## El modelo de dominio asigna a Celda dos relaciones de composición

**Artefactos y referencias**

- [modeloDelDominio.md, líneas 148–153](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#L148-L153): declara composición entre `Tablero–Celda` y también entre `Línea–Celda`.
- [ModeloDominio.puml, líneas 7–8 y 16](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/ModeloDominio.puml#L7-L16): representa `Tablero *-- Celda` y `Linea *-- Celda`, ambas con composición.

**Problema**

La tabla de relaciones y el diagrama UML coinciden en que las celdas son partes compuestas del tablero y, a la vez, partes compuestas de una línea. No es una contradicción entre esos dos artefactos ambos expresan la misma decisión, pero el modelo resultante plantea un problema de propiedad: una misma celda del tablero puede pertenecer a una fila/línea, pero no debería tener dos propietarios compositivos independientes. En UML, la composición expresa una relación fuerte de propiedad y una parte no puede estar compuesta simultáneamente por dos objetos compuestos distintos.

**Propuesta de corrección**

Mantener la composición `Tablero–Celda`, porque las celdas forman la estructura del tablero. Para `Línea–Celda`, utilizar una asociación o una relación de agrupación que indique que una línea referencia/agrupa las celdas que la forman, sin ser propietaria de ellas. Una línea puede entenderse como una agrupación lógica de celdas del tablero. Su existencia depende de la organización del tablero, pero no necesita ser propietaria de las celdas. Esta representación evita duplicar la responsabilidad de propiedad.

---

# 2. Decisiones discutibles

## Representar una Línea como entidad compuesta por celdas

**Dónde aparece**

- [modeloDelDominio.md, líneas 48–49](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#L48-L49): define `Línea` como una fila horizontal completa.
- [modeloDelDominio.md, líneas 104–107](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#L104-L107): expresa que una línea contiene celdas.
- [modeloDelDominio.md, líneas 148–153](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#L148-L153): define composición entre `Tablero–Línea` y `Línea–Celda`.
- [ModeloDominio.puml, líneas 7–8 y 16](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/ModeloDominio.puml#L7-L16): refleja esas relaciones.

**Decisión tomada**

Se considera `Línea` una entidad del dominio y se representa como una parte compuesta del tablero que, a su vez, contiene celdas.

**Alternativa propuesta**

Estudiaría si `Línea` necesita ser una entidad con identidad y ciclo de vida propios o si basta con tratarla como una agrupación/propiedad derivada de las celdas del tablero. Si se mantiene como concepto, usaría una relación que no implique propiedad exclusiva de las celdas.

**Argumento**

El propio documento define una línea como una fila horizontal completa y también señala que las líneas se eliminan cuando se completan. En un tablero de Tetris, las filas pueden entenderse como posiciones o agrupaciones de celdas, y una línea completa como una condición que se calcula sobre ellas. Modelarla como entidad puede ser válido si tiene responsabilidades propias, por ejemplo, contar puntos o gestionar su eliminación—, pero la documentación debería explicar qué identidad y comportamiento justifican esa entidad. Sin esa explicación, el modelo puede introducir complejidad innecesaria y además genera el problema de doble composición.

---

## Exigir confirmación para cada reinicio

**Dónde aparece**

- [reiniciarPartida.puml, líneas 21–47](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml#L21-L47): después de solicitar el reinicio, el sistema presenta un mensaje de confirmación y espera que el jugador confirme o cancele.

**Decisión tomada**

El reinicio siempre pasa por una confirmación explícita del jugador, tanto si la partida está en curso como si ya ha finalizado.

**Alternativa propuesta**

Valorar si la confirmación debe mostrarse siempre o solo cuando exista progreso que pueda perderse. También podría ofrecerse una acción de reinicio directo con una interacción de confirmación menos intrusiva.

**Argumento**

La confirmación evita reinicios accidentales, lo cual es útil cuando se está jugando y se perdería el progreso. Sin embargo, cuando la partida ya está en estado `FINALIZADO`, el coste de confirmar puede aportar menos valor. La decisión debería tener en cuenta el contexto y el impacto de perder el estado actual, en vez de aplicar necesariamente el mismo paso en todas las situaciones. No propongo eliminar la confirmación sin más, si no que se deberia justificar cuándo es necesaria y cuándo puede simplificarse.

---

## Considerar suficiente una única imagen para prototipar varios casos de uso

**Dónde aparece**

- [ProcesoRequisitos.md, líneas 162–185](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#L162-L185): se presenta una única imagen y se afirma que los demás casos de uso quedan implícitos y claros, por lo que añadir más prototipos sería redundante.
- [Prototipo tetris.png](https://github.com/puntoReflex/pyTetris/blob/main/images/tetris.png): muestra un tablero, piezas, puntuación y los controles indicados.

**Decisión tomada**

Se considera que una única imagen del juego en estado `EN_CURSO` es suficiente para ilustrar también el inicio, el descenso automático, el cambio de velocidad y el reinicio.

**Alternativa propuesta**

Mantendría la imagen general como prototipo principal, pero añadiría pequeñas vistas o secuencias para los estados e interacciones que no se pueden deducir de ella: pantalla inicial, partida en curso, fin de partida y confirmación/cancelación del reinicio.

**Argumento**

La imagen muestra principalmente el tablero durante una partida y la leyenda de controles. No representa visualmente la pantalla inicial, el mensaje de `Game Over`, ni el diálogo de confirmación del reinicio. El texto describe esos comportamientos, pero no se pueden observar en el prototipo presentado. Por esto, más prototipos no serían necesariamente redundantes: podrían aclarar las transiciones y las respuestas de la interfaz. La cantidad de prototipos debe depender de si aportan información nueva, no de una regla fija de usar uno o varios.

---

## Excluir al Jugador del modelo del dominio, aunque sea actor de requisitos

**Dónde aparece**

- [modeloDelDominio.md, líneas 52–54](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#L52-L54): identifica inicialmente a `Jugador` como concepto.
- [modeloDelDominio.md, líneas 181–195](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md#L181-L195): lo descarta porque es implícito en la versión de un solo jugador.
- [ProcesoRequisitos.md, líneas 24–34](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#L24-L34): lo define como actor externo que controla el juego.

**Decisión tomada**

El jugador se excluye como entidad del modelo del dominio, pero se mantiene como actor externo en el modelo de requisitos.

**Alternativa propuesta**

Esta decisión puede ser correcta, ya que ,un actor de requisitos no tiene por qué convertirse automáticamente en una entidad del dominio. Pero convendría explicar expresamente esta diferencia en la documentación para evitar que parezca una contradicción. Si el juego necesitara guardar nombre, récords, preferencias o historial de jugadores, entonces sí tendría sentido reconsiderar su inclusión como entidad del dominio.

**Argumento**

El modelo de dominio representa conceptos y relaciones del ámbito del problema; el modelo de casos de uso representa roles externos que interactúan con el sistema. Son perspectivas distintas. Por eso, que `Jugador` aparezca como actor y no como entidad no constituye por sí solo una incoherencia. La decisión de excluirlo es razonable para un Tetris individual y sencillo, aunque su justificación podría explicitar mejor la diferencia entre ambos modelos.

---

# 3. Conclusiones

El análisis muestra que los principales puntos de mejora están relacionados con la consistencia de los nombres de los casos de uso, la precisión de los controles del prototipo y la claridad de algunas decisiones del modelo de dominio. En general, el repositorio ofrece una base comprensible para el proceso de modelado de un Tetris sencillo. Sin embargo, algunos detalles necesitan mayor precisión para que los artefactos se puedan seguir de forma consistente y para que las decisiones de modelado estén suficientemente justificadas.
