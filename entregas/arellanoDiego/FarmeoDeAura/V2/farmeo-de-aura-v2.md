# Glosario

| Término | Definición Conceptual |
| :--- | :--- |
| **Individuo** | Persona que interactúa en su entorno buscando proyectar respeto, presencia o estatus. |
| **Aura** | Nivel de reputación, respeto y presencia que los demás perciben de un individuo. |
| **Acción** | Hecho observable que la persona hace frente a otros y queda expuesto al juicio social. |
| **Audiencia** | Personas (en vivo o en redes) que observan la acción y reaccionan a ella. |
| **Veredicto** | Calificación o veredicto decidido por la audiencia, el cual decide el impacto en el aura. |

# Supuestos Adoptados

1. **Intención de destacar:** La persona busca activamente el reconocimiento social y prestigio mediante lo que hace.
2. **Dependencia del público:** El aura no existe en el vacío; requiere necesariamente testigos que observen la acción.
3. **El veredicto manda:** Una acción es neutra hasta que la audiencia decide un veredicto, el cual determina si el aura sube o baja.
4. **El farmeo como conducta continua:** Farmear no es un objeto estático, sino la actividad continuada mediante la cual el individuo hace acciones para acumular aura.

# Decisiones de Modelado: Farmear aura 
* `Aura` se representa como una clase conceptual y no como un atributo numérico de `Individuo`, porque tiene reglas, umbrales y estados propios que cambian con el tiempo.
* `Audiencia` y `Veredicto` se modelan como entidades separadas de `Accion`, porque el cambio de aura no ocurre en soledad y depende enteramente del juicio del público.
* Se omiten deliberadamente los pensamientos y la autoestima del `Individuo`, porque los estados mentales ocultos no son observables ni atañen a la operación del sistema.
