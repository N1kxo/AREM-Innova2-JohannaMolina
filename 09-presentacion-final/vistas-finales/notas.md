# Notas sobre las vistas ArchiMate

## Vista de Motivación (`archimate-motivacion.drawio` / `.png`)
Los `Driver` y `Goal` salen de la Ficha de Caracterización y de la tabla "Beneficios esperados" del Taller 0 (`00-preliminary-vision/vision.md`). Los `Principle` son los 5 de `plan-gobernanza.md`, sección Principios Arquitectónicos (aquí se muestran los 3 que conectan directamente con los Goals, para no saturar el tablero). Los `Requirement` salen de las brechas de los Talleres 5-7.

Una nota de transparencia: el `Goal` "agilizar reportes de gestión" (Taller 0) no tenía, en el Taller 7, un Work Package dedicado entre WP1-WP3. Se formuló el `Requirement` "reportes semestrales desde vistas de SharePoint" como una extensión razonable de WP1 (una vez el Consolidado de Casos vive en una Lista de SharePoint estructurada, construir vistas filtradas para los reportes semestrales es una consecuencia directa de esa migración, no una funcionalidad nueva inventada), y se deja explícito aquí para que el panel pueda cuestionarlo si lo considera necesario.

## Vista de Implementación y Migración (`archimate-migracion.drawio` / `.png`)
Cada `Work Package` es una fase real de `plan-implementacion.md` (WP1/WP2/WP3 del Taller 7), asociado al `Gap` de `07-opportunities-solutions/matriz-brechas.xlsx` que cierra, y realiza el `Plateau` TO-BE del Taller 7.

Un Gap de la matriz de brechas del Taller 7 **no** queda cerrado por ningún Work Package técnico, y se muestra así deliberadamente, sin forzar una asociación que no existe:
- **Gap #9** (roles no documentados / cuello de botella en el gestor de casos): se gestiona con una acción documental (manual de respaldo del rol, fase Quick win de `plan-implementacion.md`), no con un Work Package técnico — igual que el riesgo R1 de `matriz-riesgos.xlsx`.

El Gap #7 (transferencia internacional vía WhatsApp Business) sí lo cierra WP3, que en el Taller 7 incluye la evaluación de esa transferencia junto con el procedimiento ARCO y la TRD (riesgo R8 en `matriz-riesgos.xlsx`).
