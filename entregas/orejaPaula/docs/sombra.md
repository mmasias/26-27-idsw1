
# Modelo del dominio: Una sombra

## Glosario

- **FuenteDeLuz:** elemento que emite luz.
- **CuerpoOpaco:** objeto que impide el paso de la luz.
- **PlanoProyeccion:** superficie donde aparece la sombra.
- **Sombra:** zona que queda sin iluminación directa por la presencia de un cuerpo opaco.

## Supuestos

- Para que aparezca una sombra es necesaria una fuente de luz, un cuerpo opaco y una superficie.
- La fuente de luz ilumina al cuerpo opaco, que impide que parte de la luz llegue al plano de proyección.
- Una fuente de luz puede intervenir en la formación de varias sombras.
- Un cuerpo opaco puede originar diferentes sombras.
- Un plano de proyección puede contener varias sombras.

## Decisiones de modelado

La **Sombra** se representa como un concepto que depende de la **FuenteDeLuz**, el **CuerpoOpaco** y el **PlanoProyeccion**, ya que necesita estos tres elementos para formarse.
