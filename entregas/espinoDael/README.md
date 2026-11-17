# Reto 002 - Auditoría del Modelo del Dominio y Requisitos (pyTetris)

Análisis crítico de los artefactos de ingeniería de requisitos y modelado del dominio del proyecto [pyTetris](https://github.com/puntoReflex/pyTetris), clasificando los hallazgos en **incoherencias entre artefactos** y **decisiones de diseño discutibles**.

---

## 1. Incoherencias entre artefactos

### 1.1. Discrepancia en la nomenclatura del caso de uso de velocidad de caída
* **Dónde se encuentra:** En la [especificación de casos de uso](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#casos-de-uso) frente al diagrama de especificación de [iniciarPartida()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/iniciarPartida.puml).
* **Qué dice cada uno:** El catálogo de requisitos nombra la acción como `alternarVelocidadCaida()` (comportamiento tipo *toggle* entre dos estados), mientras que el flujo de `iniciarPartida()` hace referencia a `acelerarCaida()` (acción unidireccional de incremento).
* **Corrección propuesta y justificación:** Estandarizar el identificador como `alternarVelocidadCaida()` en todos los diagramas de detalle. Semánticamente no representan lo mismo: alternar implica poder regresar a la velocidad base, mientras que acelerar solo contempla aumentarla. Mantener dos nombres rompe la trazabilidad entre el catálogo de casos de uso y sus diagramas de actividad/estado.

### 1.2. Contradicción sobre la existencia de niveles de dificultad
* **Dónde se encuentra:** En las secciones de [identificación de conceptos, decisiones de modelado y simplificaciones](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md) dentro del documento del Modelo del Dominio.
* **Qué dice cada uno:** En la fase inicial se identifica el concepto `Nivel` como el grado de dificultad actual y posteriormente se modela como un atributo dentro de la entidad `Juego`. No obstante, en el apartado de simplificaciones del mismo documento se afirma explícitamente que el sistema prescinde de niveles de dificultad.
* **Corrección propuesta y justificación:** Eliminar el atributo `nivel` de la clase `Juego` si realmente forma parte de las exclusiones de alcance de esta iteración, o bien retirar la frase de la sección de simplificaciones y definir cómo escala la dificultad. Un mismo documento arquitectónico no puede declarar un concepto como modelado y descartado simultáneamente.

### 1.3. Doble composición excluyente sobre la entidad `Celda`
* **Dónde se encuentra:** En la [tabla de relaciones del dominio](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md) y en el [diagrama estructural del Modelo del Dominio](https://github.com/puntoReflex/pyTetris/blob/main/images/modelosUML/ModeloDominio.svg).
* **Qué dice cada uno:** El modelo establece una relación de composición fuerte (rombo negro) desde `Tablero` hacia `Celda`, y simultáneamente otra composición fuerte desde `Línea` hacia `Celda`.
* **Corrección propuesta y justificación:** Reestructurar la jerarquía en cascada (`Tablero` compone a `Línea`, y `Línea` compone a `Celda`) o cambiar la relación de `Línea` con `Celda` a una agregación/asociación derivada. En UML, la composición estricta implica pertenencia exclusiva del ciclo de vida: una instancia de `Celda` no puede tener dos contenedores propietarios distintos al mismo tiempo.

### 1.4. Condición de transición omitida al reiniciar partida
* **Dónde se encuentra:** En el [diagrama de contexto del Actor Jugador](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorJugador.puml) frente a la especificación detallada de [reiniciarPartida()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml).
* **Qué dice cada uno:** El diagrama de contexto representa `reiniciarPartida()` como una transición directa e incondicional hacia el estado `Inactivo`. Sin embargo, el detalle interno del caso de uso introduce un paso de validación donde el jugador puede cancelar la operación, permaneciendo en el estado actual.
* **Corrección propuesta y justificación:** Añadir en el diagrama de contexto una transición reflexiva (o condición de guarda `[cancelado]` / `[confirmado]`) que refleje que el estado `Inactivo` solo se alcanza si el usuario confirma la acción. El diagrama de alto nivel no debe contradecir las postcondiciones posibles definidas en el nivel detallado.

### 1.5. Puntos de salida ambiguos en los diagramas de detalle de casos de uso
* **Dónde se encuentra:** En los diagramas de actividad de [moverPieza()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/moverPieza.puml), [rotarPieza()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/rotarPieza.puml), [descenderPieza()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/descenderPieza.puml) y [reiniciarPartida()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml).
* **Qué dice cada uno:** Durante el flujo interno se bifurcan los caminos según reglas de negocio (colisión vs. movimiento libre, confirmación vs. cancelación), pero al final todos los flujos convergen en un único nodo de unión antes de volver a dividirse hacia múltiples estados de salida sin etiquetas de guarda.
* **Corrección propuesta y justificación:** Etiquetar explícitamente las transiciones finales con sus guardas correspondientes (ej. `[movimiento válido]`, `[colisión detectada]`, `[reinicio confirmado]`, `[reinicio cancelado]`) o mantener los flujos separados hasta su estado final respectivo. Fusionar las ramas y volver a abrirlas sin condición impide saber qué camino interno produce cada postcondición.

### 1.6. Orden de validación invertido en el descenso de piezas
* **Dónde se encuentra:** En el detalle de [descenderPieza()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/descenderPieza.puml) en contraste con el detalle de [moverPieza()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/moverPieza.puml).
* **Qué dice cada uno:** En `moverPieza()` primero se verifica si la celda destino está libre antes de renderizar la pieza en la nueva posición. Por el contrario, en `descenderPieza()` el diagrama indica que primero se presenta la pieza en la posición inferior y posteriormente se evalúa si ha colisionado o tocado fondo.
* **Corrección propuesta y justificación:** Unificar la lógica en `descenderPieza()` para que evalúe la viabilidad del movimiento **antes** de actualizar la vista de la posición. Renderizar primero y validar después implica dibujar momentáneamente una pieza superpuesta con el fondo o con bloques ya fijados, lo cual contradice la lógica de colisiones aplicada en el desplazamiento lateral.

### 1.7. Omisión del control de velocidad en el prototipo de interfaz
* **Dónde se encuentra:** En el [catálogo de casos de uso](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md), en el detalle de [alternarVelocidadCaida()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/alternarVelocidadCaida.puml) y en la captura del [prototipo](https://github.com/puntoReflex/pyTetris/blob/main/images/tetris.png).
* **Qué dice cada uno:** Los artefactos de requisitos especifican que el jugador dispone de una acción para alternar la velocidad de caída, y la documentación afirma que la imagen del prototipo cubre la totalidad de las interacciones. Sin embargo, la leyenda de controles de la imagen (`4`, `6`, `7`, `9`) solo mapea traslación y rotación.
* **Corrección propuesta y justificación:** Incorporar en la leyenda visual del prototipo la tecla asignada a `alternarVelocidadCaida()` (y a `reiniciarPartida()`). Si el documento declara que el prototipo valida la interfaz de todos los casos de uso del jugador, todos los disparadores de entrada deben estar representados en él.

### 1.8. Ausencia del diálogo de confirmación de reinicio en el prototipo
* **Dónde se encuentra:** En el flujo detallado de [reiniciarPartida()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml) frente a la sección de [prototipado de casos de uso](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#prototipar-casos-de-uso).
* **Qué dice cada uno:** El diagrama de detalle exige que el sistema solicite confirmación al usuario antes de abortar la partida actual, ofreciendo las opciones de confirmar o cancelar. El prototipo presentado consta de una única pantalla de juego activo sin ningún *wireframe* o estado visual para dicho mensaje de confirmación.
* **Corrección propuesta y justificación:** Añadir un segundo boceto al prototipo que muestre el prompt o modal de confirmación de reinicio y las teclas para aceptar o cancelar. Sin esto, el prototipo está incompleto respecto a las interacciones de sistema definidas en la especificación detallada.

### 1.9. Asimetría en la transición desde el estado `Finalizado` hacia `Inactivo`
* **Dónde se encuentra:** En el [diagrama de contexto del Actor Jugador](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorJugador.puml) y en el [diagrama de contexto del Actor Tiempo](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto001-ActorTiempo.puml).
* **Qué dice cada uno:** En la vista del Jugador, el paso de `Finalizado` a `Inactivo` está etiquetado con el caso de uso `reiniciarPartida()`. En la vista del actor Tiempo, aparece exactamente la misma flecha de transición entre ambos estados pero huérfana de etiqueta o evento disparador.
* **Corrección propuesta y justificación:** Eliminar esa transición del diagrama del actor Tiempo (ya que el tiempo no reinicia la partida) o etiquetarla claramente indicando qué actor la dispara. Dejar una transición sin nombre en un diagrama de estados induce a pensar que el paso de `Finalizado` a `Inactivo` ocurre de forma automática o inmediata sin intervención.

---

## 2. Decisiones discutibles

### 2.1. Multiplicidad de piezas activas en el tablero
* **Qué se decidió:** En la [tabla de relaciones del modelo del dominio](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md) se modela que una instancia de `Juego` se asocia con múltiples piezas activas simultáneamente.
* **Propuesta y argumento:** Restringir la multiplicidad de la pieza en caída a `1` (o `0..1` dependiendo del estado) y diferenciarla conceptualmente de los bloques ya consolidados en el tablero. En las reglas estándar de Tetris clásico, el jugador solo controla una pieza activa por turno de caída, y una vez fijada deja de comportarse como pieza independiente para pasar a ser celdas ocupadas de las líneas.

### 2.2. Ausencia de multiplicidades en el diagrama visual del dominio
* **Qué se decidió:** Documentar las cardinalidades únicamente en una tabla de texto en lugar de reflejarlas sobre las líneas de asociación del [diagrama UML del dominio](https://github.com/puntoReflex/pyTetris/blob/main/images/modelosUML/ModeloDominio.svg).
* **Propuesta y argumento:** Incluir las multiplicidades (`1`, `1..*`, `4`, `10x20`, etc.) directamente sobre los extremos de las relaciones en el archivo `.svg`/`.puml`. Un diagrama de clases del dominio debe ser autoexplicativo de un vistazo sin obligar al lector a cruzar referencias constantemente con una tabla externa para saber cuántos elementos participan en cada relación.

### 2.3. Omisión de las restricciones dimensionales del tablero (10 × 20)
* **Qué se decidió:** Mencionar en el texto preliminar que el tablero mide 10 columnas por 20 filas, pero abstraer por completo ese dato en el modelo final del dominio.
* **Propuesta y argumento:** Reflejar la dimensión `20` líneas y `10` celdas por línea mediante multiplicidades o notas de restricción en el modelo del dominio (y la constante de `4` bloques por tetrominó). En el dominio específico de Tetris, la geometría 10×20 y los tetrominós de 4 celdas no son detalles técnicos de implementación, sino reglas estructurales del propio juego.

### 2.4. Inclusión del concepto `Turno` en un juego de tiempo continuo
* **Qué se decidió:** Modelar la clase `Turno` dentro del [diagrama del dominio](https://github.com/puntoReflex/pyTetris/blob/main/images/modelosUML/ModeloDominio.svg) vinculándola con las piezas y los movimientos.
* **Propuesta y argumento:** Eliminar la entidad `Turno` o renombrarla/definirla explícitamente como `CicloDeCaida` (el intervalo de vida desde que se genera una pieza hasta que se bloquea). El término «turno» pertenece al vocabulario de juegos secuenciales o por pasos (como el ajedrez); en un juego arcade en tiempo real gobernado por ticks de reloj, hablar de turnos resulta confuso si no se acota su significado.

### 2.5. Sobrediseño al modelar `TipoPieza` como clase independiente en lugar de enumerado
* **Qué se decidió:** Representar `TipoPieza` como una entidad separada asociada a `Pieza` en el [modelo del dominio](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md), sin atribuirle comportamiento ni propiedades diferenciadas.
* **Propuesta y argumento:** Modelar los 7 tetrominós estándar (`I`, `O`, `T`, `S`, `Z`, `J`, `L`) mediante un estereotipo `<<enumeration>>` o especificar qué atributos propios justifican la clase (por ejemplo, matriz de rotación inicial y color). Mantener una clase vacía sin atributos ni responsabilidades añade complejidad innecesaria al grafo del dominio cuando el conjunto de valores posibles es finito y cerrado.

### 2.6. Justificación débil para descartar la entidad `Jugador` del dominio
* **Qué se decidió:** Excluir el concepto `Jugador` del [modelo del dominio](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md) argumentando únicamente que el juego es para un solo usuario.
* **Propuesta y argumento:** Justificar la exclusión basándose en la **ausencia de persistencia de datos del usuario** (no se guardan perfiles, alias ni récords históricos), no en el número de jugadores. Un sistema monojugador sí necesita la entidad `Jugador` en el dominio si almacena estadísticas o perfiles; si no guarda nada de él, ese es el verdadero criterio arquitectónico para dejarlo solo como actor externo.

### 2.7. Modelado de `Tiempo` como actor primario en requisitos
* **Qué se decidió:** Declarar a `Tiempo` como un actor en la [disciplina de requisitos](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#actores) encargado de invocar el caso de uso `descenderPieza()`.
* **Propuesta y argumento:** Clarificar si `Tiempo` representa un temporizador externo del sistema operativo o tratar el descenso automático como un evento temporal interno dentro de la sesión de juego. Aunque en metodologías como RUP se admite el Tiempo como actor secundario/ficticio para disparar eventos periódicos, en un videojuego el *game loop* suele formar parte de la propia lógica interna del sistema mientras la partida está en estado `EnCurso`.

### 2.8. Nomenclatura de casos de uso con sintaxis de métodos de programación
* **Qué se decidió:** Nombrar todos los casos de uso utilizando *camelCase* y paréntesis vacíos (`iniciarPartida()`, `moverPieza()`, `rotarPieza()`) en el [diagrama de casos de uso](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaCasosUso.puml).
* **Propuesta y argumento:** Utilizar lenguaje natural en infinitivo con espacios («Iniciar partida», «Mover pieza», «Rotar pieza»). Los casos de uso pertenecen a la disciplina de requisitos y deben expresarse en el lenguaje del dominio y del usuario, no con notación de firmas de funciones propias de la fase de diseño o implementación.

### 2.9. Uso de diagramas de máquina de estados bajo el título de «Diagramas de contexto»
* **Qué se decidió:** Presentar en el apartado de [diagramas de contexto](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md#presentar-el-modelo-de-casos-de-uso-como-un-todo) diagramas cuyos nodos son estados de la partida (`Inactivo`, `EnCurso`, `Finalizado`) y cuyas aristas son los casos de uso.
* **Propuesta y argumento:** Llamarlos con precisión «Diagramas de navegación de estados de los casos de uso» (como recomienda la literatura cuando se ordenan las pre y postcondiciones de los casos de uso), o bien complementar la sección con un diagrama de contexto de sistema tradicional que delimite la frontera del software y los actores externos.

### 2.10. Semántica confusa de `reiniciarPartida()` al llevar al estado `Inactivo`
* **Qué se decidió:** Definir en [reiniciarPartida()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/reiniciarPartida.puml) que, tras confirmar el reinicio, el sistema transiciona al estado `INACTIVO` en lugar de comenzar una nueva partida en `EN_CURSO`.
* **Propuesta y argumento:** Si el destino es `INACTIVO` (pantalla de espera previa a jugar), renombrar el caso de uso a **«Abandonar partida»** o **«Salir al menú inicial»**; si se mantiene el nombre **«Reiniciar partida»**, hacer que la transición final limpie el tablero y deje al usuario directamente en `EN_CURSO` con una nueva pieza. Para cualquier jugador, «reiniciar» significa volver a empezar a jugar de inmediato, no quedarse en reposo esperando otro comando de inicio.

### 2.11. Falta de especificación del sentido de giro en la rotación
* **Qué se decidió:** Mostrar en el [prototipo](https://github.com/puntoReflex/pyTetris/blob/main/images/tetris.png) dos controles distintos (`7=rotarS` y `9=rotarU`), mientras que el [detalle de rotarPieza()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/rotarPieza.puml) describe una rotación genérica sin parametrizar la dirección.
* **Propuesta y argumento:** Incluir en la especificación de `rotarPieza()` que el actor indica el sentido de rotación (horario / antihorario) y aclarar en la leyenda del prototipo el significado de las siglas `S` y `U`. De lo contrario, el caso de uso no recoge toda la intención que el usuario expresa a través de la interfaz.

### 2.12. Regla de negocio incompleta para el cálculo de puntuación
* **Qué se decidió:** Indicar en [descenderPieza()](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/detalleCasosUso/descenderPieza.puml) la acción «Actualizar puntuación» al completar líneas, sin documentar la fórmula o tabla de puntos en ningún artefacto.
* **Propuesta y argumento:** Documentar como regla de negocio la relación entre líneas eliminadas simultáneamente (1, 2, 3 o 4 líneas / *Tetris*) y los puntos otorgados. En ingeniería de requisitos, dejar el cálculo de puntuación abierto impide verificar en las pruebas de aceptación si el sistema suma los puntos correctamente.

### 2.13. Acoplamiento entre el ciclo de vida de la aplicación y el de la partida
* **Qué se decidió:** Mezclar `arrancarSistema()` e `iniciarPartida()` en el mismo flujo de estados ([DiagramaDeContexto000.puml](https://github.com/puntoReflex/pyTetris/blob/main/modelosUML/DiagramaDeContexto000.puml)) y dibujar una transición hacia el nodo de fin `[*]` desde `Finalizado` sin especificar qué caso de uso cierra el sistema.
* **Propuesta y argumento:** Separar el ciclo de ejecución del programa (arrancar y cerrar aplicación) del ciclo de una partida, o añadir explícitamente el caso de uso `cerrarSistema()` / `salirJuego()`. Tal como está dibujado, parece que al terminar una partida el programa puede cerrarse solo sin que ningún actor lo solicite.

### 2.14. Responsabilidad de «pausar» huérfana en los requisitos
* **Qué se decidió:** Asignar a la clase `Juego` la responsabilidad de controlar el estado de pausa en la [tabla del modelo del dominio](https://github.com/puntoReflex/pyTetris/blob/main/docs/modeloDelDominio.md), pero omitir por completo el estado `Pausado` y el caso de uso `pausarPartida()` en el documento de requisitos.
* **Propuesta y argumento:** Alinear ambos documentos: o se elimina la mención a la pausa de la tabla del dominio por quedar fuera del alcance (MVP), o se incorpora el caso de uso y el estado correspondiente en los requisitos. Cada responsabilidad del dominio debe responder a una necesidad funcional trazable.

### 2.15. Indefinición del comportamiento de las velocidades de caída entre piezas
* **Qué se decidió:** Establecer en los [requisitos](https://github.com/puntoReflex/pyTetris/blob/main/docs/ProcesoRequisitos.md) que existen dos velocidades de caída alternables, sin especificar si la velocidad acelerada se mantiene al generarse la siguiente pieza o se restablece a la normal.
* **Propuesta y argumento:** Especificar en el detalle de `descenderPieza()` (cuando se fija una pieza y aparece la siguiente) y en `alternarVelocidadCaida()` si el modo rápido es global a la partida o temporal para la pieza activa. Esta distinción cambia por completo la jugabilidad y la validación funcional del requisito.

---

## 3. Conclusión y síntesis de la auditoría

Los defectos más críticos detectados se concentran en la **pérdida de trazabilidad entre vistas**:
1. El desajuste de nombres entre `alternarVelocidadCaida()` y `acelerarCaida()`.
2. La doble composición sobre `Celda` en el modelo del dominio, que vulnera las reglas formales de contenencia en UML.
3. Las discrepancias entre los diagramas de contexto y las especificaciones detalladas (como en `reiniciarPartida()`, donde la opción de cancelar cambia el flujo de estados pero no se refleja en el mapa general, ni tampoco en el prototipo visual).

Corregir estas incoherencias y explicitar las reglas de negocio ausentes (cálculo de puntos, sentidos de rotación y persistencia de la velocidad) permitiría pasar a la fase de diseño e implementación sin ambigüedades de interpretación.
