# Corrección: `ModeloDominio`

## 1. Clasificación del hallazgo

- **Flanco:** Incoherencia entre artefactos (y decisión discutible en la depuración de sinónimos).

## 2. Descripción de la incoherencia

En el documento `modeloDelDominio.md` se decide descartar el concepto **Partida** considerándolo sinónimo de **Juego**, bajo el argumento de que *"Partida"* es propio de juegos multijugador.

Sin embargo, en toda la fase de requisitos (`ProcesoRequisitos.md`, `DiagramaCasosUso`, diagramas de contexto y detalles de casos de uso) el término **Partida** reaparece constantemente (`iniciarPartida()`, `reiniciarPartida()`, *"gestión de partida"*, *"partida iniciada"*), rompiendo el contrato conceptual que el propio modelo del dominio establece.

## 3. Justificación

Descartar *"Partida"* por tratarse de un juego de un solo jugador es una justificación floja: en español es completamente natural hablar de jugar o reiniciar una partida en solitario.

Además, **Juego** y **Partida** no son sinónimos en este dominio, sino dos conceptos diferenciados que encajan directamente con el ciclo de vida del sistema:

- **Juego:** Es el sistema o aplicación en sí (lo que se arranca con `arrancarSistema()`).
- **Partida:** Es cada sesión concreta que se inicia, se juega en un tablero y se reinicia (`iniciarPartida()`, `reiniciarPartida()`).

Incorporar `Partida` en el modelo del dominio resuelve la incoherencia de raíz y alinea el vocabulario con toda la fase de requisitos sin necesidad de renombrar los casos de uso.

## 4. Corrección aplicada

Se retira el descarte de `Partida` y se incorpora al diagrama del modelo del dominio como la entidad gestionada por `Juego` que contiene el `Tablero`, las `Pieza` activas y los `Turno`.

### Comparativa visual

**Diagrama original (repositorio)**

![Original ModeloDominio](https://github.com/puntoReflex/pyTetris/blob/main/images/modelosUML/ModeloDominio.svg)

**Diagrama corregido**

![Corregido ModeloDominio](../../../images/ModeloDominio.svg)