# Farmear Aura: Iteración 4

Despues de las iteraciones anteriores, el modelo del dominio ha ido siendo refinado tanto en sus conceptos como en las relaciones entre ellos.

Para esta ultima iteración he decidido utilizar un **diagrama de colaboración**, con el objetivo de comprobar si los objetos del dominio están correctamente relacionados y pueden colaborar entre si para representar el proceso de farmear aura.

Me hago la siguiente pregunta:

**¿Que objetos necesitan colaborar entre sí para que una persona pueda farmear aura?**

### Conceptualmente

Partiendo de un caso concreto:

- `carlos : Persona`
- `irseSinMirar : Movimiento`
- `amigos : Publico`
- `auraDeCarlos : Aura`

las interacciones serían:

1. **Carlos se marca un movimiento.**
2. El **publico ve e interpreta el movimiento**.
3. Dependiendo de esa interpretación, el **publico determina el estado del aura**.

Además, `auraDeCarlos` está relacionada con `carlos`, ya que representa la percepción que el público tiene sobre él.

### Análisis

Al realizar el diagrama de colaboracion pude observar que las interacciones dinamicas necesarias se pueden realizar utilizando las relaciones que ya existen en el modelo.

También se vuelve a observar la diferencia entre una **interacción** y una **relación estructural**.

Por ejemplo:

`Aura -> Persona : tiene`

no es una accion que ocurra en un momento determinado, sino una relacion estructural entre ambos conceptos. 

### Resultado de la iteración

Esta iteracion no requiere añadir nuevas clases al modelo. El diagrama de colaboración permite comprobar que las relaciones obtenidas después de las iteraciones anteriores son suficientes para representar el proceso.

Por tanto, esta ultima iteracion sirve principalmente para **validar y simplificar el modelo**, evitando añadir conceptos o relaciones que no sean necesarios (sobreingenieria).