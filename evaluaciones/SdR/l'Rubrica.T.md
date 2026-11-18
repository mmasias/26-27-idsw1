# Trazabilidad y versionado (1.0 pts)

La trazabilidad sale sola del trabajo bien hecho en GitHub: commits descriptivos, issues que documentan las decisiones con el cliente y referencias cruzadas entre artefactos. No hay que entregar ningún documento adicional de trazabilidad.

## Qué

- Commits cuyo mensaje explique qué cambió y por qué, y que mencionen el issue relacionado cuando lo haya (#23).
- Issues y discussions con el cliente, cerrados con la decisión tomada.
- Casos de uso con su origen (minuta o issue) y su validación.
- Versiones del modelo identificables (dominio-v1, v2, v3...), con una nota en `/modelos/` de cómo evolucionó.
- Todos los miembros con `git config` correcto (nombre y correo reales) y aportaciones propias en el historial.

## Cómo

Lo que importa es que se vea el trabajo ocurrir: que el historial cuente una evolución y no una entrega de última hora. Un commit como `fix(cu-03): corrige flujo alternativo según feedback #45` dice mucho; `update` o `cambios` no dicen nada. El trabajo debe estar repartido en el tiempo y entre los miembros, y las fechas de los commits deben ser coherentes con las de las minutas.

Con un historial claro, issues enlazados con commits y trabajo repartido hay buena nota. Con un historial razonable pero con commits genéricos o muy concentrados en una o dos personas, aprobado.

## Incidencias

Si un miembro estuvo ausente por causa justificada, se documenta en un issue y no se penaliza el reparto. Los commits de merge automáticos y los squash con mensaje descriptivo no cuentan en contra.

## Innegociable

Commits con fechas manipuladas para aparentar trabajo anterior, o issues que declaran validación del cliente sin que él haya intervenido, anulan el apartado.

## Para escribir buenos commits

No puntúa, pero ayuda: `tipo(ámbito): qué cambió`, como `feat(mdd): añade clase Reserva`, `docs(glosario): define 'disponibilidad' según cliente`.