# Propuesta de Modelo de Dominio

## Escenario 1: Famear (Diagrama de Estados)

Para este escenario se optó por un diagrama de estados. Esto refleja fielmente la dinámica de cómo cambia la percepción relativa ("Aura") de una persona al interactuar y tratar de destacar ("Famear") dentro de un grupo social o contexto (el "chat" o el "lobby").

![DDE_Farmear_Aura](/entregas/navasNicolas/diagramas_UML/DDE_Farmear_Aura_2.png)

## Diagrama de Objetos (Instancia de Dominio)

Para complementar la dinámica general, se presenta un diagrama de objetos que ilustra un momento específico en el tiempo: una "fotografía" de cuando un usuario logra farmear aura con éxito.

![DDO_Farmear_Aura](/entregas/navasNicolas/diagramas_UML/DDO_Farmear_Aura.png)

### Explicación de la Instancia
Este diagrama representa el flujo exacto de un evento de validación social exitoso:
* **El Sujeto:** Un usuario concreto (`bro_1`) con un estado mental "Confiado" realiza una acción.
* **La Acción:** Se ejecuta un `intento_73` de riesgo "Alto", que consiste específicamente en "Tirar un fact basado".
* **El Contexto:** Esta acción es evaluada por el `lobby_principal`, que en este caso es un Twitch Chat / Discord con una vibra general "Exigente".
* **El Resultado:** El contexto valida la acción dictaminando una victoria (`W`). Esto instancia temporalmente un `EstadoAura` (`aura_actual`) de nivel "Modo_Prime", otorgándole a la persona un multiplicador de "+10000 Aura".

### Glosario (Lenguaje del Dominio)

* **Entra al lobby / AFK (Away From Keyboard):** Acciones de entrada y salida definitiva del sistema o contexto social.
* **Chilling:** Estado base o neutral. El aura es continua; se mantiene en un nivel estable mientras el usuario no interactúe de forma arriesgada.
* **Hace lock-in:** Transición que marca el momento en el que el usuario deja de estar *chill* para concentrarse e intentar destacar.
* **Farmeando:** Estado activo donde el sujeto se expone al juicio del contexto.
* **Modo_Prime (+Aura):** Estado de alta validación social, alcanzado tras la aprobación del contexto (*W chat / Basado*).
* **Flop (-Aura):** Estado de pérdida de estatus tras un rechazo del contexto (*L chat / Da cringe*) o por cometer un error grave estando en la cima (*Clip forzado / Se regala*).
* **Busca el clip / All-in:** Decisión de arriesgar el aura ganada en *Modo_Prime* para intentar una jugada aún mayor.
* **Arc de redención / Eu Farei 10x se preciso:** Referencia cultural (meme de superación) que representa el esfuerzo por salir del estado de *Flop* y volver a farmear aura.
* **Se vuelve Gigachad:** Retirarse del foco de atención estando en la cima (*Modo_Prime*), consolidando el respeto y volviendo al estado *Chilling*.
* **lowkey se va / Pa'l lobby:** Retirada discreta tras un *Flop* para volver al estado base esperando que el contexto olvide el incidente.

### Supuestos y Decisiones Discutibles

#### Supuestos
1. El nivel inicial de "aura" de la persona no es necesario aclararlo ni cuantificarlo. Lo relevante es meramente la observación de su variación (sube o baja) como una **hipérbole cuantitativa**.
2. El entorno ("chat") juzga de forma binaria en la mayoría de los casos: o es una victoria (*W*) o es una derrota (*L*).
3. El aura nunca desaparece del todo. Se asume un estado inicial y de reposo (*Chilling*) al que el usuario siempre regresa tras un evento de validación, ya sea coronándose (*Se vuelve Gigachad*) o retirándose discretamente (*lowkey se va*).

#### Decisiones de Modelado
* **Uso de diagrama de estados en lugar de clases:** Se descartó el modelo de dominio estructural tradicional porque "Famear" es fundamentalmente un comportamiento y una fluctuación de estatus, no una entidad estática. El diagrama de estados captura de forma precisa la volatilidad de la validación social.
* **Adopción de Lenguaje Ubicuo (Jerga de internet):** Se decidió utilizar términos orgánicos del ecosistema digital ("lock-in", "All-in", "Eu Farei 10x se preciso", "Gigachad"). En el Diseño Guiado por el Dominio (DDD), el modelo debe hablar el mismo idioma que los expertos del negocio (la comunidad de usuarios), evitando traducciones formales o académicas que destruyan el significado real de las interacciones.