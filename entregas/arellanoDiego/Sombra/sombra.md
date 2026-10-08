# Glosario

| Término | Definición Conceptual |
| :--- | :--- |
| **Fuente de Luz** | Elemento del entorno que emite iluminación (ej. el Sol, una bombilla o una linterna). |
| **Luz** | Haz o radiación luminosa proyectada en una dirección determinada. |
| **Objeto Opaco** | Cuerpo físico interpuesto que impide o bloquea total o parcialmente el paso de la luz. |
| **Superficie** | Plano o fondo físico sobre el cual impacta la luz no bloqueada. |
| **Sombra** | Región oscura o silueta proyectada sobre la superficie debido al bloqueo de la luz por el cuerpo opaco. |

# Supuestos Adoptados

1. **Dependencia espacial:** Una sombra no existe en el vacío; requiere simultáneamente de luz, un obstáculo y una superficie donde plasmarse.
2. **Propagación directa:** La luz se propaga en línea recta y el cuerpo opaco interrumpe físicamente su trayectoria.
3. **Perspectiva del observador:** La silueta visible cambia de escala, forma y posición según la distancia y el ángulo entre la fuente de luz y el cuerpo.
4. **Presencia material:** Se asume que tanto la fuente, el objeto y la superficie se encuentran en el mismo espacio físico contiguo.

# Decisiones de Modelado: Una Sombra

* `Sombra` se representa como un concepto independiente y no como un atributo de `ObjetoOpaco`, porque un mismo objeto puede producir varias sombras según las fuentes de luz presentes.
* `Superficie` se incluye como clase explícita y no implícita, porque sin un plano de proyección la silueta bidimensional visible no llega a materializarse en el dominio.
* `Superficie` contiene a `Sombra` mediante composición, porque una sombra no puede flotar ni existir desligada del plano físico que la soporta.
* Se descartan conceptos ópticos complejos (penumbra, refracción), para evitar la sobreingeniería y mantener el foco en la estructura esencial del dominio.