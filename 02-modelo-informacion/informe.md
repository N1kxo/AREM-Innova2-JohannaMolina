# 📄 Informe Técnico del Taller

## 🔖 Nombre del Taller
_Taller 2 - Modelo de Información y Diagrama de Contexto_

## 👥 Integrantes del equipo
- Nicolas Esteban Muñoz Sendoya (nico9ms@hotmail.com / N1kxo)
- Juan David Orozco Rodriguez (davidorozcoj1@gmail.com / DavidOrozcoJ)

## 🧠 Descripción general del trabajo
El objetivo de esta entrega fue construir el modelo entidad-relación (ERD) y el diagrama de contexto del dominio de información del cliente real: la **Dirección de Servicio al Cliente / Contact Center de la Universidad de La Sabana**, sobre el proceso de gestión de casos PQRSF ("Comuníquese con Nosotros"). Ambos diagramas se construyeron a partir de los campos y actores que la responsable del área describió en la reunión de levantamiento, tomando como entidad central el Excel que hoy funciona como base de datos del proceso.

## 🔧 Proceso de desarrollo
Se aplicaron las dos metodologías de 4 pasos de la guía del taller:

**ERD:**
1. Se identificaron las entidades directamente a partir de las columnas del Excel descritas por la cliente (Caso, Usuario, Categoría, Tema, Subtema, NivelReporte, UnidadResponsable) y se sumaron dos entidades nuevas —HistorialCambios y Reapertura— que no existen hoy pero que resuelven los dos problemas que la cliente priorizó.
2. Se definieron atributos y clave primaria de cada entidad.
3. Se trazaron las relaciones con su verbo (radica, clasifica, atiende, genera, etc.).
4. Se asignó la cardinalidad en ambos extremos de cada relación (todas 1:N) y se verificó que ninguna entidad quedara desconectada.

**Diagrama de contexto:**
1. Se identificó al Usuario/Solicitante como el único actor externo al límite organizacional, y a Microsoft 365 (Teams/Correo) como sistema de terceros.
2. Se identificaron los sistemas y actores internos: Alexander, la Unidad Responsable, la Jefatura de Calidad, Unísabana Anexo, el sistema propuesto de gestión de casos y el módulo de reportes.
3. Se trazaron los flujos de información entre todos los elementos.
4. Se etiquetó cada flujo con la información que transporta y se dibujó el límite organizacional para separar lo interno de lo externo, marcando el sistema de terceros con borde punteado.

Ambos diagramas se generaron de forma programática en Python como archivos `.drawio` y se validaron como XML bien formado.

## 🧩 Análisis del modelo propuesto
El ERD traduce en estructura de datos los dos problemas que la cliente priorizó: la entidad **HistorialCambios** resuelve la falta de trazabilidad (quién modificó qué campo y cuándo, exigida por el sistema de calidad ISO 9001), y la entidad **Reapertura** modela explícitamente el ciclo de vida real de un caso, que puede regresar a la unidad responsable si el usuario no queda conforme con la respuesta.

El diagrama de contexto, por su parte, hace explícito un límite organizacional que agrupa a Alexander, la Unidad Responsable, la Jefatura de Calidad y los sistemas internos, dejando fuera únicamente al Usuario/Solicitante (actor externo) y a Microsoft 365 (sistema de terceros, marcado con borde punteado) — esta distinción retoma un detalle que la cliente mencionó en la reunión: la universidad restringe el uso de aplicaciones no institucionales y solo permite trabajar dentro del conjunto Microsoft, por lo que cualquier módulo de notificaciones debía apoyarse en Teams/Outlook y no en una herramienta externa nueva.

**Supuestos tomados:**
- Se asume que todas las relaciones del ERD son 1:N; no se identificaron relaciones N:N en la descripción de la cliente, por lo que no fue necesario introducir entidades asociativas adicionales.
- Se asume que Alexander, la Unidad Responsable y la Jefatura de Calidad son actores internos a la organización (aunque externos al software), siguiendo el mismo criterio que el caso base usó para clasificar al Asistente Administrativo.
- El módulo de Reportes se modeló como un sistema interno que consume los datos del sistema central, sin detallar su lógica interna de generación, ya que a ese nivel de detalle corresponde a un diagrama de componentes fuera del alcance de este taller.

## 📈 Diagrama final entregado
Ver [`modelo-final-er.drawio`](modelo-final-er.drawio) y [`diagrama-contexto-final.drawio`](diagrama-contexto-final.drawio).

## 📋 Tabla de actores, entidades o componentes

| Nombre del elemento | Tipo | Descripción | Responsable |
|---|---|---|---|
| Caso PQRSF | Entidad | Ticket radicado por un usuario, con su clasificación, plazos y estado | Cliente |
| HistorialCambios | Entidad (nueva) | Registra cada modificación de un caso: campo, valor anterior, valor nuevo, usuario y fecha/hora | Sistema propuesto |
| Reapertura | Entidad (nueva) | Registra motivo, fecha y responsable de cada reapertura de un caso | Sistema propuesto |
| Usuario / Solicitante | Actor (externo) | Radica el caso y recibe la respuesta | Cliente |
| Microsoft 365 (Teams / Correo) | Sistema de terceros | Canal por el que se envían las alertas automáticas de vencimiento | Microsoft / Dirección de Tecnología |

## 🔍 Investigación complementaria
### Tema investigado:
Buenas prácticas para la construcción de modelos entidad-relación y diagramas de contexto, y su relación con los requisitos de trazabilidad de ISO 9001.

### Resumen:
La investigación sobre ERD confirmó dos principios aplicados en el modelo: nombrar entidades con sustantivos y relaciones con verbos consistentes con el dominio (Lucidchart, LinkedIn), y aplicar normalización para evitar campos de texto libre redundantes — por eso Categoría, Tema y Subtema se modelaron como tres entidades encadenadas y no como columnas de texto repetidas dentro de Caso, tal como el Excel actual de la cliente ya las maneja como listas desplegables independientes.

Sobre el diagrama de contexto, Mural señala que su valor está en mantenerse a nivel de "caja negra": mostrar el sistema, las entidades externas y los flujos de datos, sin entrar en el detalle interno del sistema. Este criterio se aplicó al representar el Módulo de Notificaciones y el Módulo de Reportes como sistemas separados sin detallar su lógica interna. Finalmente, la revisión de la cláusula 8.5.2 de ISO 9001 reforzó por qué el HistorialCambios debe alimentar directamente los reportes hacia la Jefatura de Calidad: los estándares de trazabilidad exigen que los registros de cambio sean recuperables y verificables ante una auditoría, no solo almacenados.

## 📚 Referencias
- [1] Lucidchart. *What is an entity relationship diagram (ERD)?*. https://www.lucidchart.com/pages/er-diagrams
- [2] LinkedIn Collaborative Article. *What are the best practices for creating entity-relationship diagrams?*. https://www.linkedin.com/advice/0/what-best-practices-creating-entity-relationship-69sle
- [3] Visual Paradigm. *What is Entity Relationship Diagram (ERD)?*. https://www.visual-paradigm.com/guide/data-modeling/what-is-entity-relationship-diagram/
- [4] Mural. *Context diagrams: Guide and best practices*. https://www.mural.co/blog/context-diagrams
- [5] Qualityze. *ISO 9001 Clause 8.5.2 — Identification & Traceability*. https://www.qualityze.com/blogs/iso-9001-clause-8-5-2-identification-traceability
- [6] Pretesh Biswas. *ISO 9001:2015 Clause 8.5.2 Identification and traceability*. https://preteshbiswas.com/2023/09/06/iso-90012015-clause-8-5-2-identification-and-traceability/
- Fuente asistida por IA: Claude (Anthropic), agosto 2026 — apoyo en la estructuración del ERD, el diagrama de contexto y la redacción de este informe a partir de la transcripción de la reunión con la cliente.

---

_Este documento hace parte de la entrega del Taller 2 del curso AREM (Arquitectura Empresarial) - Universidad de La Sabana._
