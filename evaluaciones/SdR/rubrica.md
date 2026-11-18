# Rúbrica

## Sistema de puntuación

<div align=center>

|Distribución de puntos||
|-|-|
Evidencia de interacción con cliente|2.0
Modelo del dominio|2.5
Casos de uso y criterios de aceptación|2.5
Trazabilidad y versionado|1.0
Organización del repositorio|1.0
Uso responsable de IA (declaración)|1.0

</div>

>**Modificadores adicionales, aplicados a la nota de la rúbrica antes de la nota final:**
>
>- Evaluación entre pares: multiplicador 0.75 a 1.10
>- Entrega tardía: -25% por día natural hasta 2 días (después: 0 pts salvo acuerdo)
>- Penalizaciones de la declaración de IA: se restan de la nota de la rúbrica

## Umbral de aprobación

El trabajo debe cumplir simultáneamente:

<div align=center>

|||
|-|-|
**Primer mínimo**|>= 1.0 puntos en "Evidencia de interacción con cliente" (≥0.8 si el cliente no responde y el equipo documenta los intentos, según el apartado completo)
**Segundo mínimo**|>= 1.2 puntos en "Modelo del dominio"
**Tercer mínimo**|>= 1.2 puntos en "Casos de uso y criterios de aceptación"
**Cuarto mínimo**|>= 5 sobre 10 en la nota final

</div>

Los tres primeros mínimos se comprueban sobre los puntos de cada apartado, antes de aplicar modificadores. Si no se alcanza cualquiera de ellos, la nota final es 0 independientemente del resto de puntuaciones.

## Nota final

La nota de la actividad combina la evaluación del profesor y la valoración del cliente:

**Nota final = 0.6 x nota de la rúbrica + 0.4 x nota del cliente**

- **Nota de la rúbrica** (sobre 10, con un máximo de 10): suma de los seis apartados con los modificadores aplicados. Evalúa los artefactos y la defensa ante el profesor.
- **Nota del cliente** (sobre 10): (media de los ítems 1 a 3 de su rúbrica - 1) x 2.5. Una media de 1 equivale a 0 y una de 5 a 10.

Si el cliente no completa su rúbrica, la nota final es la nota de la rúbrica.

## Evidencia de interacción con cliente (2.0 pts)

> Leer el [apartado completo](l'Rubrica.interaccionCliente.md)

El trabajo se desarrolla en fork del repositorio de la asignatura (https://github.com/mmasias/26-27-IdSw1-SdR) en la cuenta del coordinador del equipo. Toda la evidencia de interacción debe estar documentada en este repositorio.

## Modelo del dominio (2.5 pts)

> Leer el [apartado completo](l'Rubrica.MdD.md)

El modelo del dominio representa la realidad operativa del cliente en lenguaje conceptual, sin contaminación de decisiones de implementación. Se documenta en carpeta `/modelos/` del repositorio.

## Casos de uso y criterios de aceptación (2.5 pts)

> Leer el [apartado completo](l'Rubrica.CdU.md)

Los casos de uso describen la funcionalidad deseada del sistema desde la perspectiva de los actores. Se documentan en carpeta `/casos-de-uso/` del repositorio, con trazabilidad a evidencias de origen.

## Trazabilidad y versionado (1.0 pts)

> Leer el [apartado completo](l'Rubrica.T.md)

La trazabilidad emerge naturalmente del trabajo bien hecho en GitHub: commits descriptivos, issues que documentan decisiones con el cliente, y referencias cruzadas entre artefactos. No se requieren documentos adicionales de trazabilidad.

## Organización del repositorio (1.0 pts)

> Leer el [apartado completo](l'Rubrica.O.md)

Se valora la capacidad de organizar el trabajo de forma que sea navegable y auditable. No se prescribe una estructura única, pero debe seguir principios de claridad y trazabilidad similares a los ejemplos vistos en clase.

## Uso responsable de IA (declaración) (1.0 pts)

> Leer el [apartado completo](l'Rubrica.uRIA.md)

El uso de modelos de lenguaje (LLMs) es permitido como herramienta de apoyo, pero debe ser declarado y demostrar que hubo trabajo intelectual del equipo. El objetivo es transparencia, no prohibición.