# Plan de revisión

[README](../README.md) · [Hallazgos](hallazgos.md)

## 1. Versión revisada

Reviso pyTetris tal como estaba en el commit [`92c950a`](https://github.com/puntoReflex/pyTetris/tree/92c950ad9746dc768be130f75c988c0f712d5509). Todos los enlaces de la entrega apuntan a esa versión, para que un cambio posterior en el repositorio no deje los hallazgos sin evidencia.

## 2. Alcance

**Dentro:** todo lo que aparece en estas dos páginas, porque el enunciado cuenta como artefacto cualquier texto, tabla, diagrama o imagen.

| Página | Artefactos |
|-|-|
| [Modelo del dominio](https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/docs/modeloDelDominio.md) | Observaciones iniciales, tabla de sustantivos, tabla de sinónimos, tabla de verbos, relaciones naturales, decisiones críticas, tabla de multiplicidades, diagrama resultante, validación, descartes, simplificaciones, evolución del modelo, lecciones y conclusión |
| [Proceso de requisitos](https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/docs/ProcesoRequisitos.md) | Punto de partida, tabla de actores, tabla de casos de uso, diagrama de casos de uso, diagrama de estados del sistema, diagramas de contexto de Jugador y Tiempo, detalle de los 7 casos de uso, imagen del prototipo y su texto |

**Fuera:** análisis, diseño y código. No los uso para justificar ni para corregir nada: los requisitos tienen que entenderse por sí mismos.

## 3. Criterios

**Incoherencia.** Dos artefactos, o dos partes del mismo artefacto, afirman cosas incompatibles sobre lo mismo. Para cada una indico:

1. Dónde está cada artefacto.
2. Qué dice cada uno, citado literalmente.
3. Cuál corregiría.
4. Por qué ese y no el otro.

**Decisión discutible.** Algo coherente con el resto, pero que considero mal decidido. Para cada una indico:

1. Qué se decidió.
2. Qué propongo.
3. El argumento, apoyado en lo visto en la asignatura: definición del modelo del dominio, semántica UML, criterios de actores y casos de uso.

**Lo que no cuento como hallazgo:**

- Un "yo lo habría hecho distinto" sin argumento.
- Las simplificaciones declaradas por el propio ejemplo (sin multijugador, sin niveles, sin power-ups), salvo que otro artefacto las contradiga.

## 4. Método de búsqueda

| Paso | Comprobación | Busca |
|-|-|-|
| 1 | Texto del MdD frente a su diagrama | Decisiones del texto que el diagrama no refleja |
| 2 | Vocabulario del MdD frente al de los requisitos | Conceptos con nombres distintos, o que solo aparecen en un lado |
| 3 | Tabla de casos de uso, diagrama, contextos y detalles | Nombres, estados de origen y destino, lo que "permite solicitar" cada caso de uso |
| 4 | Diagrama de estados del sistema frente a los diagramas por actor | Transiciones que faltan o sobran |
| 5 | Detalles de casos de uso frente al prototipo | Casos de uso que no se pueden pedir y pantallas que no se muestran |
| 6 | Cada decisión frente a lo visto en clase | Decisiones mal tomadas aunque sean coherentes |

## 5. Plan de trabajo

| Commit | Contenido |
|-|-|
| 1 | README y este plan |
| 2 | Incoherencias dentro del modelo del dominio (MD) |
| 3 | Incoherencias entre el modelo del dominio y los requisitos (MR) |
| 4 | Incoherencias dentro de los requisitos y con el prototipo (RQ) |
| 5 | Decisiones discutibles del modelo del dominio (DD) |
| 6 | Decisiones discutibles de los requisitos (DD) |
| 7 | Correcciones: diagramas rehechos en PlantUML |
| 8 | Repaso final y enlace en `entregas/README.md` |