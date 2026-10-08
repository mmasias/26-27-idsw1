# Farmear Aura: Iteración 3

Para analizar en que orden interactuan los diferentes conceptos del dominio cuando una persona intenta farmear aura, he decidido utilizar un diagrama de secuencias donde se apreciará mejor la ordenacion. 

Me he hecho la siguiente pregunta:

**¿En que orden interactúan Persona, Movimiento, Público y Aura cuando una persona intenta farmear aura?**

### Conceptualmente

La **persona** se marca un **movimiento**  
↓  \
El **público** ve el **movimiento**  
↓  \
El **público** interpreta el **movimiento**  
↓  \
Dependiendo de su interpretación, el movimiento puede producir una impresión positiva, neutra o negativa  
↓  \
El **público** modifica el estado del **aura**  
↓  \
El **aura** representa la percepción que el público tiene sobre la **persona**

### Primera versión del diagrama de secuencia

![Diagrama de secuencia 1](/entregas/valdesCarlos/images/aura/AuraIteracion3DiagramaSecuencia1.png)

En una primera versión traté de representar las relaciones que ya tenía en el diagrama de clases dentro del diagrama de secuencia.

Al analizar el resultado observé dos problemas:

- La relacion **Movimiento -> Publico: genera efecto en** resultaba ambigua. Realmente el movimiento no realiza una acción sobre el publico, sino que es el **publico quien ve e interpreta el movimiento**.
- La relacion **Aura -> Persona: representa la percepción sobre** tampoco representa una acción que ocurra en un momento        concreto. Es una relacion estructural entre los conceptos del dominio, por lo que tiene más sentido mantenerla en los diagramas de clases y objetos y no representarla como un mensaje en el diagrama de secuencia.

### Refinamiento

A partir de estos cambios he realizado una segunda versión del diagrama de secuencia.

La relación:

**Publico -> Movimiento: ve**

y:

**Movimiento -> Público: genera efecto en**

se sustituye por:

**Publico -> Movimiento: ve e interpreta**

De esta forma se representa mejor quién realiza realmente la acción de interpretar el movimiento.

Tambien, elimino del diagrama de secuencia la relación:

**Aura -> Persona: representa la percepción sobre**

ya que no representa una interacción temporal, aunque se mantiene como relación dentro del modelo de dominio.

Este diagrama de secuencia también permite representar que, dependiendo de la interpretación del público, el estado del aura puede mejorar, mantenerse o empeorar.

![Diagrama de secuencia 1](/entregas/valdesCarlos/images/aura/AuraIteracion3DiagramaSecuencia2.png)