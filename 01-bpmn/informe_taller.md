# 📄 Informe Técnico del Taller

## 🔖 Nombre del Taller
_Taller 1 - Modelado de Proceso del Cliente con BPMN_

## 👥 Integrantes del equipo
- Nicolas Esteban Muñoz Sendoya (nico9ms@hotmail.com / N1kxo)
- Juan David Orozco Rodriguez (davidorozcoj1@gmail.com / DavidOrozcoJ)

## 🧠 Descripción general del trabajo
El objetivo de esta entrega fue modelar en BPMN un proceso real del cliente asignado: la **Dirección de Servicio al Cliente / Contact Center de la Universidad de La Sabana**, específicamente el proceso de **gestión y seguimiento de casos PQRSF ("Comuníquese con Nosotros")**. La información se levantó en una reunión con la responsable del área, quien describió cómo se reciben, clasifican, atienden y cierran los casos hoy en día, y cuáles son los problemas que enfrenta el equipo con su herramienta actual (una base de datos en Excel).

## 🔧 Proceso de desarrollo
Se siguió la metodología de 5 pasos de la guía del taller:

1. **Actores**: a partir de la transcripción de la reunión se identificaron cuatro participantes del proceso — el Usuario/Solicitante, el sistema (Unísabana Anexo y la base de datos), Alexander (Coordinador de Servicio, quien hoy hace todo el trabajo manual) y la Unidad Responsable a la que se asigna cada caso.
2. **Inicio y fin**: el proceso inicia cuando el usuario radica un caso por cualquiera de los canales disponibles (web, app, correo, WhatsApp o llamada), y termina cuando el caso queda cerrado, ya sea a tiempo o fuera de tiempo.
3. **Actividades**: se listaron las tareas que la cliente describió explícitamente en la reunión — revisar casos nuevos a diario, clasificar el caso, calcular la fecha límite según el nivel de reporte, asignar la unidad responsable, atender y responder, y registrar la solución.
4. **Gateways**: se identificaron tres puntos de decisión reales del proceso descrito por la cliente — si la información está completa, si el usuario queda de acuerdo con la respuesta (que puede generar una reapertura del caso) y si la respuesta se dio dentro del plazo.
5. **Conexión y validación**: se trazaron los flujos de secuencia, se etiquetaron las salidas de cada gateway y se verificó el modelo contra la checklist de autoevaluación de la guía (un solo evento de inicio, ningún camino sin evento de fin, actividades nombradas como verbos de acción, sin elementos flotantes).

Se usó Python para generar el archivo `.drawio` de forma programática (coordenadas y conexiones), y se validó abriéndolo como XML bien formado antes de la entrega.

## 🧩 Análisis del modelo propuesto
El modelo se organiza como un único pool con cuatro carriles, uno por actor, lo que deja explícita la responsabilidad de cada paso — esto responde directamente al primer problema que planteó la cliente: hoy no hay claridad sobre quién modifica qué información dentro del Excel compartido.

El punto más particular del modelo es el manejo del control de plazos: en vez de modelarlo como una tarea manual de "revisar vencimientos", se representó como un **evento límite (timer) no interruptivo** adosado a la actividad de atención de la unidad responsable, que dispara el envío de una alerta automática por Teams/correo y cierra en su propio evento de fin. Esto refleja fielmente lo que la cliente pidió en la reunión: una alerta que llegue de forma paralela y automática, sin depender de que alguien recuerde revisar el estado de los casos.

**Supuestos tomados:**
- Se asume que la nueva alerta automática reemplaza la revisión manual actual, pero no elimina el registro que hoy hace Alexander — solo lo complementa.
- Se asume un único evento de fin conceptual ("Caso cerrado"), alcanzado por dos caminos distintos (a tiempo / fuera de tiempo), para mantener el diagrama legible sin sacrificar la distinción que exige el indicador del 80%.
- No se modeló en detalle el subproceso de generación de reportes semestrales/anuales, porque la cliente lo describió como una salida del proceso y no como parte del flujo de atención de un caso individual; ese tema se retoma en el Taller 2 con el módulo de reportes del diagrama de contexto.

## 📈 Diagrama final entregado
Ver [`modelo-final.drawio`](modelo-final.drawio).

## 📋 Tabla de actores, entidades o componentes

| Nombre del elemento | Tipo | Descripción | Responsable |
|---|---|---|---|
| Usuario / Solicitante | Actor | Estudiante, profesor, administrativo o público externo que radica el caso | Cliente |
| Alexander (Coordinador de Servicio) | Actor | Revisa, clasifica, asigna y da trazabilidad a cada caso | Cliente |
| Unidad Responsable | Actor | Facultad o área que atiende y responde de fondo el caso asignado | Cliente |
| Unísabana Anexo / Base de datos | Sistema | Herramienta de tickets y repositorio donde se calculan fechas límite y se generan alertas | Cliente / Dirección de Tecnología |
| Evento límite "Plazo por vencer" | Evento (timer, no interruptivo) | Dispara la alerta automática cuando un caso está por vencer su plazo de respuesta | Sistema propuesto |

## 🔍 Investigación complementaria
### Tema investigado:
Buenas prácticas de modelado BPMN y su relación con los requisitos de trazabilidad de un sistema de gestión de calidad ISO 9001.

### Resumen:
La investigación confirmó dos criterios que guiaron directamente las decisiones de modelado. Primero, fuentes como Trisotech y ProcessMaker coinciden en mantener el flujo predominantemente horizontal, agrupado en un único pool con carriles por actor, y en evitar líneas cruzadas — por eso el modelo se organizó como un solo pool con cuatro carriles y un flujo de izquierda a derecha, en vez de fragmentarlo en varios diagramas.

Segundo, la revisión de la cláusula 8.5.2 de ISO 9001 (identificación y trazabilidad) mostró que los registros manuales en hojas de cálculo dificultan mantener una cadena de auditoría íntegra, y que la buena práctica es que el sistema asigne identificadores únicos y actualice el estado de forma automática en vez de depender de transcripción manual. Esto sustenta por qué el modelo no se limitó a digitalizar el proceso actual, sino que incorporó el evento de alerta automática como un elemento nuevo del flujo — es la traducción directa, en BPMN, del problema de trazabilidad que la cliente planteó como su prioridad.

## 📚 Referencias
- [1] Trisotech. *BPMN Modeling Best Practices*. https://www.trisotech.com/bpmn-modeling-best-practices/
- [2] ProcessMaker / Decisions. *Process Modeling Best Practices*. https://docs.processmaker.com/docs/process-modeling-best-practices
- [3] Qflow. *BPMN Implementation Best Practices*. https://qflowbpm.com/bpmn-best-practices-2/
- [4] ProcessMind. *BPMN 2.0 Modeling Tips & Best Practices*. https://processmind.com/resources/docs/best-practices/bpmn-modeling-tips
- [5] Qualityze. *ISO 9001 Clause 8.5.2 — Identification & Traceability*. https://www.qualityze.com/blogs/iso-9001-clause-8-5-2-identification-traceability
- [6] Core Business Solutions. *Clause 8.5.2 ISO 9001:2015 Explained*. https://www.thecoresolution.com/clause-8-5-2-iso-9001-2015-explained
- Fuente asistida por IA: Claude (Anthropic), agosto 2026 — apoyo en la estructuración del modelo BPMN y la redacción de este informe a partir de la transcripción de la reunión con la cliente.

---

_Este documento hace parte de la entrega del taller 1 del curso AREM (Arquitectura Empresarial) - Universidad de La Sabana._
