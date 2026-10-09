# Flujo del Sistema de Aura

## 1. Ciclo de Vida de la Acción
* **Creada:** Se inicia la acción dentro de un contexto.
* **Ejecutada:** El actor realiza la acción.
* **EnValidacion:** La acción queda visible para que los espectadores la evalúen.

## 2. Resultados de la Validación
* **AuraIncrementada:** Ocurre tras un voto positivo normal. Otorga los puntos de aura base (`sumarAura`).
* **EnRacha:** Ocurre tras un voto positivo de alto impacto (`[Impacto]`). Otorga un multiplicador de aura y activa el estado especial.
* **AuraReducida:** Ocurre tras un voto negativo. Penaliza al usuario restándole aura (`restarAura`).

## 3. Ruptura de Racha
* **EnRacha → AuraReducida:** Si un usuario que se encuentra en el estado **EnRacha** recibe un voto negativo, pierde su estado de gracia y su racha se destruye inmediatamente (`romperRacha`).
