## Descripcion
El dominio representa la mecánica de un juego en la que un jugador realiza determinadas actividades para obtener y acumular aura. El aura puede aumentar mediante recompensas y utilizarse para representar el progreso del jugador.

## Glosario
Jugador: persona o personaje que participa en el juego.

Aura: recurso o atributo que el jugador puede acumular.

Actividad: acción que puede realizar el jugador para obtener aura.

Recompensa: resultado obtenido al completar una actividad.

Sesión: realización concreta de una actividad por parte del jugador.


## Supuestos adoptados
“Farmear aura” significa realizar actividades repetidamente para aumentar la cantidad de aura del jugador.

Cada jugador posee un aura que representa su progreso acumulado.

Una actividad proporciona una recompensa que incluye una determinada cantidad de aura.

Una misma actividad puede realizarse varias veces.

Se distingue entre Actividad y Sesion: la primera define qué actividad existe y la segunda representa una ejecución concreta.

El modelo no especifica un juego concreto ni reglas particulares sobre cómo se calcula la recompensa.

El tipo de aura se mantiene como atributo para permitir diferentes clases de aura sin introducir más complejidad.


## Justificación de las decisiones de modelado
Aura como clase

Se ha modelado Aura como una clase porque representa un concepto central del dominio y tiene información propia: cantidad, nivel y tipo.

Actividad y sesión como conceptos diferentes

Se ha separado Actividad de Sesion. Una actividad representa una acción disponible en el juego, mientras que una sesión representa una ejecución concreta de esa acción por un jugador. Esto permite registrar que una misma actividad puede realizarse muchas veces.

Recompensa como clase

Se ha incluido Recompensa para representar el resultado de completar una actividad. Aunque podría ser un atributo de Actividad, como clase permite ampliarla posteriormente con otros recursos, objetos o efectos.

Jugador y aura

La relación Jugador -- Aura expresa que cada jugador tiene asociado su progreso de aura. Se ha evitado tratar el aura únicamente como un atributo de Jugador porque es un concepto relevante del dominio.


## Alcance
El modelo representa únicamente la mecánica básica de conseguir aura mediante actividades. No se incluyen inventario, comercio, enemigos, mapas, misiones o economía del juego.
