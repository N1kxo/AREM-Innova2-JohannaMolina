# Notas sobre las vistas ArchiMate

## Vista de Motivación (`archimate-motivacion.drawio` / `.png`)
Los `Driver` y `Goal` salen de la ficha de caracterización y de la tabla "Beneficios esperados" del Taller 0 (`00-preliminary-vision/vision.md`); el driver de cumplimiento usa la cifra corregida del Taller 6 (10 de 12 ítems en Parcial). Los `Principle` son los de `plan-gobernanza.md` (aquí se muestran los 3 que conectan directamente con los Goals, para no saturar la vista). Los `Requirement` salen de las brechas de los Talleres 5 a 7.

El diagrama se organizó en columnas para que cada cadena se lea de abajo hacia arriba sin cruzar otras:
- **Trazabilidad por diseño** realiza dos Goals: trazabilidad/auditabilidad y el indicador de oportunidad del 80%. Por ahí suben tanto el historial automático como las **alertas de SLA**: una alerta solo es confiable si sale de un registro fiable de estados y fechas. (En la versión anterior la línea de las alertas parecía subir por "Seguridad por diseño", y ese vínculo no se sostenía.)
- **Evolución institucional, no reemplazo** realiza solo el Goal de fortalecer la gestión en Microsoft 365, a través de la continuidad del servidor de Anexo. Ya no aparece conectado al Goal de reportes.
- El Goal "agilizar los reportes de gestión" lo realiza directamente el Requirement "reportes semestrales desde vistas de la Lista", sin pasar por un principio. Ese Requirement es una extensión de WP1: el Taller 7 no tenía un Work Package dedicado a reportes, pero una vez el Consolidado vive en una Lista estructurada, armar vistas filtradas para los reportes es consecuencia directa de la migración. Se deja explícito para que el panel pueda cuestionarlo.

## Vista de Implementación y Migración (`archimate-migracion.drawio` / `.png`)
Cada `Work Package` está al lado del `Gap` de `07-opportunities-solutions/matriz-brechas.xlsx` que cierra, y realiza el `Plateau` TO-BE del Taller 7. Todas las líneas tienen etiqueta (asociación, cierra, realiza) y ninguna cruza un Work Package.

**Por qué hay 3 Work Packages y 4 fases.** La guía pide una fila del plan por Work Package. Aquí WP1 (Lista de SharePoint + Power Automate) es un solo paquete de trabajo en el Taller 7, pero `plan-implementacion.md` lo ejecuta en dos fases para no bloquear el roadmap: el piloto (Mediano plazo) y el rollout con automatización (Largo plazo). WP2 corresponde a la fase Corto plazo y WP3 a la fase Quick win. La correspondencia es:

| Fase del plan | Work Package |
|---|---|
| Quick win | WP3 (+ acción documental para el Gap #9) |
| Corto plazo | WP2 |
| Mediano plazo | WP1, primera parte (piloto) |
| Largo plazo | WP1, segunda parte (rollout y Power Automate) |

**El Gap #9** (Gestor de Casos sin respaldo) no lo cierra ningún Work Package técnico del Taller 7: se ataca con una acción de personas y documentación en la fase Quick win (designar y entrenar un respaldo, documentar roles), igual que el riesgo R1 de `matriz-riesgos.xlsx`. Se muestra así a propósito, sin forzar una asociación con un WP que no existe.

El Gap #7 (transferencia internacional vía WhatsApp Business) sí lo cierra WP3, que incluye la evaluación de esa transferencia junto con el procedimiento ARCO y la TRD (riesgo R8).
