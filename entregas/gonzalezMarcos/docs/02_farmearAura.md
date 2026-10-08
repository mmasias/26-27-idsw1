[← Anterior: Una sombra](01_sombra.md) · [🏠 README](../README.md) · [→ Siguiente: El concepto de simpatía](03_simpatia.md)

---

# Reto 001 — Modelado: Farmear aura

## 1 · Estudio del dominio

### Diagrama de clases base

Se identifican los conceptos clave del dominio "farmear aura" tal y como los entendería un adolescente (el cliente):

![Diagrama de clases base](./../images/02_farmearAura_basico.png)

### Glosario inicial

| Término | Qué es |
|---|---|
| **Chaval** | Un adolescente cualquiera, el que hace cosas o el que las ve hacer. |
| **Aura** | Lo que los demás piensan de ti. Tu reputación, el rollo que transmites. |
| **Movida** | Algo concreto que hace una persona: un comentario, un gesto, algo que se marca. |
| **Momento** | Dónde y cuándo pasa la movida: en clase, en el recreo, en una fiesta... |
| **Gente** | Los que están ahí mirando y juzgando lo que haces. El público. |

### Supuestos

1. El aura no es un número fijo, es algo que la gente siente sobre ti.
2. Una misma movida puede "molarle" a unos y parecerle "cringe" a otros.
3. El momento importa: lo que "mola" en una fiesta puede ser "ridículo" en clase.
4. No hay una fórmula mágica para calcular el aura, es subjetivo.

---

## 2 · Elaboración y estructuración

### Diagrama de clases con relaciones

Se conectan los conceptos usando verbos que diría un adolescente (cliente):

![Diagrama de clases con relaciones](./../images/02_farmearAura_clases.png)

| Relación | Qué significa |
|---|---|
| Chaval — Aura : **tiene** | Cada chaval tiene su propia aura |
| Chaval — Movida : **se marca** | Un chaval se marca una movida (hace algo) |
| Movida — Momento : **pasa en** | Cada movida pasa en un momento concreto |
| Gente — Movida : **mira** | La gente que está ahí mira lo que haces |
| Movida — Aura : **sube o baja** | Según la movida, tu aura sube o baja |

### Diagrama de objetos

Un ejemplo concreto: *"Pepe suelta un chiste en el recreo, los colegas lo miran y su aura sube"*

![Diagrama de objetos](./../images/02_farmearAura_objetos.png)

### Glosario expandido

| Término | Qué es |
|---|---|
| **Chaval** | Un adolescente cualquiera, el que hace cosas o el que las ve hacer. |
| **Aura** | Lo que los demás piensan de ti. Tu reputación, el "rollo" que transmites. No es algo tuyo solo: depende de lo que piense la gente. |
| **Movida** | Algo concreto que hace un chaval: un comentario, un gesto, algo que se marca. Es lo que la gente ve y juzga. |
| **Momento** | Dónde y cuándo pasa la movida: en clase, en el recreo, en una fiesta... El contexto que hace que algo mole o sea "cringe". |
| **Gente** | Los que están ahí mirando y juzgando lo que haces. El público. No tienen por qué ser desconocidos; son otros chavales. |
| **se marca** | Cuando un chaval hace algo, "se marca una movida". |
| **sube o baja** | El efecto que tiene una movida en tu aura. Si la gente flipa, sube. Si les parece "cringe", baja. |

---

## 3 · Refinamiento y detalle

### Diagrama de clases con multiplicidades y atributos

Se añaden multiplicidades (cuántos de cada cosa) y atributos (qué datos tiene cada concepto):

![Diagrama de clases detallado](./../images/02_farmearAura_detallado.png)

| Multiplicidad | Qué quiere decir |
|---|---|
| Chaval **1** — **1** Aura | Cada chaval tiene exactamente un aura |
| Chaval **1** — **0..*** Movida | Un chaval puede marcarse ninguna, una o muchas movidas |
| Movida **1** — **1** Momento | Cada movida pasa en un momento concreto |
| Gente **0..*** — **1** Movida | Una movida puede ser mirada por nadie o por mucha gente |
| Movida **0..*** — **1** Aura | Un aura se ve afectada por muchas movidas a lo largo del tiempo |

### Diagrama de estados del Aura

El aura de un chaval no es fija: va cambiando según lo que haga y cómo reaccione la gente:

![Diagrama de estados del Aura](./../images/02_farmearAura_estados.png)

| Estado | Qué significa |
|---|---|
| **Normal** | Aura estable, ni sube ni baja. El chaval no se ha marcado nada. |
| **Subiendo** | La gente empieza a flipar contigo. Vas bien. |
| **Bajando** | La gente empieza a verte cringe. Vas mal. |
| **PorLasNubes** | Eres el amo. Todo lo que haces mola. |
| **PorLosSuelos** | Estás cancelado. Todo lo que haces está mal. |

### Glosario refinado

| Término | Qué es |
|---|---|
| **Chaval** | Un adolescente cualquiera (nombre, edad). Puede marcarse movidas y tiene un aura. |
| **Aura** | La reputación de una persona (nivel, tendencia). Depende de sus movidas y de lo que opine la gente. No es algo fijo: tiene estados (normal, subiendo, bajando, por las nubes, por los suelos). |
| **Movida** | Algo concreto que hace un chaval (tipo, intención). Un chaval se marca 0 o muchas. Cada una pasa en un momento y puede ser mirada por gente. |
| **Momento** | El contexto (lugar, cuándo). Cada movida ocurre en exactamente uno. |
| **Gente** | Los que miran (quiénes, reacción). Pueden ser 0 o muchos mirando una movida. |

---

## 4 · Profundización y extensión

### Diagrama de secuencia

Se muestra el flujo completo de lo que pasa cuando un chaval se marca una movida, paso a paso:

![Diagrama de secuencia](./../images/02_farmearAura_secuencia.png)

**Lectura del diagrama:**

1. **Pepe se marca** una movida (soltar un chiste).
2. La movida **pasa en** un momento concreto (el recreo).
3. La gente (los colegas) **mira** la movida.
4. La gente **juzga** si mola o es cringe.
5. Según el veredicto:
   - Si la gente **flipa** → el aura **sube** y pasa al estado "Subiendo".
   - Si la gente lo ve **cringe** → el aura **baja** y pasa al estado "Bajando".

### Glosario final

| Término | Qué es |
|---|---|
| **Chaval** | Un adolescente cualquiera (nombre, edad). Puede marcarse movidas y tiene un aura. Es tanto el que hace cosas como el que las mira. |
| **Aura** | La reputación de una persona (nivel, tendencia). No es algo fijo: cambia según las movidas y la reacción de la gente. Tiene 5 estados posibles: normal, subiendo, bajando, por las nubes, por los suelos. |
| **Movida** | Algo concreto que se marca un chaval (tipo, intención). Un chaval se marca 0 o muchas. Cada una pasa en un momento, es mirada por gente, y sube o baja el aura. |
| **Momento** | El contexto donde pasa la movida (lugar, cuándo). Importa porque la misma movida puede molar en un sitio y ser cringe en otro. |
| **Gente** | Los que miran y juzgan (quiénes, reacción). Pueden ser 0 o muchos. Su reacción es lo que decide si el aura sube o baja. |
| **se marca** | Cuando un chaval hace algo. "Pepe se marca un chiste". |
| **mira** | Cuando la gente ve lo que hace un chaval. |
| **sube o baja** | El efecto de una movida en el aura. Depende de la reacción de la gente. |
| **flipa** | Reacción positiva de la gente. El aura sube. |
| **cringe** | Reacción negativa de la gente. El aura baja. |

### Decisiones de modelado

**¿Por qué Aura es un concepto propio y no un dato del Chaval?**
Se podría poner el aura como un dato dentro de Chaval (como una puntuación). Pero el aura no depende solo del chaval: depende de lo que piense la gente. Tratarla como concepto separado refleja mejor eso.

**¿Por qué Gente es un concepto propio y no otro Chaval?**
Un observador sí es otro chaval, pero en el contexto de "farmear aura" lo que importa es su papel como público que mira y juzga. Separarlo aclara la dinámica.

**¿Por qué el diagrama de estados solo modela el Aura?**
Porque el aura es el único concepto cuyo estado cambia de forma interesante a lo largo del tiempo. El chaval no cambia de estado: simplemente se marca movidas.

**¿Por qué el diagrama de secuencia usa el mismo ejemplo que el de objetos?**
Para mantener la consistencia. El mismo escenario ("Pepe suelta un chiste en el recreo") se ve desde diferentes ángulos: la estructura (objetos) y el flujo temporal (secuencia).

---

## 5 · Refinamiento UML Avanzado

Para aportar mayor riqueza semántica (como se recomienda en la teoría de modelado avanzado), refinamos el diagrama de clases para distinguir el tipo de relaciones (asociación, agregación y composición) y extraer posibles valores a enumerados.

![Diagrama de clases extendido](./../images/02_farmearAura_extendido.png)

### Mejoras introducidas:

1. **Composición (`*--`)**: Un `Chaval` está compuesto indisolublemente por su `Aura`. Si el chaval desaparece, su aura (en este contexto) también.
2. **Agregación (`o--`)**:
   - `Gente` es una agregación de `Chaval` (el público son otros chavales).
   - Un `Momento` engloba o agrupa varias `Movida` (en un mismo recreo pasan muchas cosas).
3. **Enumerados (`enum`)**: 
   - Se definen explícitamente los estados de `TendenciaAura` (*PorLasNubes*, *Subiendo*, *Normal*, *Bajando*, *PorLosSuelos*) basados en el diagrama de estados previo.
   - Se definen las reacciones en `ReaccionGente` (*Flipa*, *NiFuNiFa*, *Cringe*).
4. **Navegabilidad (`-->`)**: Queda claro que la `Gente` observa la `Movida`, y es la `Movida` la que altera el `Aura` (flujo de impacto unidireccional).

---

[← Anterior: Una sombra](01_sombra.md) · [🏠 README](../README.md) · [→ Siguiente: El concepto de simpatía](03_simpatia.md)
