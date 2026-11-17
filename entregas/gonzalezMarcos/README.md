# Reto 002 · Revisión crítica de los requisitos de Tetris

**Autor:** Marcos González Hidalgo · **Asignatura:** Ingeniería del Software I

Revisión del modelo del dominio y de la disciplina de requisitos de `puntoReflex/pyTetris`: textos, tablas, diagramas de casos de uso y estados, siete detalles de casos de uso y prototipo.

## Organización del trabajo

1. Fijar la versión de las fuentes e inventariar los artefactos.
2. Contrastar lo que afirman y separar incoherencias de decisiones discutibles.
3. Proponer correcciones trazables, con especial atención al detalle de casos de uso.
4. Exportar los modelos PlantUML a SVG y comprobar la navegabilidad de la entrega.

El [método de revisión](docs/metodo.md) explica los criterios y el alcance. No se crea una carpeta `src`, porque este reto no incluye implementación.

## Resultados de la revisión

- [Fuentes e inventario](docs/fuentes.md), con versión fijada y copia del prototipo original.
- [Tres incoherencias](docs/incoherencias.md): nombre de la solicitud de velocidad, transición de reinicio y alcance del nivel.
- [28 decisiones discutibles](docs/decisiones-discutibles.md), agrupadas por dominio, contextos, detalles y prototipo.

Las lagunas y ambigüedades se incluyen como decisiones discutibles; no se presentan como contradicciones demostradas ni como fallos de una implementación que no se ha evaluado.
