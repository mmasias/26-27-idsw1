# Reto 001 — Farmear aura

## Glosario

- **Persona**: alguien que realiza u observa acciones.
- **Acción**: comportamiento realizado por una persona.
- **Situación**: circunstancias en las que ocurre una acción.
- **Percepción de aura**: impresión que una persona tiene del aura de otra.
- **Farmeo de aura**: acciones que mejoran el aura percibida de una persona.

## Supuestos

- El aura es una percepción social, no una propiedad objetiva.
- La misma acción puede generar percepciones distintas.
- La situación influye en cómo se interpreta una acción.
- Farmear aura puede ser intencionado o no.

## Decisiones de modelado

- El aura no se modela como un número dentro de `Persona`.
- `PercepcionDeAura` relaciona observador, persona valorada, acción y situación.
- `FarmeoDeAura` agrupa una o varias acciones.
- Observador y persona valorada son roles de `Persona`.
