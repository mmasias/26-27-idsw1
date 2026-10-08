# Modelo de Dominio

## Glosario

* **Persona:** Individuo que puede producir o proyectar una sombra.
* **Luz:** Fuente de iluminación que incide sobre un objeto.
* **Objeto:** Elemento que bloquea parcial o totalmente la luz.
* **Sombra:** Zona donde la luz queda bloqueada por un objeto.

## Suposiciones

* Para que exista una sombra debe existir una fuente de luz.
* La sombra aparece cuando un objeto bloquea la luz.
* La forma de la sombra depende del objeto y de la posición de la luz.
* Se considera que una persona puede actuar como objeto que bloquea la luz.

## Decisiones de modelado

Se ha modelado la sombra como un concepto independiente porque no es una propiedad del objeto, sino el resultado de la interacción entre la luz y el objeto.

Se ha incluido Persona como una posible fuente del objeto que produce la sombra.