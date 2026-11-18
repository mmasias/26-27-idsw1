# Organización del repositorio (1.0 pts)

Se valora que el trabajo sea navegable y auditable. No se prescribe una estructura única, pero debe seguir principios de claridad y trazabilidad similares a los ejemplos vistos en clase.

## Qué

- Un `README.md` raíz que explique la estructura elegida y dónde está cada cosa, con enlaces a las carpetas clave.
- Tipos de artefactos en ubicaciones distintas (`/minutas/`, `/modelos/`, `/casos-de-uso/`, `/evidencias/`).
- Una convención de nombres clara y mantenida.
- Diagramas `.puml` con su exportación `.svg` o `.png`.
- Las versiones del modelo del dominio y de los casos de uso localizables, y las minutas accesibles por orden cronológico.

## Cómo

Lo que importa es que un evaluador ajeno al equipo encuentre los artefactos clave (modelo final, casos de uso priorizados, minutas) sin buscar. La estructura es libre mientras el README explique dónde está cada cosa. Nombres como `documento1`, `final` o `nuevo` no ayudan a nadie, y varias versiones sin convención ("final", "final2", "finalFINAL") tampoco.

Con un README que refleja la estructura real, carpetas ordenadas y nombres consistentes hay buena nota. Con una estructura razonable pero con README básico o algunos archivos fuera de sitio, aprobado.

## Para organizar bien

No puntúa, pero ayuda: README con índice navegable, un README breve en cada carpeta principal, nombres como `CU-03-registrar-reserva` o `minuta-2026-11-02`, un `.gitignore` apropiado y no dejar archivos temporales (`.DS_Store`, `~`, `.tmp`).

## Innegociable

Todos los artefactos deben ser navegables a partir del README.md de la raíz. De lo contrario el apartado puntúa 0.