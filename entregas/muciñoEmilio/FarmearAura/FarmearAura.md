# Modelo del Dominio: Farmear Aura

## 1. Contexto del Modelo del Dominio

El **Modelo de Farmear Aura** abstrae un sistema de puntuación social y reputación dinámica en el que los individuos (*Personas*) ejecutan actos públicos (*Acciones Realizadas*) sometidos a juicio colectivo (*Observadores*). El propósito principal del sistema es cuantificar la influencia, elegancia y temple de un individuo mediante un balance (*Aura*), ponderando la recompensa por el riesgo asumido contra el costo social de la torpeza o el ridículo.

### Flujo del Dominio
1. Una **Persona** realiza una **Acción** pública dentro de un contexto dado.
2. Un **Observador** presenciará el acto y evaluará su nivel de estilo, riesgo o torpeza.
3. El **Criterio de Aura** aplica multiplicadores según la dificultad, el ridículo y la autenticidad de la acción.
4. Se crea un **Impacto de Aura** (+Aura o -Aura) que actualiza el balance acumulado del individuo.

---

## 2. Glosario de Términos

| Término | Definición Técnica y Operativa |
| :--- | :--- |
| **Aura** | Métrica cuantitativa acumulativa que representa la reputación, prestigio y capital social de una Persona. |
| **Farmear Aura** | Patrón de comportamiento sistemático donde un individuo busca situaciones de alto riesgo y recompensa social para el incremento de su Aura. |
| **Acción Realizada** | Evento discreto e imputable ejecutado por una persona en un espacio/tiempo determinado, caracterizado por su nivel de riesgo y estilo. |
| **Observador / Testigo** | Entidad o actor que presencia la acción y emite un juicio/evaluación que valida o descarta la legitimidad del acto. |
| **Evaluación** | Registro de la percepción de un Observador sobre una Acción específica. Otorga validez a la acción según la autenticidad percibida. |
| **Criterio de Aura** | Conjunto de políticas y funciones matemáticas de dominio que ponderan variables (riesgo, estilo, torpeza, audiencia) para emitir un resultado final. |
| **Impacto de Aura** | Transacción o delta con signo (+ / -) que modifica el estado o balance general de Aura de la Persona. |
| **Quiebre de Aura** | Penalización drástica no lineal sufrida cuando una acción de alto riesgo fracasa públicamente. |

---

## 3. Suposiciones del Modelo

1. **Necesidad de Consenso y Acreditación:** Una acción no presenciada ni validada por al menos un Observador válido carece de impacto en el Aura ($\Delta \text{Aura} = 0$).
2. **Asimetría del Impacto (Ganancia vs. Pérdida):** El Aura se incrementa de forma escalar y progresiva ante los aciertos, pero su pérdida ante fallos públicos es penalizada de forma severa y exponencial (*Quiebre de Aura*).
3. **Inmutabilidad del Registro de Impacto:** Una vez consolidado un `ImpactoAura`, este no puede ser modificado. Nuevas acciones o rectificaciones generan un nuevo registro de impacto independiente.

---

## 4. Decisiones de Modelado

### Decisión 1: Introducción de Clase Asociación (`Evaluación`)
* **Decisión:** Se define `Evaluacion` como una clase asociación situada entre `AccionRealizada` y `Observador`
* **Justificación:** Una misma acción puede provocar reacciones o percepciones opuestas dependiendo de quién la presencie. Asociar directamente la acción con el observador a través de una entidad de evaluación permite registrar la validez y el grado de autenticidad asignado por cada testigo individual.

### Decisión 2: Desacoplamiento entre Reglas (`CriterioAura`) e `ImpactoAura`
* **Decisión:** Las reglas de negocio y multiplicadores se aíslan en la entidad `CriterioAura`, separándola de la transacción de resultado (`ImpactoAura`).
* **Justificación:** Los factores de ponderación (por ejemplo, tendencias o contexto cultural) pueden fluctuar con el tiempo. Desacoplar las reglas permite modificar los multiplicadores sin alterar el historial ni la estructura de los impactos ya registrados.

### Decisión 3: Balance Signado Único
* **Decisión:** Se utiliza un valor entero con signo (+/-) en `ImpactoAura` para consolidar las variaciones en lugar de registrar entidades separadas para pérdidas y ganancias.
* **Justificación:** Simplifica las operaciones aritméticas directas sobre el balance total del sujeto y mantiene una traza de auditoría clara en el dominio.

---

## 5. Representación Gráfica del Modelo

![Modelo del Dominio Farmear Aura](./FarmearAura.png)
