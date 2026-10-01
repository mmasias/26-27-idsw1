# Simpatía

## Glosario

* Persona: Alguien que cae bien o mal a otros y que también opina de los demás.
* Encuentro: Cada vez que dos o más personas coinciden o interactúan.
* Rasgo: Una cualidad que se nota al tratar con alguien.
* Simpatía: Lo bien que le cae una persona a otra, según los encuentros que han tenido y los rasgos que ha notado.

## Suposiciones

* La simpatía va de una persona hacia otra. No existe "simpático" a secas.
* Se necesita al menos un encuentro para que haya simpatía. Sin trato solo hay prejuicios, y eso se deja fuera.
* Alguien simpático es alguien a quien otra persona le tiene una simpatía alta. Dónde está ese límite lo decide cada persona.
* La simpatía puede cambiar con cada nuevo encuentro.
* La simpatía no tiene por qué ser mutua: A puede caerle bien a B y B no caerle bien a A.

## Decisiones de modelado

* La simpatía es una clase que une a dos personas, en lugar de una cualidad dentro de Persona, porque depende de quién mire.
* Los rasgos son una clase aparte y no una lista fija dentro de la persona, porque lo que le parece simpático a uno puede no importarle a otro.
* Los encuentros dan el origen de la simpatía: así se puede explicar por qué cambia con el tiempo.
* No distingo entre simpatía "buena" y "mala": una simpatía baja equivale a caer mal.