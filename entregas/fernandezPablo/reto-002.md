# La confirmación es una decisión de diseño de interacción (UX), no un requisito del dominio. Además, es inconsistente con el resto de casos de uso:
- `moverPieza()` → acción irreversible (no se puede deshacer un movimiento) → **sin confirmación**
- `rotarPieza()` → acción irreversible → **sin confirmación**
- `reiniciarPartida()` → acción reversible (se puede volver a jugar) → **con confirmación**
La asimetría es difícil de justificar: se pide confirmación para la acción menos dañina y no se pide para las más irreversibles. Si se mantiene la confirmación, el documento debería argumentar explícitamente por qué solo este caso de uso la necesita.
---
### Decisión discutible 4 — Prototipar con una única imagen sin leyenda de teclas
**¿Qué se decidió?**
El prototipo de todos los casos de uso consiste en **una única imagen** (`images/tetris.png`) mostrando el estado `EN_CURSO`, con la justificación de que *"todo está implícito y claro con esa única imagen"* (`docs/ProcesoRequisitos.md`, línea 185).
**¿Qué propongo?**
Añadir al menos una leyenda que asocie cada tecla visible en la imagen con su caso de uso correspondiente.
**Argumento:**
La imagen muestra el texto "teclas 4, 6, 7, 9" pero no especifica qué hace cada una. Para que un prototipo valide un caso de uso, debe permitir verificar que el sistema responde correctamente a la acción del actor. Sin saber qué tecla dispara `moverPieza()`, `rotarPieza()` o `alternarVelocidadCaida()`, no es posible trazar los casos de uso al prototipo. La afirmación "todo está claro" es una suposición, no una validación. Una leyenda mínima cumpliría el objetivo sin incurrir en "documentar por documentar" — precisamente el criterio que el propio documento invoca para justificar la decisión.
---
## Resumen ejecutivo
| # | Tipo | Artefactos involucrados | Núcleo del problema |
|---|---|---|---|
| **I1** |  Incoherencia | `iniciarPartida.puml` ↔ `DiagramaCasosUso.puml` | Nombre de caso de uso diferente: `acelerarCaida` vs `alternarVelocidadCaida` |
| **I2** |  Incoherencia | `DiagramaDeContexto000.puml` ↔ diagramas de actor | Transición `Finalizado→[*]` sin caso de uso que la respalde |
| **I3** |  Incoherencia | `reiniciarPartida.puml` ↔ nombre del caso de uso | "Reiniciar" lleva a `INACTIVO`, no a `EN_CURSO` |
| **I4** |  Incoherencia | `ProcesoRequisitos.md` ↔ `DiagramaCasosUso.puml` | Teclas del prototipo sin mapeo a casos de uso; posible acción del Jugador no modelada |
| **D1** |  Decisión discutible | `modeloDelDominio.md` | Jugador excluido del dominio pero incluido como actor en requisitos |
| **D2** |  Decisión discutible | `ModeloDominio.puml` | `Turno` no es vocabulario natural del dominio Tetris |
| **D3** |  Decisión discutible | `reiniciarPartida.puml` | Confirmación solo en reiniciar, no en acciones más irreversibles |
| **D4** |  Decisión discutible | `ProcesoRequisitos.md` | Una imagen sin leyenda no valida los casos de uso |