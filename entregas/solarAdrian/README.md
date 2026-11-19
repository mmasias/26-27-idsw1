# Reto 002 · Tetris

**[Tu nombre y apellidos]** · Ingeniería del Software I

Revisión crítica del [modelo del dominio](https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/docs/modeloDelDominio.md) y del [proceso de requisitos](https://github.com/puntoReflex/pyTetris/blob/92c950ad9746dc768be130f75c988c0f712d5509/docs/ProcesoRequisitos.md) del ejemplo [pyTetris](https://github.com/puntoReflex/pyTetris) visto en clase.

## Objetivo

Encontrar todos los problemas posibles en dos frentes:

| Frente | Qué es | Qué documento de cada hallazgo |
|-|-|-|
| **Incoherencias** | Artefactos que dicen cosas distintas sobre lo mismo | Dónde está cada uno, qué dice cada uno, cuál corrijo y por qué |
| **Decisiones discutibles** | Algo coherente con el resto, pero mal decidido | Qué se decidió, qué propongo y con qué argumento |

## Documentos

| Documento | Contenido |
|-|-|
| [Plan](docs/plan.md) | Alcance, artefactos revisados, criterios y método de búsqueda |
| [Hallazgos](docs/hallazgos.md) | Todas las incoherencias y decisiones discutibles, con su evidencia |
| [Correcciones](docs/correcciones.md) | Diagramas rehechos aplicando los hallazgos |

## Cómo leer los hallazgos

Cada hallazgo tiene un código según dónde se encuentra el problema:

| Código | Significado |
|-|-|
| **MD** | Incoherencia dentro del modelo del dominio |
| **MR** | Incoherencia entre el modelo del dominio y los requisitos |
| **RQ** | Incoherencia dentro de los requisitos o con el prototipo |
| **DD** | Decisión discutible |