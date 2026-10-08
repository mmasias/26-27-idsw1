# Farmear Aura: Iteración 2

Hasta el momento habia estado enfocando el **aura** de forma binaria, es decir, a una **persona** se le puede atribuir aura o no. Pero el de aura puede estar en diferentes estados. Por ejemplo, aura alta, aura normal o aura baja

Para poder mejorar este concepto de aura he decidido utilizar el diagrama de estados, y me he hecho esta pregunta: **¿Cómo cambia el aura de una persona a medida que realiza movimientos?**

### Conceptualmente
Aura baja\
   ↕\
Aura normal\
   ↕\
Aura alta

Los cambios entre estos estados vendrían provocados por cómo el **público** percibe los nuevos **movimientos** realizados por la **persona**:\
Si el movimiento impresiona -> Aumenta el aura\
Si el movimiento no causa ninguna impresión → el aura se mantiene.\
Si el movimiento no impresiona/ da cringe -> Disminuye el aura\


El diagrama de estados me ha permitido observar que el aura no debe modelarse únicamente como algo que una persona tiene o no tiene. El aura puede variar dependiendo de cómo el público perciba los distintos movimientos realizados por la persona. Por ello, voy a añadir el concepto de **estado** al aura. Este estado puede ser positivo, negativo o neutral.


