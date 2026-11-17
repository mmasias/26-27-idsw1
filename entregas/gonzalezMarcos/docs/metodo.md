# Método y alcance

[Volver a la presentación](../README.md)

## Plan de revisión

La revisión se realiza sobre el commit `92c950ad9746dc768be130f75c988c0f712d5509` de [puntoReflex/pyTetris](https://github.com/puntoReflex/pyTetris/tree/92c950ad9746dc768be130f75c988c0f712d5509), consultado el **17 de noviembre de 2026**. Los enlaces de evidencia se fijan a esa versión para que posteriores cambios no alteren el objeto de la entrega.

Se revisan las páginas del modelo del dominio y del proceso de requisitos, incluidos sus diagramas publicados, sus fuentes PlantUML y la imagen del prototipo. Las páginas de análisis, diseño y código quedan fuera del encargo: la implementación no se utiliza para completar silencios de los requisitos.

## Criterios

- **Incoherencia:** dos artefactos o fragmentos afirman cosas incompatibles sobre el mismo asunto. Se identifican ambos, se explica la diferencia y se elige qué corregir y por qué.
- **Decisión discutible:** una elección de representación, comportamiento o alcance necesita revisión aunque no contradiga otro artefacto. Se documenta la elección, una alternativa y un argumento comprobable. Las omisiones y ambigüedades se incluyen aquí cuando impiden precisar el comportamiento.
- Una simplificación no es un error por sí sola. No se exige multijugador, niveles, reserva de piezas, sombras ni reglas avanzadas.
- No se interpreta la ausencia de cardinalidades como una cardinalidad concreta. Tampoco se confunde la ausencia de una clase conceptual con la ausencia de un actor.
- Las versiones históricas pueden diferir legítimamente. Solo se señala desalineación cuando las decisiones vigentes no permiten saber qué versión rige.
- Las propuestas de esta entrega son alternativas razonadas, **no requisitos confirmados por el profesor ni reglas oficiales de Tetris**.

## Orden de contraste

1. Vocabulario, relaciones, cardinalidades y reglas del dominio.
2. Actores, catálogo de casos de uso y asociaciones.
3. Estados globales y transiciones por actor.
4. Para cada detalle: actor, disparador, precondición, entradas, respuesta observable, alternativas y postcondición.
5. Cobertura del prototipo respecto a los comportamientos prometidos.
6. Correspondencia entre fuentes PlantUML e imágenes publicadas.

## Criterio docente

Se sigue la [actividad de detallar casos de uso](https://github.com/mmasias/idsw1/blob/main/temario/contenidos/00008-Cdu.dCdU.md): flujos principales y alternativos, precondiciones, postcondiciones, requisitos especiales y lenguaje de interacción. Se mantienen los diagramas de estados utilizados en clase; emplearlos para detallar casos de uso no se considera un defecto.

La [identificación de actores](https://github.com/mmasias/idsw1/blob/main/temario/contenidos/00006-CdU.eAyCdU.md) admite expresamente al actor **Tiempo**. Por tanto, se conserva esa convención y se revisa la precisión de sus eventos, no su legitimidad como actor.

La [actividad de prototipado](https://github.com/mmasias/idsw1/blob/main/temario/contenidos/00009-CdU.ICdU.md) se usa para comprobar que las interacciones puedan validarse mediante bocetos. El [modelo del dominio de hdC](https://github.com/mmasias/hdC/blob/main/docs/UMLdocs/mdd.md) aporta un ejemplo de vistas complementarias de clases, objetos y estados; no se toma como una plantilla obligatoria para Tetris.
