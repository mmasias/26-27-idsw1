# Modelo del dominio (2.5 pts)

El modelo del dominio representa la realidad operativa del cliente en lenguaje conceptual, sin contaminación de decisiones de implementación. Se documenta en la carpeta `/modelos/` del repositorio.

## Qué

- Diagrama UML de clases conceptuales (archivo `.puml` y exportación `.svg` o `.png`).
- Glosario de términos.
- Al menos dos escenarios de ejemplo con objetos concretos.
- Diagramas dinámicos (estados, actividad o secuencia) para los procesos clave.

## Cómo

Lo que importa es que el cliente reconozca su mundo en el modelo: sus cosas, con sus nombres, relacionadas como él las vive. Deben estar modeladas las entidades que mencionó, con el vocabulario del cliente y no con el de ustedes. Las relaciones deben decir algo: si una cosa es parte de otra, que sea composición; si solo se asocian, asociación. Un diagrama donde todo se une con líneas simples es un mapa mental, no un modelo.

El glosario debe definir las clases y todo lo que pueda resultar ambiguo. Los escenarios con objetos concretos (esta factura, este paciente, este examen) sirven para comprobar que el modelo funciona con casos reales. Los diagramas dinámicos muestran cómo cambian las cosas a lo largo del proceso.

El modelo debe pasar por varias versiones, con constancia de qué cambió y por qué, y el cliente debe haber validado el modelo en un issue, una minuta o una revisión de PR con referencia concreta a clases o relaciones. Un "me parece bien" genérico no cuenta.

Con un modelo completo, validado por el cliente y con historia de versiones hay buena nota. Con un modelo razonable pero poco validado o con glosario parcial, aprobado. Por debajo de 1.2 puntos el trabajo queda suspendido.
