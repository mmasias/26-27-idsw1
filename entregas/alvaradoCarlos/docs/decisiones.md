# Decisiones de Modelado

### Primera Iteración

Decidí utilizar las entidades:

- FuenteDeLuz
- Cuerpo
- Superficie
- Posición
- Reflejo
- Perspectiva

Tomé en cuenta que siempre para generar una sombra, debe existir una fuente de iluminación y un cuerpo el cual está siendo iluminado.

Este cuerpo debe estar ubicado en una posición **(aúnque aún no sé si posición podría pasar a ser un atributo)**, y genera un **reflejo**.

Este reflejo es lo que escencialmente conocemos como sombra, y aunque la iluminación del cuerpo no rebota, considero que la silueta del mismo provoca el rebote de la umbra del cuerpo, más no de la luz, pero puede ser una forma equivocada de verlo, considerando que la umbra en realidad es simplemente el espacio del objeto al cual no le llego luz.

Este reflejo/sombra tiene una perspectiva, ya que si yo posiciono el objeto de una u otra forma, el reflejo adquiere una nueva perspectiva, pero diferente o no, sigue siendo una perspectiva concreta.

Finalmente, este reflejo se sitúa en una superficie la cual proyecta la umbra de la sombra o mejor dicho el espacio donde los fotones de luz no pudieron llegar.

### Segunda Iteración

- FuenteDeLuz
- Cuerpo
- Superficie
- Silueta

Decidí para esta iteración simplificar el modelo lo máximo posible. Re consideré la idea del reflejo y, aunque lo que tomé en cuenta al principio no estaba del todo incorrecto, metía complicaciones innecesarias en el modelo las cuales se pueden solucionar de otra forma. Como bien lo es la sustitución de la entidad reflejo por **silueta**.

La silueta representa, ya no el rebote de la luz en el cuerpo, si no la obstrucción de luz que ocurre gracias al cuerpo. Además, esta silueta se plasma en la superficie que teníamos ya modelada. Añadí también una conexión entre la fuente de luz con la silueta ya que, el cuerpo determina la forma de la silueta, pero la que proyecta esa silueta no es el cuerpo, es directamente el haz de luz que ilumina el cuerpo.

Además, convertí en atributos tanto la posición del cuerpo como la perspectiva de la sombra, ya que me di cuenta que no tenían comportamiento como tal en la vida de la sombra, únicamente influyen en ella a través de las entidades que ya tenemos listadas. Si cambia la posición de la luz, del cuerpo o de la superficie, cambiará la forma o representación de la sombra, pero más allá que de cálculos espaciales, la posición no aportaría nada al modelo por lo que puede quedarse como atributo de las 3.

Mientras que mi idea de perspectiva en el modelo anterior estaba equivocada, ya que aunque la perspectiva cambia si cambiamos la posición, en realidad la perspectiva forma parte no de la sombra, si no de un agente externo que puede verla. Por lo que si modelamos únicamente las partes escenciales de una sombra, todo aquel agente que no participe en la creación de ella no considero que deba ser introducido en el modelo.

### Tercera Iteración y Cuarta

Estuve analizando el modelo, y no llegué a tener ninguna otra mejora sobre el diagrama de clases, lo considero terminado (más probablemente no correcto). Pero he decidido separar los estados de las 3 entidades principales del sistema de la *sombra*. He separado el diagrama de estados en 3 diagramas, para poder ver de forma más explícita como cambian los 3 elementos fundamentales que compone la sombra. Aunque los 3 puedan parecerse mucho, los 3 se complementan porque conforman el proceso que puede ocurrir al momento de que una sombra existe, tanto si se mueve la luz, si se mueve el cuerpo que genera la silueta o si cambia la superficie donde se está mostrando.