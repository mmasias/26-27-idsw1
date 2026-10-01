## Descripción
El dominio representa la formación de una sombra cuando una fuente de luz ilumina una superficie y un objeto opaco bloquea parte de la luz.

El modelo se centra en los elementos físicos relevantes y en la relación entre ellos, sin intentar representar una simulación óptica completa.

## Glosario
Fuente de luz: elemento que emite luz.

Objeto: elemento que bloquea total o parcialmente la luz.

Superficie: elemento sobre el que puede proyectarse una sombra.

Sombra: región de una superficie que queda sin recibir directamente la luz debido a un objeto.


## Supuestos adoptados
Se considera una única situación de iluminación, aunque una fuente de luz puede participar en varias sombras.

El objeto se considera opaco para simplificar el modelo.

La sombra se representa como un fenómeno asociado a una superficie, no como un objeto físico independiente.

No se modelan propiedades ópticas avanzadas como reflexión, refracción, transparencia, penumbra o múltiples longitudes de onda.

La posición y la orientación se consideran conceptos físicos necesarios, pero no se detalla su estructura matemática.

Una sombra puede depender de una fuente de luz, un objeto y una superficie.


## Justificación de las decisiones de modelado
La sombra como clase

Se ha modelado Sombra como una clase porque, aunque sea un fenómeno físico y no un objeto material, posee información propia que interesa representar en el dominio: su forma, posición y tamaño. Además, permite expresar explícitamente qué fuente de luz, objeto y superficie intervienen en su existencia.

El objeto como elemento que produce la sombra

La relación entre Objeto y Sombra indica que el objeto es el elemento que bloquea la luz. No se ha creado una clase específica para el concepto de “bloqueo”, porque en este modelo simplificado no aporta información independiente.

La superficie como receptora

La sombra se relaciona con una Superficie porque no se considera una entidad independiente del lugar donde se observa. La misma configuración de luz y objeto puede producir sombras diferentes sobre superficies distintas.

Fuente de luz y objeto separados

Se mantienen como conceptos independientes porque tienen responsabilidades físicas distintas: la fuente emite luz y el objeto la bloquea. Esto permite ampliar posteriormente el modelo con diferentes tipos de fuentes u objetos sin modificar el concepto de sombra.


## Alcance
El modelo pretende ser una propuesta de modelo de dominio, no un modelo físico ni una simulación. Su objetivo es identificar los conceptos principales del escenario y las relaciones relevantes manteniendo el nivel de abstracción adecuado para un diagrama de dominio.
