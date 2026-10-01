# Glosario

| Término | Definición Conceptual |
| :--- | :--- |
| **Persona** | Individuo capaz de comunicarse y relacionarse con otros en un entorno social. |
| **Gesto Social** | Acción observable o señal manifiesta de trato (ej. saludo, sonrisa, tono cordial o escucha activa). |
| **Interlocutor** | Otra persona que presencia, recibe y experimenta el trato emitido. |
| **Afinidad** | Sensación de conexión, cercanía o comodidad generada en quien recibe la interacción. |
| **Simpatía** | Valoración o cualidad otorgada por el interlocutor a la persona cuando su comportamiento produce agrado. |

# Supuestos Adoptados

1. **Naturaleza relacional:** La simpatía no existe de forma aislada; requiere obligatoriamente una interacción entre al menos dos personas.
2. **Dependencia del interlocutor:** Calificar a alguien como "simpático" depende enteramente de la percepción de quien recibe el trato; no es una cualidad absoluta e independiente.
3. **Foco en hechos observables:** Se modelan únicamente los gestos y conductas visibles o audibles, omitiendo intenciones o pensamientos no manifiestos.
4. **Variabilidad situacional:** Una misma conducta puede generar afinidad en una persona y rechazo o indiferencia en otra según el contexto.

# Decisiones de Modelado: Concepto de simpatía

* `Simpatia` se representa como un concepto independiente y no como un atributo booleano de `Persona`, porque no es una cualidad biológica fija sino un juicio que varía con cada observador.
* `GestoSocial` y `Afinidad` se modelan como clases separadas, porque una conducta objetiva (sonreír) no garantiza automáticamente una respuesta de agrado en quien la recibe.
* `Simpatia` se asocia al `Interlocutor` para calificar a la `Persona`, porque en la realidad operativa la simpatía solo existe si un tercero la reconoce.
* Se excluyen intenciones internas y buena fe, limitando el modelo estrictamente a conductas y estímulos perceptibles del dominio.