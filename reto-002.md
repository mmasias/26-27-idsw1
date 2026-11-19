# Reto 002 - Análisis crítico de pyTetris

## 1. Incoherencias

### 1.1. `Juego` frente a `Partida` entre el modelo del dominio y los requisitos
* **Dónde están:** `pyTetris/docs/modeloDelDominio.md` (secciones 3 *Consolidación de sinónimos*, 6 *Decisiones críticas de modelado* y 7 *Refinamiento de relaciones*, además de `ModeloDominio.svg`) frente a `pyTetris/docs/ProcesoRequisitos.md` (tabla de *Actores*, tabla de *Casos de Uso* y diagramas de detalle como `iniciarPartida.svg` y `reiniciarPartida.svg`).
* **Qué dice cada uno:** En `modeloDelDominio.md` se descarta *Partida* a favor de *Juego* argumentando que *"Partida"* es más específico para juegos multijugador, y se modela `Juego` como concepto principal. Sin embargo, en `ProcesoRequisitos.md` se utiliza sistemáticamente *"partida"* en los casos de uso (`iniciarPartida()`, `reiniciarPartida()`), en la justificación del actor *Jugador* (*"gestión de partida"*) y en las notas de detalle (*"partida iniciada"*, *"continuar partida"*).
* **Cuál corregirías:** Corregiría el modelo del dominio (y su vocabulario asociado) para adoptar *Partida* (o diferenciar claramente el juego/reglas del desarrollo de una *Partida* concreta que inicia, se desarrolla y finaliza), alineándolo con los requisitos.
* **Por qué:** Porque una partida en solitario también es una partida y el vocabulario del dominio debe coincidir con el lenguaje de los requisitos para mantener trazabilidad sin forzar argumentos de multijugador que no aplican al caso.

### 1.2. `acelerarCaida()` frente a `alternarVelocidadCaida()` y su comportamiento en requisitos y prototipo
* **Dónde están:** `acelerarCaida.svg`, `iniciarPartida.svg` y el texto de *Prototipar casos de uso* en `ProcesoRequisitos.md` frente a `alternarVelocidadCaida.svg`, `DiagramaCasosUso.svg` y `DiagramaDeContexto000.svg`.
* **Qué dice cada uno:** En `iniciarPartida.svg` se lista `acelerarCaida()` entre las acciones permitidas, existe un diagrama `acelerarCaida.svg` que modela el paso a mayor velocidad y el texto del prototipo dice que *"la pieza cae más rápido"*. Por otro lado, `DiagramaCasosUso.svg` y `DiagramaDeContexto000.svg` solo contemplan `alternarVelocidadCaida()`, y `alternarVelocidadCaida.svg` alterna entre velocidad normal y acelerada.
* **Cuál corregirías:** Corregiría `iniciarPartida.svg`, `acelerarCaida.svg` y el texto de *Prototipar casos de uso* para unificar todo bajo `alternarVelocidadCaida()` y describir con precisión que alterna entre velocidad normal y acelerada.
* **Por qué:** Porque mantener dos nombres distintos para el mismo comportamiento y describirlo de forma distinta entre prototipo y detalle crea ambigüedad sobre cuál es el requisito real del sistema.

### 1.3. `Puntuación` y `Velocidad` declaradas como atributos pero ausentes en el diagrama del dominio
* **Dónde están:** `modeloDelDominio.md` (sección 6 *Decisiones críticas de modelado* y *Conceptos considerados y descartados*) y `ModeloDominio.svg` frente a `ProcesoRequisitos.md` (`iniciarPartida.svg`, `descenderPieza.svg`, `alternarVelocidadCaida.svg` y el prototipo de consola).
* **Qué dice cada uno:** En `modeloDelDominio.md` se indica que *Puntuación* y *Velocidad* son propiedades/atributos del juego/sistema, pero en `ModeloDominio.svg` la clase `Juego` no muestra atributos. Mientras tanto, los requisitos y el prototipo sí las utilizan de forma activa (*"presenta puntuación inicial (0)"*, *"presenta puntuación actualizada"*, `Score: 0`, y alternancia entre velocidad normal y acelerada).
* **Cuál corregirías:** Corregiría `ModeloDominio.svg` y `modeloDelDominio.md` para reflejar explícitamente los atributos/conceptos correspondientes a la puntuación y a la velocidad de caída (sobre la entidad correspondiente, como *Partida*/*Juego*).
* **Por qué:** Porque si el modelo del dominio no representa los datos o atributos que los requisitos y el prototipo presentan y actualizan, se rompe la trazabilidad entre el dominio y los requisitos.

### 1.4. `Nivel` tratado simultáneamente como propiedad del juego y como simplificación excluida
* **Dónde están:** `modeloDelDominio.md` (sección 6 *Decisiones críticas de modelado* frente a la tabla *Simplificaciones conscientes* y el resto de `ProcesoRequisitos.md`).
* **Qué dice cada uno:** En la sección 6 de `modeloDelDominio.md` se agrupa *Nivel* con *Puntuación* concluyendo que son propiedades del juego, pero en *Simplificaciones conscientes* figura *"Sin niveles de dificultad"*, y en `ProcesoRequisitos.md` ningún caso de uso ni diagrama contempla cambios de nivel.
* **Cuál corregirías:** Corregiría la sección 6 de `modeloDelDominio.md` eliminando *Nivel* de la decisión de modelado como propiedad activa y manteniéndolo como simplificación consciente.
* **Por qué:** Porque un mismo concepto no puede figurar como parte activa del modelo en una sección y como simplificación descartada en otra sin generar contradicción.

### 1.5. `Turno` presente en el modelo del dominio pero ausente en los casos de uso y diagramas de contexto
* **Dónde están:** `modeloDelDominio.md` (secciones 5, 7, evolución de versiones y `ModeloDominio.svg`) frente a `ProcesoRequisitos.md` (tabla de *Casos de Uso*, `DiagramaCasosUso.svg`, diagramas de contexto y diagramas de detalle).
* **Qué dice cada uno:** `modeloDelDominio.md` y `ModeloDominio.svg` modelan `Turno` con relaciones con `Juego`, `Pieza` y `Movimiento` (*"Cada turno involucra una pieza"*, *"Un turno permite movimientos"*), y además mencionan *Turno* ya desde la versión 0 y luego dicen agregarlo en la versión 2. En `ProcesoRequisitos.md`, *Turno* no aparece en ningún caso de uso, actor ni transición de estado.
* **Cuál corregirías:** Corregiría `modeloDelDominio.md` y `ModeloDominio.svg` (eliminando o reubicando `Turno` si no es un concepto real de dominio/requisitos y corrigiendo el historial de versiones) o bien reflejaría su papel en los requisitos si realmente existe.
* **Por qué:** Porque un concepto modelado como central en el dominio que no tiene ningún reflejo en los requisitos ni en la dinámica de los casos de uso rompe la coherencia del proceso.

### 1.6. `Movimiento` unificado en el dominio frente a acciones separadas en requisitos
* **Dónde están:** `modeloDelDominio.md` (sección 3 *Consolidación de sinónimos*, sección 4 y `ModeloDominio.svg`) frente a `ProcesoRequisitos.md` (casos de uso `moverPieza()`, `rotarPieza()` y `descenderPieza()`, y sus diagramas de detalle).
* **Qué dice cada uno:** En `modeloDelDominio.md` se consolidan *Rotación*, *Caída* y *Desplazamiento* bajo el concepto *Movimiento*, y `ModeloDominio.svg` muestra una clase `Movimiento`. En `ProcesoRequisitos.md`, esas acciones se separan en distintos casos de uso (`moverPieza()` para izquierda/derecha, `rotarPieza()` para rotar y `descenderPieza()` disparado por *Tiempo*).
* **Cuál corregirías:** Revisaría y corregiría `modeloDelDominio.md` y `ModeloDominio.svg` para que la estructura del dominio no contradiga ni oscurezca la separación de comportamientos que exigen los casos de uso.
* **Por qué:** Porque si el dominio agrupa en un único concepto acciones que los requisitos tratan con reglas, actores y flujos diferenciados, se dificulta la trazabilidad hacia el análisis y el diseño.

### 1.7. Multiplicidades y relaciones no justificadas en `ModeloDominio.svg` y `modeloDelDominio.md`
* **Dónde están:** `ModeloDominio.svg` frente a `modeloDelDominio.md` (sección 7 *Refinamiento de relaciones* y evolución de versiones).
* **Qué dice cada uno:** La sección 7 anuncia el análisis de multiplicidades y tipos, y la versión 2 indica que se refinan multiplicidades, pero `ModeloDominio.svg` no muestra multiplicidades en las asociaciones; además, la tabla de la sección 7 justifica un subconjunto de relaciones mientras que el diagrama incluye relaciones adicionales (como `Juego`–`Turno`, `Turno`–`Pieza`, `Turno`–`Movimiento` y `Pieza`–`Celda`) sin el mismo tratamiento explícito.
* **Cuál corregirías:** Corregiría `ModeloDominio.svg` (añadiendo las multiplicidades en ambos extremos de cada relación) y completaría la tabla de la sección 7 de `modeloDelDominio.md` para cubrir todas las relaciones presentes en el diagrama.
* **Por qué:** Porque el diagrama UML y la tabla que lo justifica son dos partes del mismo artefacto y deben coincidir en relaciones y multiplicidades.

### 1.8. Piezas activas (`1:*` en el dominio) frente a pieza activa única en los casos de uso y prototipo
* **Dónde están:** `modeloDelDominio.md` (sección 7 y `ModeloDominio.svg`) frente a `ProcesoRequisitos.md` (`iniciarPartida.svg`, `descenderPieza.svg`, `moverPieza.svg`, `rotarPieza.svg`, `alternarVelocidadCaida.svg`) y el prototipo de consola.
* **Qué dice cada uno:** En `modeloDelDominio.md` y `ModeloDominio.svg`, la relación con piezas activas se plantea como múltiple (`1:*`, *"contiene piezas activas"*). Sin embargo, en todos los diagramas de detalle de `ProcesoRequisitos.md` se habla siempre de una única pieza activa (*"presenta primera pieza activa"*, *"presenta pieza activa en nueva posición"*, *"presenta nueva pieza activa"*) y en el prototipo solo hay una pieza (`I`) cayendo sobre piezas ya fijadas.
* **Cuál corregirías:** Corregiría la relación y multiplicidad en `modeloDelDominio.md` y `ModeloDominio.svg` (a `0..1` por partida, distinguiendo claramente las piezas ya fijadas en el tablero).
* **Por qué:** Porque la regla del juego y los casos de uso trabajan con una pieza activa a la vez; modelar una multiplicidad distinta sin aclararlo genera confusión sobre el estado del juego.

### 1.9. Nombres inconsistentes de estados y vocabulario entre documentos y prototipo
* **Dónde están:** `DiagramaDeContexto000.svg`, `DiagramaDeContexto001-ActorTiempo.svg`, los diagramas de detalle (`iniciarPartida.svg`, `descenderPieza.svg`, `reiniciarPartida.svg`, etc.), `modeloDelDominio.md` y la imagen del prototipo de consola.
* **Qué dice cada uno:** Los estados se escriben como `Inactivo`, `EnCurso` y `Finalizado` en los diagramas de contexto, pero como `INACTIVO`, `EN_CURSO` y `FINALIZADO` en los diagramas de detalle; además, en `descenderPieza.svg` aparece *"Game Over"*, en el prototipo aparece *"Score: 0"*, y en el dominio/requisitos se habla de *"puntuación"* y *"finalizado"*.
* **Cuál corregirías:** Unificaría los nombres de estados y términos clave en todos los diagramas, textos y prototipo (usando *Puntuación* en lugar de mezclar *Score*, y manteniendo un único nombre canónico para cada estado y evento).
* **Por qué:** Porque la consistencia terminológica es un requisito básico para que todos los artefactos comuniquen lo mismo sin obligar al lector a traducir términos sobre la marcha.

### 1.10. Transiciones sin caso de uso o sin etiqueta en los diagramas de contexto
* **Dónde están:** `DiagramaDeContexto000.svg` y `DiagramaDeContexto001-ActorTiempo.svg` frente a `DiagramaCasosUso.svg` y la tabla de *Casos de Uso* en `ProcesoRequisitos.md`.
* **Qué dice cada uno:** En `DiagramaDeContexto000.svg` aparece una transición desde `Finalizado` hacia el estado final `[*]` sin un caso de uso asociado de salida, y la transición `EnCurso --> Finalizado` no tiene etiqueta de evento/condición. En `DiagramaDeContexto001-ActorTiempo.svg` aparecen flechas (`[*] -> Inactivo`, `Inactivo -> EnCurso`, `Finalizado -> Inactivo`) sin etiqueta de evento y que no corresponden al actor *Tiempo*.
* **Cuál corregirías:** Corregiría `DiagramaDeContexto000.svg` y `DiagramaDeContexto001-ActorTiempo.svg` para que cada transición tenga su evento/condición correspondiente y quede claro qué corresponde a cada actor o al ciclo del sistema.
* **Por qué:** Porque un diagrama de contexto debe reflejar fielmente qué eventos o casos de uso provocan los cambios de estado sin dejar transiciones huérfanas o ambiguas.

### 1.11. Alcance del prototipo frente a lo que el texto afirma que cubre
* **Dónde están:** Sección *Prototipar casos de uso* y la imagen del prototipo de consola en `ProcesoRequisitos.md` frente a los casos de uso y diagramas de detalle (`arrancarSistema.svg`, `iniciarPartida.svg`, `reiniciarPartida.svg`, `alternarVelocidadCaida.svg`, `descenderPieza.svg`).
* **Qué dice cada uno:** El texto de *Prototipar casos de uso* afirma que esa única imagen deja claro cómo interactúa el usuario (teclas `4, 6, 7, 9`), qué información presenta el sistema y que todo lo demás (*iniciarPartida()*, *descenderPieza()*, *alternarVelocidadCaida()*, *reiniciarPartida()*) es implícito y claro. Sin embargo, las teclas `4, 6, 7, 9` solo cubren mover y rotar (no iniciar, reiniciar ni alternar velocidad), y la captura solo muestra `EN_CURSO`, sin vistas ni esquemas de la pantalla inicial, la confirmación de reinicio ni *Game Over*.
* **Cuál corregirías:** Corregiría el apartado *Prototipar casos de uso* de `ProcesoRequisitos.md` añadiendo representaciones/esquemas sencillos de los demás estados y acciones del jugador, y ajustando la afirmación de que una sola imagen lo cubre todo.
* **Por qué:** Porque afirmar que una captura de un único estado cubre todos los casos de uso cuando faltan estados clave (inicio, reinicio, fin) y controles deja sin prototipar flujos esenciales.

### 1.12. Sentido de rotación en el prototipo frente a `rotarPieza.svg`
* **Dónde están:** Imagen del prototipo de consola (`7=rotar↻, 9=rotar↺`) frente a `rotarPieza.svg` en `ProcesoRequisitos.md`.
* **Qué dice cada uno:** El prototipo muestra dos comandos de rotación en sentidos opuestos (`7=rotar↻` y `9=rotar↺`), mientras que `rotarPieza.svg` indica únicamente *"Jugador solicita rotar pieza"* sin especificar el sentido (a diferencia de `moverPieza.svg`, que sí menciona *"izquierda o derecha"*).
* **Cuál corregirías:** Corregiría `rotarPieza.svg` para indicar que el jugador solicita rotar la pieza (en sentido horario o antihorario), de forma análoga a como `moverPieza.svg` especifica izquierda o derecha.
* **Por qué:** Porque si el prototipo expone dos sentidos de rotación y el detalle del caso de uso no lo contempla, hay una discrepancia entre lo prototipado y lo especificado.

### 1.13. Validación y conclusión del dominio frente a omisiones y sobrantes detectados
* **Dónde están:** `modeloDelDominio.md` (secciones 9 *Validación con expertos del dominio*, *Conceptos considerados y descartados*, *Conclusión* y *¿Y ahora qué?*) frente a los demás apartados del mismo documento y a `ProcesoRequisitos.md`.
* **Qué dice cada uno:** En la sección 9 y en la *Conclusión* se afirma que no falta ningún concepto esencial, que no sobra ninguno y que el modelo es completo. Sin embargo, en los requisitos aparecen elementos como puntuación, velocidad y estados de partida que no están modelados como tales en el dominio, y en el dominio aparecen *Turno* y *Movimiento* que no se usan en requisitos.
* **Cuál corregirías:** Corregiría la sección 9 y la *Conclusión* de `modeloDelDominio.md` una vez ajustados los conceptos y relaciones del modelo.
* **Por qué:** Porque declarar completo y sin carencias un modelo que aún presenta huecos o conceptos desconectados de los requisitos resta rigor a la validación.

### 1.14. Verbos del dominio (`pausar`, `terminar`) y origen de los casos de uso
* **Dónde están:** `modeloDelDominio.md` (sección 4 *Análisis de responsabilidades*, verbos de *Juego*) frente a `ProcesoRequisitos.md` (sección *Casos de Uso*, *"Del modelo del dominio surge..."*).
* **Qué dice cada uno:** En la sección 4 de `modeloDelDominio.md` se asocian a *Juego* los verbos *"Iniciar, terminar, pausar, mantener puntuación"*. En `ProcesoRequisitos.md` se afirma que los casos de uso surgen del modelo del dominio, pero no existen casos de uso para pausar ni para terminar la partida, mientras que sí aparecen `arrancarSistema()` y `alternarVelocidadCaida()`.
* **Cuál corregirías:** Corregiría `modeloDelDominio.md` y `ProcesoRequisitos.md` para alinear los verbos/responsabilidades del dominio con los casos de uso que realmente se derivan de él.
* **Por qué:** Porque afirmar que los casos de uso surgen del modelo del dominio cuando hay verbos sin caso de uso y casos de uso sin verbo correspondiente debilita la trazabilidad declarada.

### 1.15. Ambigüedad de `Línea` (fila completa vs. fila del tablero vs. columna)
* **Dónde están:** `modeloDelDominio.md` (secciones 2, 5, 7 y *Conceptos considerados y descartados*, además de `ModeloDominio.svg`) y `descenderPieza.svg` (*"tablero sin líneas completas"*).
* **Qué dice cada uno:** En la identificación de conceptos se define *Línea* como *"fila horizontal completa"* y *Columna* como *"fila vertical"*, pero luego el diagrama y la sección 7 tratan *Línea* como parte/agrupación de celdas del tablero en general, y en `descenderPieza.svg` se habla de *"líneas completas"*.
* **Cuál corregirías:** Corregiría la terminología en `modeloDelDominio.md` y `ModeloDominio.svg` para distinguir claramente entre fila/celdas del tablero y la condición de línea completa que se elimina y puntúa.
* **Por qué:** Porque usar *"Línea"* unas veces como fila horizontal completa y otras como agrupación general del tablero produce ambigüedad en las reglas del juego.

---

## 2. Decisiones discutibles

### 2.1. Excluir `Jugador` del modelo del dominio por ser un juego de un solo jugador
* **Qué se decidió:** En `modeloDelDominio.md` se excluye *Jugador* como concepto del dominio argumentando que en *single-player* es implícito y que el sistema no necesita modelar múltiples jugadores.
* **Qué propones:** Incluir *Jugador* en el modelo del dominio relacionado con *Partida* (por ejemplo, mediante una asociación de participación/juego), manteniendo el foco en los conceptos naturales del dominio.
* **El argumento:** Que el juego sea para un solo jugador no elimina que exista un jugador que participa en la partida; incluirlo facilita la trazabilidad conceptual hacia el actor *Jugador* de la disciplina de requisitos.

### 2.2. Composición doble de `Celda` bajo `Tablero` y bajo `Linea`
* **Qué se decidió:** En `modeloDelDominio.md` y `ModeloDominio.svg`, *Celda* figura en composición tanto con *Tablero* como con *Linea*.
* **Qué propones:** Estructurar la composición jerárquicamente (por ejemplo, el tablero compuesto por filas/celdas o evitando que una misma celda tenga doble composición con dos contenedores distintos).
* **El argumento:** En UML, la composición implica propiedad exclusiva y ciclo de vida ligado a un todo; evitar la doble composición previene ambigüedades al eliminar filas o consultar celdas.

### 2.3. Modelar `Turno` y `Movimiento` como clases en el modelo del dominio
* **Qué se decidió:** En `ModeloDominio.svg` se incluyen `Turno` y `Movimiento` como clases conectadas al juego y a las piezas.
* **Qué propones:** Prescindir de `Turno` y `Movimiento` como clases de dominio (o reevaluar su necesidad real en fases posteriores) en favor de conceptos más fieles a la mecánica continua del Tetris y a los casos de uso.
* **El argumento:** En Tetris el descenso ocurre de forma continua por temporización y no por turnos alternados, y *Movimiento* representa acciones/casos de uso más que una entidad con estado propio en el dominio; simplificarlo evita clases innecesarias.

### 2.4. Descartar `Bloque` pese a que en el tablero quedan piezas fijadas/fragmentadas con su tipo
* **Qué se decidió:** En `modeloDelDominio.md` se descarta *Bloque* por considerarlo una abstracción innecesaria frente a *Pieza*.
* **Qué propones:** Evaluar la inclusión de *Bloque* (o precisar cómo se representan las celdas ocupadas tras fijar una pieza y eliminar filas) cuando una pieza deja de moverse o se limpia parcialmente.
* **El argumento:** Cuando una pieza se fija y se completan filas parciales, las celdas ocupadas conservan la marca/tipo de la pieza original aunque la pieza completa ya no exista como unidad móvil; modelarlo o aclararlo evita vacíos al representar el tablero.

### 2.5. Modelar `Juego`–`Pieza` como agregación (`1:*`)
* **Qué se decidió:** En `modeloDelDominio.md` (sección 7) y `ModeloDominio.svg` se representa la relación entre *Juego* y *Pieza* como agregación (`1:*`) indicando que las piezas pueden existir conceptualmente sin un juego.
* **Qué propones:** Modelar la relación entre la partida en curso y la pieza activa como una asociación simple con multiplicidad `0..1` (o `1` durante partida activa), y justificar con precisión el ciclo de vida de la pieza.
* **El argumento:** Una pieza en juego no tiene sentido independiente fuera de una partida y, según los casos de uso, solo hay una pieza activa en cada momento; una multiplicidad `0..1` refleja mejor la regla del juego.

### 2.6. Dejar `TipoPieza` sin los valores de los 7 tetrominós en el modelo del dominio
* **Qué se decidió:** Se incluye `TipoPieza` en el modelo del dominio por existir 7 tipos definidos, pero sin explicitar cuáles son (`I`, `O`, `T`, `S`, `Z`, `J`, `L`) ni cómo se representan.
* **Qué propones:** Especificar los 7 tipos de tetrominó (por ejemplo, como enumerado o lista explícita en el modelo) y aplicar el mismo criterio a conjuntos cerrados como estados de partida o modos de velocidad.
* **El argumento:** Hacer explícito el conjunto cerrado de 7 piezas aporta concreción al modelo del dominio sin introducir detalles de implementación ni tecnología.

### 2.7. Incluir flechas de navegabilidad/dirección en asociaciones del modelo del dominio
* **Qué se decidió:** En `ModeloDominio.svg` varias asociaciones tienen flechas dirigidas (`Pieza --> TipoPieza`, `Pieza --> Celda`, `Turno --> Pieza`).
* **Qué propones:** Utilizar asociaciones no dirigidas (o con dirección justificada únicamente cuando sea conceptualmente relevante) en el modelo del dominio, dejando las decisiones de navegabilidad para la fase de diseño.
* **El argumento:** Indicar qué clase conoce o referencia a cuál pertenece más al diseño orientado a objetos que al modelo conceptual del dominio, donde importa la relación y su multiplicidad.

### 2.8. Descartar `Preview/Siguiente` exclusivamente como detalle de interfaz
* **Qué se decidió:** En `modeloDelDominio.md` se descarta *Preview/Siguiente* únicamente como detalle de interfaz.
* **Qué propones:** Distinguir entre el concepto de *siguiente pieza* (regla/información del juego) y su representación visual en pantalla, moviéndolo a *Simplificaciones conscientes* si se omite por simplicidad del reto.
* **El argumento:** Separar concepto de dominio de representación visual evita descartar elementos del juego por motivos de interfaz cuando en realidad se trata de una simplificación deliberada del alcance.

### 2.9. Nombrar los casos de uso con sintaxis de función (`nombreCamelCase()`)
* **Qué se decidió:** En `ProcesoRequisitos.md` y en los diagramas de casos de uso se nombran todos los casos de uso con paréntesis (`iniciarPartida()`, `moverPieza()`, `rotarPieza()`, etc.).
* **Qué propones:** Redactar los nombres de los casos de uso en lenguaje natural orientado al objetivo del actor (por ejemplo, *Iniciar partida*, *Mover pieza*, *Rotar pieza*) y reservar la notación técnica para fases posteriores de análisis o diseño.
* **El argumento:** La sintaxis de función con paréntesis recuerda a métodos de programación y puede contaminar la fase de requisitos con una mentalidad de implementación prematura.

### 2.10. Usar expresiones de interfaz o implementación interna en los detalles de casos de uso
* **Qué se decidió:** En los diagramas de detalle de `ProcesoRequisitos.md` aparecen expresiones como *"presenta pantalla inicial"*, *"presenta mensaje de confirmación"* o *"Sistema limpia estado de partida"*.
* **Qué propones:** Reformular las acciones del sistema en términos de información presentada o acciones permitidas al actor (por ejemplo, *"permite confirmar o cancelar el reinicio"* o *"presenta la partida finalizada"*), evitando detalles internos como *"limpia estado"*.
* **El argumento:** Mantener los detalles de casos de uso centrados en lo observable por el actor y en la interacción con el sistema preserva el nivel de abstracción propio de la disciplina de requisitos.

### 2.11. Hacer que `reiniciarPartida()` devuelva al estado `INACTIVO` y pida confirmación incluso en `FINALIZADO`
* **Qué se decidió:** En `reiniciarPartida.svg` se solicita confirmación tanto desde `EN_CURSO` como desde `FINALIZADO`, y al aceptar se transita a `INACTIVO` (requiriendo luego `iniciarPartida()` para volver a jugar).
* **Qué propones:** Reiniciar directamente a una nueva partida en curso (o pedir confirmación solo cuando haya una partida activa `EN_CURSO`, sin exigir confirmación en `FINALIZADO` ni obligar a un paso intermedio si el objetivo es reiniciar).
* **El argumento:** Pedir confirmación cuando la partida ya terminó no protege ningún progreso activo y añade pasos innecesarios al flujo de reinicio.

### 2.12. Hablar de *"intervalo configurado"* para `Tiempo` sin requisito de configuración
* **Dónde/Qué se decidió:** En la tabla de actores de `ProcesoRequisitos.md` se indica que *Tiempo* solicita el descenso automático *"según intervalo configurado"*, sin que exista ningún requisito o caso de uso para configurar ese intervalo.
* **Qué propones:** Eliminar *"configurado"* y especificar los intervalos de descenso (normal y acelerado) como un requisito suplementario o regla del sistema.
* **El argumento:** Mencionar un intervalo *"configurado"* sugiere una funcionalidad de configuración inexistente; fijar los tiempos de caída en requisitos suplementarios elimina la ambigüedad.

---

## 3. Correcciones aplicadas en conjunto a los artefactos

* **En `ProcesoRequisitos.md` y sus diagramas (`DiagramaCasosUso.svg`, `DiagramaDeContexto000.svg`, `DiagramaDeContexto001-ActorTiempo.svg` y detalles):**
  * Unificar el nombre del caso de uso de velocidad (`alternarVelocidadCaida()`) en todos los diagramas y textos, eliminando o corrigiendo referencias contradictorias a `acelerarCaida()`.
  * Etiquetar la transición `EnCurso --> Finalizado` en `DiagramaDeContexto000.svg` con su evento/condición de fin de partida y depurar `DiagramaDeContexto001-ActorTiempo.svg` para que no muestre transiciones sin etiqueta o de otros actores como propias de *Tiempo*.
  * Alinear las transiciones y cancelaciones de `reiniciarPartida.svg` con `DiagramaDeContexto000.svg` y ajustar `arrancarSistema.svg` para que su transición hacia `INACTIVO` sea coherente.
  * Ampliar *Prototipar casos de uso* con vistas o esquemas sencillos de la pantalla inicial (`INACTIVO`), confirmación de reinicio y partida finalizada (`FINALIZADO`), e incluir el sentido horario/antihorario en `rotarPieza.svg`.
* **En `modeloDelDominio.md` y `ModeloDominio.svg`:**
  * Aclarar la diferencia entre entidad interna del dominio y actor externo (*Jugador*), revisar el uso de *Partida*/*Juego* y *Línea*/*Fila*, y eliminar *Nivel* de las propiedades activas si es una simplificación excluida.
  * Completar las multiplicidades en `ModeloDominio.svg`, alinear las relaciones del diagrama con la tabla de la sección 7 y corregir la inconsistencia sobre *Turno* y *Movimiento*.