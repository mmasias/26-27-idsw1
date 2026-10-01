## Escenario 1: Farmear Aura

[Diagrama de Farmeo Aura](/entregas/zhenChao/src/reto-001/images/DOM_AURA.png)

### Entidades
* **Persona:** Entidad única unificada que puede asumir el rol de protagonista (quien actúa) o de testigo (quien observa y juzga).
* **Aura:** Acción específica ejecutada por la persona.
* **Evento Social:** Contexto o situación donde es ejecutada la acción.
* **Impacto:** El "golpe" social directo derivado de la acción.
* **Lugar:** Espacio físico o digital que enmarca el evento social.

### Consideraciones
* El aura no es autogestionable; carece de valor si no hay una audiencia.
* Tanto la acción realizada como el impacto que produce suceden de forma simultánea durante la interacción

### Justificaciones de Modelado
* Normalmente una entidad debería gestionar sus propios datos. Por lo tanto, es discutible permitir que una entidad externa (rol de testigo) determine directamente el Aura de otra (rol de protagonista). 

---

## Escenario 2: El Concepto de Simpatía

[Diagrama de Simpatía](entregas/zhenChao/src/reto-001/images/DOM_SIMPATIA.png)

### Entidades
* **Persona:** Entidad única unificada que actúa tanto como emisor y como juez.
* **Comportamiento:** La acción mostrada.
* **Evaluación Social:** El proceso cognitivo mediante el cual una persona juzga un comportamiento.
* **Rasgo Social:** La etiqueta final adjudicada.

### Consideraciones
* La simpatía es un fenómeno estrictamente relacional y perceptivo, no una variable genética o intrínseca de una persona.
* No hay transitividad directa; para saber quién es simpático, es obligatorio rastrear el comportamiento evaluado.

### Justificaciones de Modelado
* Se decidió modelarlo así porque en la vida real nadie es 'simpático' o 'antipático' de forma absoluta. Es una cuestión de perspectiva. Por eso el modelo refleja que este rasgo no es una característica que la persona simplemente 'tiene', sino que es una etiqueta que nace únicamente después de que alguien más evalúa su comportamiento.

---

## Escenario 3: Una Sombra

[ Diagrama de la Sombra](entregas/zhenChao/src/reto-001/images/DOM_SOMBRA.png)

### Entidades
* **Fuente de Luz:** Emisor de luz.
* **Cuerpo:** Obstáculo físico que interrumpe la luz.
* **Sombra:** Área de luz resultante de la interrupción con el cuerpo.
* **Superficie:** Topología donde la sombra se materializa visualmente.

### Consideraciones
* El modelo ignora factores ambientales como difracción o medios de dispersión.


### Justificaciones de Modelado
* El modelado de la luz iluminando el cuerpo, el cuerpo generando la sombra y la sombra proyectándose en la superficie es una simplificación del fenómeno físico real.
