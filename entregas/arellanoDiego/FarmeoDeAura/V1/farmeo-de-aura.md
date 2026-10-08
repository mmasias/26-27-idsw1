# Glosario

| Término | Definición Conceptual |
| :--- | :--- |
| **Individuo** | Sujeto que interactúa con su entorno buscando proyectar estatus. |
| **Aura** | Nivel de reputación, respeto y presencia que los demás perciben de un individuo. |
| **Farmeo** | Intentos continuos y organizados de una persona por acumular aura. |
| **Acción** | Hecho observable realizado por el individuo y abierto al juicio de los demás. |
| **Acción Épica** | Momento épico o de genialidad que suma respeto (+aura). |
| **Acción Vergonzosa** | Momento ridículo o fallo público que resta respeto (-aura). |
| **Audiencia** | Personas (en vivo o en redes) que presencian y juzgan la acción. |
| **Veredicto** | Reacción de la audiencia que determina cuánta aura se suma o se resta. |

# Supuestos Adoptados

1. **Intención de destacar:** La persona busca activamente el reconocimiento social con sus acciones.
2. **Dependencia del público:** El aura no existe en un entorno vacio; requiere testigos que reaccionen al hecho.
3. **Fluctuación constante:** El aura sube con los momentos épicos y baja con momentos ridículos.
4. **Escala del impacto:** El cambio de aura es proporcional al tamaño y al tipo de audiencia presente.

# Decisiones de Modelado: Farmear aura 
* `Aura` se representa como una clase conceptual y no como un atributo numérico de `Individuo`, porque tiene reglas, umbrales y estados propios que cambian con el tiempo.
* `Audiencia` y `Veredicto` se modelan como entidades separadas de `Accion`, porque el cambio de aura no ocurre en soledad y depende enteramente del juicio del público.
* Se omiten deliberadamente los pensamientos y la autoestima del `Individuo`, porque los estados mentales ocultos no son observables ni atañen a la operación del sistema.
* No se incluyen identificadores técnicos ni persistencia, para no caer en el antipatrón de confundir el modelo del dominio con un diseño de base de datos.