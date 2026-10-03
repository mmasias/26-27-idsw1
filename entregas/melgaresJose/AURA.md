# Escenario 2. Farmear Aura

## Esquema

![Esquema ](./img/aura-diagrama.png)


## Glosario de Términos

- **Aura**: Capital intangible y métrica social de presencia, serenidad, estilo e indiferencia. Representa qué tan "imponente" o "mítico" es percibido un individuo.
- **Farmear** Aura: Acción deliberada o circunstancial de ejecutar gestos, respuestas o conductas estratégicas (con apariencia de "cero esfuerzo") para acumular puntos de aura frente a los demás.
- **Sujeto** (Aura Farmer): Individuo que ejecuta la hazaña o reacciona ante una situación buscando proyectar dominio o coolness.
- **Hazaña** (o Flex): Evento o acción concreta expuesta (ej. atajar un vaso en el aire sin mirar, mantener la calma en un caos, responder con frialdad) que sirve como insumo para el farmeo.
- **Audiencia**: Testigos presenciales o digitales (seguidores, espectadores, comunidad) que validan o penalizan la acción.
- **Evento** de Aura: Ajuste explícito de puntos (+10.000 / -50.000 de aura) resultante de la reacción de la audiencia.

## Supuestos Adoptados

Riesgo de Cringe:

- Justificación: Farmear aura no es gratis. Si la Audiencia percibe que la hazaña requirió mucho esfuerzo o fue actuada/forzada, el EventoDeAura genera un resultado negativo (esPerdida = Verdadero, entrando en "deuda de aura"). Por eso el atributo esfuerzoPercibido en Hazana es clave.

El Aura como saldo dinámico único:

- Justificación: Puntos de aura que tiene el individuo

Rol de la Audiencia como ente emisor:

- Justificación: El aura no existe en el aislamiento. Una hazaña a solas no farmea aura a menos que quede registrada para una Audiencia (digital o física) que emita el veredicto.