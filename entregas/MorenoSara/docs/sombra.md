# UNA SOMBRA
Luz choca contra un objeto opaco e interrumpe su trayectoria, proyectando una silueta.
## Glosario de términos:
- *Luz* (Fuente emisora de la luz)
- *Objeto* (Interrumpe la trayectoria de la luz)
- *Superficie* (Donde se proyecta y halla la sombra)
- *Sombra* (Proyección de la silueta del objeto)
## Supuestos:
1. **Objeto opaco**
2. **Triángulo de dependencia:** Es necesario contar con luz, obstaculo y superficie. Si falta alguna no se origina la sombra. Si se modifica alguna se origina una nueva sombra. (es única).

Fuente de Luz -[emite]-> Objeto
Objeto -[Proyecta]-> Superficie

## Justificación:
*Inexistencia de la sombra como entidad*: No posee comportamiento propio ni puede existir de forma aislada. Fisicamente, es un fenómeno originado por la relación luz, objeto y sombra. Si no existe la relación, se destruye la sombra.

*Inmutabilidad y unicidad*: Cualquier cambio en la posición o estado de los tres elementos destruyen la sombra existente y genera una nueva. (Sombra es única)

*Inclusión de superficie como clase conceptual:* en lugar de un atributo. Sin un recpetor físico donde interceotar los rayos, la luz continua o se disipa impidiendo que se genere la sombra.
