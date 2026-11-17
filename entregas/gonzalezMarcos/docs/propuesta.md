# Propuesta de corrección

[Volver al README](../README.md) · [Ver el análisis](analisis.md)

Estos dos diagramas desarrollan las correcciones de I02, D12, D13 y D17. Mantengo los tres estados y los casos de uso del ejemplo. Son mi propuesta para aclararlos, no una nueva versión completa de todos los requisitos.

## Estados del sistema

![Propuesta de estados del sistema](../images/estados.svg)

[Fuente PlantUML](../modelosUML/estados.puml)

Añado el paso de `EN_CURSO` a `INACTIVO` cuando se confirma el reinicio. También indico qué provoca el fin de la partida y aclaro que `FINALIZADO` no significa cerrar la aplicación. Los nombres se escriben igual en los dos diagramas.

El diagrama resume los resultados de cada caso de uso. Si el reinicio se cancela, se conserva el estado anterior, como se detalla a continuación.

## Detalle de reiniciarPartida()

- **Actor:** Jugador.
- **Antes de empezar:** la partida está en `EN_CURSO` o `FINALIZADO` y no hay otra confirmación abierta.
- **Inicio:** el jugador solicita reiniciar la partida.

**Flujo principal:**

1. El sistema presenta una confirmación y permite confirmar o cancelar.
2. El jugador solicita confirmar.
3. El sistema presenta la pantalla inicial, sin pieza activa ni bloques fijados, con puntuación cero y modo de caída normal.
4. El sistema permite solicitar iniciar una partida. El estado queda en `INACTIVO`.

**Alternativa:** si el jugador solicita cancelar, el sistema presenta la partida o el resultado final sin cambios. Se conserva el estado desde el que se pidió reiniciar.

Mientras se espera la respuesta, la partida no avanza ni permite mover, girar o alternar la velocidad. Si se cancela desde `EN_CURSO`, se reanuda la caída con un intervalo completo del modo que estaba activo. No se acumulan descensos durante la espera. Esto evita que la partida termine mientras se está confirmando.

![Detalle propuesto de reiniciar partida](../images/reiniciarPartida.svg)

[Fuente PlantUML](../modelosUML/reiniciarPartida.puml)

## Ejemplos para comprobarlo

| Situación | Solicitud | Resultado esperado |
|---|---|---|
| Partida en curso con puntos y bloques fijados | Reiniciar y confirmar | `INACTIVO`, sin bloques, puntuación 0 y modo normal |
| Partida finalizada | Reiniciar y confirmar | `INACTIVO`, con opción de iniciar otra partida |
| Partida en curso | Reiniciar y cancelar | Misma pieza, tablero, puntos y velocidad; continúa en `EN_CURSO` |
| Partida finalizada | Reiniciar y cancelar | Conserva tablero y puntuación; sigue en `FINALIZADO` |
| Confirmación abierta durante una partida | Esperar sin responder | La pieza no baja ni se modifica la partida |
| Reinicio confirmado | No solicitar todavía iniciar | Se mantiene en `INACTIVO`; no aparece una pieza nueva |

Estos ejemplos sirven para revisar la propuesta. No son pruebas de un programa, ya que esta entrega no tiene implementación.

## Generar las imágenes

Las dos imágenes se generan desde sus fuentes PlantUML en formato SVG. Desde la carpeta `gonzalezMarcos`, con PlantUML disponible:

```sh
java -jar /ruta/plantuml.jar -charset UTF-8 -tsvg -o ../images modelosUML/*.puml
```
