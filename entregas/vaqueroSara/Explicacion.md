**Glosario**

- **Fuente de luz**: elemento que emite luz. Es puntual (sin tamaño) o extensa.
- **Cuerpo**: objeto físico con forma y un grado de opacidad.
- **Superficie**: cara de un cuerpo que puede recibir luz.
- **Oclusor**: papel de un cuerpo que bloquea la luz de una fuente.
- **Superficie receptora**: superficie sobre la que se observa la sombra.
- **Sombra**: región de una superficie que recibe menos luz de una fuente porque un oclusor la bloquea.
- **Umbra**: zona de la sombra con bloqueo total.
- **Penumbra**: zona de la sombra con bloqueo parcial; solo aparece con fuentes extensas.

**Supuestos**

1. Se modela la sombra en sentido físico. Quedan fuera los sentidos metafóricos.
2. La escena es estática, en un instante dado; no se modela el movimiento.
3. Las fuentes de luz no proyectan ni reciben sombras.
4. Cada sombra cae sobre una única superficie. Si cae sobre suelo y pared, son dos sombras.
5. No se modela la oscuridad resultante cuando se solapan sombras de varias fuentes.

**Justificación de decisiones discutibles**

1. **Sombra como clase y no como atributo de Cuerpo.** La sombra depende de tres elementos (fuente, oclusor y superficie), no solo del cuerpo. Un mismo cuerpo tiene tantas sombras como fuentes y superficies, y sin luz la sombra desaparece aunque el cuerpo siga ahí.

2. **Modelar una ausencia de luz.** Aunque físicamente es falta de luz, la sombra tiene forma, tamaño e intensidad observables, y se habla de ella como entidad. Se modela con atributos derivados porque no existe por sí misma, sino que se calcula a partir del resto de la escena.

3. **Oclusor y receptora como roles, no subclases.** Un mismo cuerpo puede ser oclusor y receptor a la vez, incluso de su propia sombra. Con subclases eso no se podría representar.

4. **Superficie como parte compuesta de Cuerpo.** La sombra cae sobre una cara concreta, no sobre el cuerpo entero, y una superficie no existe sin su cuerpo.

5. **Umbra y penumbra como partes de la sombra, no como tipos de sombra.** Una sombra de fuente extensa tiene ambas zonas a la vez, así que no son tipos excluyentes.