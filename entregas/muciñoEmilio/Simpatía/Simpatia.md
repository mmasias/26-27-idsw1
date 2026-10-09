# Modelo del Dominio: Simpatía Social

## 1. Contexto del Modelo del Dominio

El **Modelo del Dominio de Simpatía** evalúa cómo la conducta social y la disposición de una persona generan sensaciones de calidez, apertura y afinidad en su entorno. En lugar de tratar la simpatía como un rasgo estático o innato, este modelo la aborda como un estado dinámico que resulta de la percepción acumulada y filtrada de sus interacciones cotidianas.

### Flujo del Dominio
1. Una **Persona** realiza una **Interacción Social** (conversar, escuchar, sonreír o dar apoyo público o privado).
2. La interacción es recibida por un **Interlocutor**, generando una **Percepción Empática** basada en el trato recbido.
3. El **Criterio de Afinidad** contrasta las impresiones recibidas contra sus propios umbrales sociales, emocionales o culturales.
4. Se procesa la **Evaluación de Simpatía** (reflejando el índice de afinidad actual), determinando si la persona es percibida con alta simpatía dentro de ese grupo o contexto.

---

## 2. Glosario de Términos

| Término | Definición Técnica y Operativa |
| :--- | :--- |
| **Simpatía** | Estado dinámico y cualidad percibida que refleja la capacidad de un sujeto para transmitir calidez, trato agradable y cercanía |
| **Interacción Social** | Evento comunicativo, gesto o conducta observable ejecutada por una persona dentro de un marco de convivencia. |
| **Interlocutor / Receptor** | Entidad o sujeto que participa en el intercambio comunicativo y experimenta de forma directa el impacto del gesto. |
| **Percepción Empática** | Registro cualitativo y afectivo del grado de agrado o calidez dejado por una interacción específica en un receptor. |
| **Criterio de Afinidad** | Conjunto de reglas, expectativas culturales y filtros de tolerancia que determinan si una percepción satisface las expectativas del entorno. |
| **Evaluación de Simpatía** | Resultado consolidado que determina si las interacciones recientes superan el umbral para considerar a la persona como simpática. |
| **Índice de Afinidad** | Valor flotante acumulativo que representa el nivel de aceptación y receptividad social de la persona. |

---

## 3. Suposiciones del Modelo

1. **Estructura Dinámica y Variable:** La simpatía no es una propiedad inmutable del sujeto; se construye, fortalece o degrada según la calidad y frecuencia de las interacciones recientes.
2. **Dependencia del Filtro Contextual:** Un mismo comportamiento puede generar niveles de afinidad drásticamente opuestos según el estado emocional, bagaje cultural o expectativas del interlocutor.
3. **Necesidad de Escucha Activa:** El incremento de la simpatía no depende únicamente de la expresividad (humor, habla), sino del equilibrio con la capacidad de escucha y empatía manifestada hacia el otro.

---

## 4. Decisiones de Modelado

### Decisión 1: Clase Asociación (`PercepciónEmpática`)
* **Decisión:** Se define `PercepcionEmpatica` como una clase asociación entre `InteraccionSocial` e `Interlocutor`.
* **Justificación:** Una misma frase o gesto social puede ser interpretado como simpático por una persona y neutro/pesado por otra. Vincular ambas entidades a través de esta asociación permite capturar la impresión única que deja un acto en cada receptor individual.

### Decisión 2: Desacoplamiento de Reglas mediante `CriterioAfinidad`
* **Decisión:** Los umbrales de aprobación y tolerancias contextuales se aíslan en la entidad `CriterioAfinidad`.
* **Justificación:** Cada grupo o cultura posee estándares de interacción diferentes. Aislar las reglas de evaluación de los datos del sujeto permite que una misma interacción sea calificada por distintos criterios de afinidad sin duplicar datos.

### Decisión 3: Condición Derivada e Índice Continuo
* **Decisión:** La condición de "simpático" no se asigna de forma manual, sino que se deriva de la suma de evaluaciones ponderadas procesadas en el tiempo.
* **Justificación:** Refleja la naturaleza fluida de las relaciones humanas, donde la reputación de calidez social se ajusta continuamente ante nuevos actos.

---

## 5. Representación Gráfica del Modelo

![Modelo del Dominio Simpatia](./Simpatia.png)



