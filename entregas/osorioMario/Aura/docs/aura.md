# Modelo de Dominio: La Mecánica de "Farmear Aura"

A continuación, se presenta el glosario de términos y la justificación de las decisiones de modelado para el aura, cubriendo las reglas generales (Clases), los casos específicos (Objetos) y el ciclo de vida social (Estados).

---

## 1. Glosario de Términos

### Entidades del Sistema (Modelos de Clases y Objetos)
* **Persona:** El individuo que pone en juego su estatus social y almacena su "aura actual".
* **Intento de Farmeo:**  Es la acción específica que la persona realiza (un chiste, un gesto, una frase) con la esperanza de verse genial o ganar validación.
* **Se vio forzado (Tryhard):** El atributo más crítico del intento. Es la métrica que define si la acción fluyó de forma natural o si se notó la desesperación por llamar la atención.
* **Contexto:** El escenario de la jugada. Define exactamente en qué lugar ocurrió y quiénes eran los jueces (el público).
* **Reacción del Público:** La respuesta inmediata del entorno, que puede ir desde la admiración total hasta la vergüenza ajena pura.
* **Consecuencia:** El cálculo final. La cantidad matemática de respeto (aura) que se suma o se resta tras el juicio del público.

### Estados Sociales (Modelo de Estados)
* **Normal:** El estado base de cualquier persona. No destacas, pero tampoco estás pasando vergüenza. Tu aura está intacta y pasas desapercibido.
* **En La Cima:** El *prime* social. Llegaste aquí porque te la jugaste y te salió bien de forma completamente natural.
* **En El Pozo:** La zona de cancelación o "cringe". El lugar donde terminas cuando fuerzas una situación y el público te rechaza masivamente.

---

## 2. Justificación de las Decisiones de Modelado

**1. Separar el "Intento" del "Contexto" (Modelo de Clases)**
![Diagrama de Clases de Aura](/entregas/osorioMario/Aura/images/diagramaClases.svg)

* *Decisión:* Se decidió modelar el `IntentoDeFarmeo` y el `Contexto` como clases separadas e interdependientes, en lugar de agrupar todo como atributos directos de la `Persona`.
* *Justificación:* Esto explica por qué una misma acción tiene resultados drásticamente distintos. Un gesto  (`IntentoDeFarmeo`) puede sumar muchísima aura si el `Contexto` es una fiesta con amigos, pero causará  cringe destructivo si el `Contexto` es algo mas serio.

**2. Uso de un "Objeto" para relatar el desastre (Modelo de Objetos)**
![Diagrama de Objetos de Aura](/entregas/osorioMario/Aura/images/diagramaObjetos.svg)

* *Decisión:* Se instanció a un sujeto real con atributos específicos y valores extremos (ej. Se vio forzado = "Sí, muchísimo", Impresión = "Vergüenza ajena") para explicar el diagrama de objetos.
* *Justificación:* Los diagramas de clases dictan reglas teóricas, pero el concepto de "aura" es netamente situacional y se mide en la práctica.

**3. La simplificación del "Pozo" y su única vía de escape (Modelo de Estados)**
![Diagrama de Estados de Aura](/entregas/osorioMario/Aura/images/diagramaEstados.svg)

* *Decisión:* Se dejó una única transición de salida desde el estado `EnElPozo` hacia el estado `Normal` etiquetada como "Aplica la ley del hielo (Silencio y tiempo)".
* *Justificación:* Intentar arreglar un momento que dio "cringe" hablando de más o justificándose siempre resulta en una mayor pérdida de aura. Por diseño lógico, el sistema bloquea cualquier intento de salida "activa". La única mecánica válida y realista para resetear el estatus es la inactividad prolongada, dejando que el entorno simplemente olvide el suceso.