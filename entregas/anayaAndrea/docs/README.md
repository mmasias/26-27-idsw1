# Propuesta de modelo del dominio

## 1. Una sombra

### Glosario

* **Persona:** objeto que proyecta una sombra.
* **Sombra:** proyección producida por una persona al bloquear la luz.
* **Fuente de luz:** elemento que produce la luz necesaria para generar la sombra.

### Supuestos

* Una persona puede proyectar una sombra.
* Una fuente de luz puede generar varias sombras.
* Se considera una relación simplificada entre persona, sombra y fuente de luz.

### Decisión de modelado

Se ha separado la fuente de luz de la sombra porque la sombra depende de la existencia de una fuente de luz.

---

## 2. Farmear aura

### Glosario

* **Persona:** individuo que obtiene aura.
* **Acción:** actividad realizada para conseguir aura.
* **Evento:** situación en la que se realiza una acción.

### Supuestos

* Una persona puede realizar varias acciones.
* Una acción ocurre en un evento.
* Una persona puede participar en varios eventos.

### Decisión de modelado

Se separan las acciones de los eventos porque una misma acción puede realizarse en diferentes eventos.

---

## 3. Simpatía

### Glosario

* **Persona:** individuo que puede mostrar simpatía.
* **Comportamiento:** forma de actuar de una persona.
* **Interacción:** relación entre personas.

### Supuestos

* Una persona puede mostrar diferentes comportamientos.
* Una persona puede participar en varias interacciones.
* Una interacción puede involucrar a varias personas.

### Decisión de modelado

Se utiliza el comportamiento como elemento relacionado con la simpatía, ya que esta se puede identificar a partir de la forma en que una persona interactúa con otras.
