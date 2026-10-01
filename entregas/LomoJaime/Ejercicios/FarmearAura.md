# Farmear aura

## Glosario

* Persona: Alguien que protagoniza cosas y que también ve lo que hacen los demás.
* Suceso: Algo que pasa y protagoniza una persona.
* Reacción: Cómo responde alguien que lo ve.
* Aura: El "saldo" de respeto o presencia que una persona tiene ante los demás.
* Farmeo: Una racha de sucesos buscados para ganar aura.

## Suposiciones

* El aura funciona como un saldo: sube y baja según las reacciones.
* Sin alguien que lo vea no hay reacción, y sin reacción el aura no cambia.
* Lo que hace subir o bajar el aura depende de dónde y con quién pasa el suceso. Eso se guarda como dato del suceso.
* Para hablar de farmeo hace falta intención y repetición. Un solo suceso suelto no cuenta.

## Decisiones de modelado

* El aura la cambian las reacciones y no los sucesos directamente. Así se explica que un mismo suceso suba aura con unos y la baje con otros.
* El contexto no es una clase, sino un dato del suceso, porque por sí solo no hace nada.
* Farmeo sí es una clase, porque es lo que da nombre al tema: agrupa sucesos de una misma persona con un objetivo.
* El aura es una clase y no un número suelto porque es lo central del tema y va cambiando con el tiempo.