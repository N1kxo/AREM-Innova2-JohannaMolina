# 📄 Informe Técnico del Taller

## 🔖 Nombre del Taller
_Taller 1 - Modelado de Proceso del Cliente con BPMN_

## 👥 Integrantes del equipo
- Nicolas Esteban Muñoz Sendoya (nico9ms@hotmail.com / N1kxo)
- Juan David Orozco Rodriguez (davidorozcoj1@gmail.com / DavidOrozcoJ)

## 🧠 Descripción general del trabajo
El objetivo de esta entrega fue modelar en BPMN un proceso real del cliente asignado: la **Dirección de Servicio al Cliente / Contact Center de la Universidad de La Sabana**, específicamente el proceso de **gestión y seguimiento de casos PQRSF ("Comuníquese con Nosotros")**. La información se levantó en una reunión con la responsable del área y se complementó con el análisis presentado en clase ("Arquitectura Empresarial para el Contact Center: Automatización, Trazabilidad y Control"), que identificó tres problemas centrales: falta de trazabilidad de cambios, ausencia de alertas de vencimiento y generación manual de reportes.

## 🔧 Proceso de desarrollo
Se siguió la metodología de 5 pasos de la guía del taller, ajustada tras la retroalimentación de clase para alinear los carriles y la lógica de enrutamiento con el análisis presentado:

1. **Actores (carriles)**: se ajustaron a cuatro carriles — **Solicitante**, **Sistema** (Unísabana Anexo / Excel Online + Power Automate), **Gestor de Casos** (Alexander) y **Área / Facultad** — nomenclatura alineada con la presentada en clase, en vez de nombrar los carriles con nombres propios sueltos.
2. **Inicio y fin**: el proceso inicia cuando el usuario radica un caso por cualquiera de los canales disponibles, y termina en el evento "Caso cerrado", después de que el sistema registra automáticamente el cambio en el historial de auditoría.
3. **Actividades**: revisar casos nuevos, clasificar el caso, resolver directamente o redirigir según el nivel, atender y responder, registrar la respuesta y actualizar el estado.
4. **Gateways**: se reforzó el punto de enrutamiento por nivel de reporte como una **compuerta XOR explícita** de tres salidas mutuamente excluyentes (Nivel 1, Nivel 2, Nivel 3) — antes este cálculo estaba implícito dentro de una tarea del sistema; ahora es una decisión de negocio visible en el modelo, como se explicó en la sesión de clase ("un ticket solo puede pertenecer a un nivel a la vez").
5. **Conexión y validación**: se trazaron los flujos, se etiquetó cada salida de gateway y se revalidó contra la checklist de la guía (un solo evento de inicio, ningún camino sin evento de fin, verbos de acción, sin elementos flotantes).

## 🧩 Análisis del modelo propuesto
El modelo conserva la estructura de carriles pero incorpora tres ajustes derivados de la discusión en clase:

- **Compuerta XOR de enrutamiento por nivel**: después de clasificar el caso, una compuerta exclusiva decide entre **Nivel 1 (2 días hábiles, resuelto directamente por el Gestor)** y **Nivel 2 / Nivel 3 (4 o 15 días hábiles, que exigen redirección a la Facultad o al Área Legal)**. Esto reemplaza el cálculo de plazo "silencioso" del modelo anterior por una decisión explícita y visible, consistente con la lógica de enrutamiento que se explicó en clase.
- **Punto crítico resaltado**: la tarea "Actualizar estado a Cerrado en Excel" se marcó con un color distinto (ámbar) porque es, según el diagnóstico presentado en clase, el paso donde hoy se pierde la trazabilidad — al sobrescribir el Excel manualmente no queda registro de quién hizo el cambio ni cuándo.
- **Registro automático de auditoría**: inmediatamente después de ese punto crítico se agregó la tarea "Registrar automáticamente en Historial de Auditoría (quién, cuándo, qué cambió)", en el carril de Sistema, representando la solución arquitectónica (Power Automate) que resuelve el punto crítico sin modificar el Anexo Unísabana ni introducir software de terceros.

Se mantiene el evento límite (timer) no interruptivo sobre la tarea de atención en Facultad, que dispara la alerta automática por Teams/correo — este es el segundo pilar de la solución (alertas tempranas) y no cambió respecto a la versión anterior.

**Supuestos tomados:**
- Se asume que el Nivel 1 no requiere redirección a Facultad y es resuelto directamente por el Gestor, mientras que los Niveles 2 y 3 sí, tal como lo describe la cliente y lo resume la línea de tiempo presentada en clase.
- Se asume que el registro en el Historial de Auditoría ocurre de forma automática (vía Power Automate) inmediatamente después de cualquier actualización de estado, sin intervención manual del Gestor.
- Se mantiene un único evento de fin conceptual ("Caso cerrado"), alcanzado después de pasar por el punto crítico y su registro de auditoría, para no fragmentar el diagrama.

## 📈 Diagrama final entregado
Ver [`modelo-final.drawio`](modelo-final.drawio).

## 📋 Tabla de actores, entidades o componentes

| Nombre del elemento | Tipo | Descripción | Responsable |
|---|---|---|---|
| Solicitante | Actor | Estudiante, profesor, administrativo o público externo que radica el caso | Cliente |
| Gestor de Casos (Alexander) | Actor | Revisa, clasifica, enruta y da trazabilidad a cada caso | Cliente |
| Área / Facultad | Actor | Atiende y responde de fondo los casos de Nivel 2 y Nivel 3 | Cliente |
| Unísabana Anexo / Excel Online + Power Automate | Sistema | Herramienta de tickets y repositorio donde se calculan plazos, se enrutan casos y se generan alertas y auditoría | Cliente / Dirección de Tecnología |
| Compuerta XOR "¿Qué nivel de reporte aplica?" | Gateway | Enruta el caso de forma excluyente a Nivel 1, 2 o 3 según su gravedad | Sistema propuesto |
| Actualizar estado a Cerrado en Excel | Tarea (punto crítico) | Paso donde hoy se pierde la trazabilidad si no se automatiza | Cliente (hoy manual) |
| Historial de Auditoría | Tarea automática | Registra quién, cuándo y qué campo se modificó en cada caso | Sistema propuesto (Power Automate) |

## 🔍 Investigación complementaria
### Tema investigado:
Buenas prácticas de modelado BPMN, uso de compuertas exclusivas (XOR) para lógica de enrutamiento, y su relación con los requisitos de trazabilidad de un sistema de gestión de calidad ISO 9001.

### Resumen:
La investigación confirmó que el uso de una compuerta XOR es la forma estándar de modelar decisiones mutuamente excluyentes como la asignación de nivel de reporte, evitando ambigüedad sobre si un caso podría clasificarse en más de un nivel a la vez. Fuentes como Trisotech y ProcessMaker coinciden en mantener el flujo predominantemente horizontal con un único pool y carriles por actor, criterio que se mantuvo al reorganizar los carriles con la nomenclatura discutida en clase (Solicitante, Sistema, Gestor, Facultad).

La revisión de la cláusula 8.5.2 de ISO 9001 (identificación y trazabilidad) sigue sustentando por qué el "punto crítico" del cierre manual en Excel necesita un paso de auditoría automática inmediatamente después: los registros manuales dificultan mantener una cadena de auditoría íntegra, y la buena práctica es que el sistema capture el cambio de forma automática en el mismo momento en que ocurre, no como una tarea separada que dependa de que alguien la recuerde.

## 📚 Referencias
- [1] Trisotech. *BPMN Modeling Best Practices*. https://www.trisotech.com/bpmn-modeling-best-practices/
- [2] ProcessMaker / Decisions. *Process Modeling Best Practices*. https://docs.processmaker.com/docs/process-modeling-best-practices
- [3] Qflow. *BPMN Implementation Best Practices*. https://qflowbpm.com/bpmn-best-practices-2/
- [4] ProcessMind. *BPMN 2.0 Modeling Tips & Best Practices*. https://processmind.com/resources/docs/best-practices/bpmn-modeling-tips
- [5] Qualityze. *ISO 9001 Clause 8.5.2 — Identification & Traceability*. https://www.qualityze.com/blogs/iso-9001-clause-8-5-2-identification-traceability
- [6] Core Business Solutions. *Clause 8.5.2 ISO 9001:2015 Explained*. https://www.thecoresolution.com/clause-8-5-2-iso-9001-2015-explained
- [7] OMG. *Especificación oficial BPMN*. https://www.omg.org/spec/BPMN/
- Fuente asistida por IA: Claude (Anthropic), agosto 2026 — apoyo en la reestructuración del modelo BPMN según la retroalimentación de clase y la redacción de este informe.

---

_Este documento hace parte de la entrega del taller 1 del curso AREM (Arquitectura Empresarial) - Universidad de La Sabana._
