
# Revisión crítica del proyecto pyTetris



# 1. Incoherencias

## I-01. Diferencia en el nombre del caso de uso para acelerar la caída

**Dónde se encuentra:**
- `docs/ProcesoRequisitos.md`, apartado «Casos de Uso».
- `modelosUML/DiagramaCasosUso.puml`.
- `modelosUML/detalleCasosUso/iniciarPartida.puml`.

**Qué ocurre:**

En el documento de requisitos y en el diagrama general se utiliza el nombre `alternarVelocidadCaida()` para representar la acción de cambiar la velocidad de caída de las piezas.

Sin embargo, en el diagrama detallado de `iniciarPartida.puml` aparece `acelerarCaida()`.

Aunque ambas expresiones pueden referirse a una funcionalidad parecida, no tienen exactamente el mismo significado.

**Propuesta de corrección:**

Utilizar un único nombre en todos los documentos. Si la funcionalidad consiste en alternar entre velocidad normal y acelerada, mantener `alternarVelocidadCaida()`.

**Justificación:**

La consistencia en los nombres facilita la trazabilidad entre requisitos y casos de uso, evitando dudas sobre si se trata de una misma funcionalidad o de dos diferentes.

---

## I-02. Referencia a un prototipo que no aparece en el README

**Dónde se encuentra:**
- `docs/ProcesoRequisitos.md`, apartado «Prototipar casos de uso».
- `README.md`.

**Qué ocurre:**

En el documento de requisitos se hace referencia a una imagen del README que permite observar el aspecto del juego y su interacción mediante las teclas 4, 6, 7 y 9.

Sin embargo, el README no contiene la imagen del prototipo mencionada.

**Propuesta de corrección:**

Incluir la imagen del prototipo en el README o enlazarla correctamente desde el documento de requisitos.

**Justificación:**

Un prototipo debe poder consultarse para validar los requisitos. Si se menciona una imagen que no está disponible, resulta imposible comprobar las afirmaciones realizadas.

---

## I-03. Ausencia del modelo del dominio resultante

**Dónde se encuentra:**
- `docs/modeloDelDominio.md`, apartado 8: «Modelo del dominio resultante».

**Qué ocurre:**

El documento desarrolla el proceso de identificación de conceptos, las relaciones entre ellos y las decisiones de modelado.

Sin embargo, el apartado destinado a presentar el modelo del dominio resultante aparece vacío.

**Propuesta de corrección:**

Incluir un diagrama UML de clases del dominio que represente los conceptos principales, sus relaciones y multiplicidades.

**Justificación:**

El modelo del dominio es uno de los principales resultados de esta fase. No basta con explicar cómo se ha construido: es necesario presentar el resultado de forma visual y comprensible.

---

## I-04. Fases del desarrollo sin completar

**Dónde se encuentra:**
- `docs/ProcesoAnalisis.md`.
- `docs/DecisionesTecnologicas.md`.
- `docs/ProcesoDiseño.md`.

**Qué ocurre:**

Los documentos presentan contenido pendiente:

- El proceso de análisis contiene «PENDIENTE...».
- Las decisiones tecnológicas contienen «PENDIENTE».
- El proceso de diseño también contiene «PENDIENTE».

Sin embargo, estas fases aparecen como parte de la estructura documental del proyecto.

**Propuesta de corrección:**

Completar los documentos correspondientes o indicar claramente qué fases no forman parte del alcance del proyecto.

**Justificación:**

La documentación debe permitir seguir la evolución del proyecto desde los requisitos hasta el diseño. Las fases pendientes dificultan comprobar la continuidad entre ellas.

# 2. Decisiones discutibles

## D-01. Exclusión del jugador como concepto del dominio

**Dónde se encuentra:**
- `docs/modeloDelDominio.md`, apartado 6: «¿Incluir Jugador?».
- `docs/ProcesoRequisitos.md`, apartado «Actores».

**Qué se ha decidido:**

En el modelo del dominio se decide excluir el concepto Jugador porque se considera implícito en una versión de Tetris para un solo jugador.

Sin embargo, en el documento de requisitos el Jugador aparece como actor principal, encargado de iniciar partidas, mover piezas, rotarlas y reiniciar el juego.

**Por qué es discutible:**

La decisión no es necesariamente incorrecta, ya que un actor externo no tiene por qué convertirse en una entidad del dominio.

No obstante, la justificación de que no es necesario modelarlo porque el juego es individual resulta algo simplista.

**Propuesta de mejora:**

Mantener Jugador como actor externo en los casos de uso, pero explicar que su exclusión del modelo del dominio se debe a que no necesita tener identidad, atributos ni comportamiento propio dentro del modelo conceptual.

**Argumento:**

La separación entre actores externos y entidades del dominio es importante. La decisión puede mantenerse, pero debería justificarse desde esa distinción.

---

## D-02. Considerar el tiempo como actor del sistema

**Dónde se encuentra:**
- `docs/ProcesoRequisitos.md`, apartado «Actores».
- `modelosUML/DiagramaCasosUso.puml`.

**Qué se ha decidido:**

Se identifica a Tiempo como un actor del sistema, responsable de solicitar el descenso automático de las piezas.

**Por qué es discutible:**

El tiempo no es un usuario convencional ni un sistema externo con objetivos propios. Es un mecanismo temporal que provoca una acción del sistema.

**Propuesta de mejora:**

Valorar si resulta más adecuado representar el paso del tiempo como un evento temporal interno o como una interacción de un temporizador externo.

Si se mantiene como actor, debería explicarse claramente qué representa.

**Argumento:**

Es importante diferenciar entre los actores que interactúan con el sistema y los eventos que provocan comportamientos automáticos.

---

## D-03. Eliminación del concepto Partida

**Dónde se encuentra:**
- `docs/modeloDelDominio.md`, apartado 3: «Primera depuración: consolidación de sinónimos».

**Qué se ha decidido:**

Se decide unificar los conceptos Juego y Partida, conservando únicamente Juego.

La justificación es que Partida es un concepto más específico para juegos multijugador.

**Por qué es discutible:**

En un juego individual también existe el concepto de partida como una sesión concreta de juego.

Un jugador puede iniciar una partida, perderla y comenzar otra sin que cambie la aplicación.

**Propuesta de mejora:**

Valorar mantener ambos conceptos:

- Juego: representa el sistema o la actividad general.
- Partida: representa una ejecución concreta del juego, con su propio estado, puntuación y evolución.

Si se mantiene un único concepto, justificarlo por la simplicidad del alcance del proyecto.

**Argumento:**

Distinguir ambos conceptos puede mejorar la comprensión del ciclo de vida de una sesión de juego y facilitar futuras ampliaciones.

---

## D-04. Composición entre Tablero, Línea y Celda

**Dónde se encuentra:**
- `docs/modeloDelDominio.md`, apartado 7: «Refinamiento de relaciones».

**Qué se ha decidido:**

El documento establece relaciones de composición entre:

- Tablero y Línea.
- Línea y Celda.
- Tablero y Celda.

**Por qué es discutible:**

Esto implica que una misma celda forma parte del tablero y también de una línea concreta.

Sin embargo, una línea en Tetris puede entenderse como una agrupación lógica de celdas, no necesariamente como un objeto independiente que sea propietario de ellas.

**Propuesta de mejora:**

Mantener la composición entre Tablero y Celda, y representar Línea como una agrupación lógica o relación derivada de las celdas del tablero.

**Argumento:**

Las relaciones de composición deberían utilizarse cuando existe una relación de pertenencia fuerte y dependencia del ciclo de vida. En este caso, la relación entre línea y celda podría expresarse de una manera más sencilla.

---

## D-05. Utilizar una única imagen para representar todos los casos de uso

**Dónde se encuentra:**
- `docs/ProcesoRequisitos.md`, apartado «Prototipar casos de uso».

**Qué se ha decidido:**

El documento considera que una única imagen del juego es suficiente para representar todos los casos de uso.

**Por qué es discutible:**

Una imagen estática permite observar el aspecto visual del juego, pero no representa necesariamente todos sus comportamientos.

Por ejemplo, no permite comprobar por sí sola:

- El inicio de una partida.
- El reinicio después de perder.
- El descenso automático de las piezas.
- El cambio de velocidad.
- La aparición del mensaje de Game Over.

**Propuesta de mejora:**

Mantener una imagen general del juego, pero complementarla con capturas o secuencias que representen los estados más importantes.

**Argumento:**

El objetivo de un prototipo es facilitar la validación de los requisitos. Una representación demasiado limitada puede ocultar comportamientos importantes.

---

## D-06. Simplificación del diagrama de estados general

**Dónde se encuentra:**
- `modelosUML/DiagramaDeContexto000.puml`.

**Qué se ha decidido:**

El diagrama representa tres estados principales:

- Inactivo.
- EnCurso.
- Finalizado.

Sin embargo, no especifica detalladamente los eventos o condiciones que provocan cada cambio de estado.

**Por qué es discutible:**

La simplificación permite entender rápidamente el ciclo de vida general del juego, pero deja sin concretar aspectos importantes.

Por ejemplo, no se especifica qué condición provoca el paso de EnCurso a Finalizado, aunque en el caso de uso `descenderPieza()` aparece la situación en la que no hay espacio disponible y se produce Game Over.

**Propuesta de mejora:**

Añadir etiquetas a las transiciones que indiquen los eventos o condiciones correspondientes.

Por ejemplo:

- Inactivo → EnCurso: `iniciarPartida()`.
- EnCurso → Finalizado: `Game Over`.
- EnCurso → Inactivo: `reiniciarPartida()`.
- Finalizado → Inactivo: `reiniciarPartida()`.

También convendría revisar la transición EnCurso → EnCurso para especificar qué comportamiento representa.

**Argumento:**

Un diagrama de estados debe permitir comprender no solo qué estados existen, sino también qué condiciones provocan los cambios entre ellos.

