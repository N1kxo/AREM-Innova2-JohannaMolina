# 📄 Informe Técnico del Taller

## 🔖 Nombre del Taller
_Taller 3 - Arquitectura Actual del Sistema con el Modelo C4 (Parte 2 — Aplicación al Cliente Real)_

## 👥 Integrantes del equipo
- Nicolas Esteban Muñoz Sendoya (nico9ms@hotmail.com / N1kxo)
- Juan David Orozco Rodriguez (davidorozcoj1@gmail.com / DavidOrozcoJ)

## 🧠 Descripción general del trabajo
El objetivo del taller fue representar, mediante las vistas C1 (Contexto) y C2 (Contenedores) del modelo C4, la arquitectura actual del proceso de gestión de PQRs ("Comuníquese con Nosotros") del Centro de Contacto de la Universidad de La Sabana — el cliente real del proyecto, con Johanna Andrea Molina (Supervisora del Centro de Contacto) como persona de contacto. El ejercicio continúa el trabajo de los talleres previos (visión preliminar, BPMN y ERD/diagrama de contexto) sobre el mismo proceso, esta vez centrado en qué sistemas y herramientas lo soportan hoy.

## 🔧 Proceso de desarrollo
Se partió de la ficha de caracterización del cliente y de los modelos BPMN y ERD ya construidos en los talleres 1 y 2, que ya identificaban el "Sistema (Unísabana Anexo / Excel Online)" como el soporte actual del proceso. Se confirmó que el sistema no es una plataforma unificada: **Anexo Unisabana** es la plataforma de tickets institucional, pero no permite exportar todos los campos que el área necesita, por lo que el gestor de casos (Alexander) mantiene en paralelo una **hoja de cálculo en Excel** que es, en la práctica, la fuente de datos real del proceso. El sitio web institucional y la app Unisabana alimentan directamente a Anexo Unisabana; el correo, WhatsApp y las llamadas telefónicas, en cambio, no tienen integración alguna y dependen de que Alexander transcriba manualmente cada solicitud al Excel. Toda la solución debe operar dentro de Microsoft 365, por restricción explícita de la política de TI de la universidad — condición que se mantuvo como límite del modelo.

Con esa base se aplicó la metodología de 4 pasos de la guía del taller, primero para C1 y después para C2: (1) identificar actores, (2) identificar el sistema en alcance y los sistemas/canales externos, (3) trazar relaciones, y (4) etiquetar cada relación con el protocolo o mecanismo de comunicación y validar contra la checklist de autoevaluación. Los diagramas finales se construyeron en draw.io.

## 🧩 Análisis del modelo propuesto
- **Estructura del modelo:** el C1 muestra cuatro actores humanos (Solicitante, Alexander como Gestor de Casos, la Unidad Responsable/Facultad que atiende los casos de Nivel 2 y 3, y Johanna como Supervisora) y cuatro sistemas externos que corresponden a los canales y herramientas de terceros (Sitio Web/App Unisabana, Microsoft 365, WhatsApp, Central Telefónica). El C2 descompone el "Sistema de Gestión de PQRs y Tickets" en Anexo Unisabana, el Consolidado de Casos en Excel y tres contenedores de registro manual (correo, WhatsApp, llamadas), más el módulo de Reportes Semestrales.
- **Representación de las necesidades del cliente:** el modelo hace visibles los cuatro problemas de la ficha de caracterización. Anexo Unisabana aparece limitado a exportar solo campos básicos, lo que explica por qué existe el Excel paralelo (Problema #3); ese Excel se edita directamente por Alexander sin ningún control de versiones, lo que evidencia la falta de trazabilidad exigida por ISO 9001 (Problema #1); el módulo de Reportes Semestrales depende de tablas dinámicas construidas a mano (Problema #2); y ninguna relación del modelo incluye una alerta automática de vencimiento de SLA (Problema #4).
- **Diferencias con el caso base (RedExpress):** a diferencia de RedExpress, donde el sistema en alcance es una plataforma propia con contenedores desarrollados a la medida (app móvil, motor de rutas, etc.), aquí el "sistema en alcance" es en realidad la combinación de una plataforma institucional de terceros (Anexo Unisabana) y herramientas ofimáticas de Microsoft 365, sin desarrollo de software propio — una arquitectura mucho más manual y fragmentada.
- **Supuestos tomados:** se asumió que el sitio web y la app Unisabana entregan el caso a Anexo Unisabana de forma automática (por ser la misma plataforma institucional), y que el correo, WhatsApp y las llamadas no tienen ninguna integración con el Excel, dependiendo enteramente de la transcripción manual de Alexander — consistente con lo descrito en la ficha de caracterización y en los talleres 1 y 2.

## 📈 Diagrama final entregado
- `entrega/c1-contexto-final.drawio` — Vista de Contexto (C1)
- `entrega/c2-contenedores-final.drawio` — Vista de Contenedores (C2)

## 📋 Tabla de actores, entidades o componentes

| Nombre del elemento | Tipo | Descripción | Responsable |
|---|---|---|---|
| Solicitante | Actor | Estudiante, profesor, administrativo o público externo que radica un caso | Cliente |
| Alexander (Gestor de Casos) | Actor | Recibe, clasifica, registra y responde los casos por los distintos canales | Cliente |
| Unidad Responsable / Facultad | Actor | Atiende y responde de fondo los casos de Nivel 2 y Nivel 3 | Cliente |
| Johanna (Supervisora del Centro de Contacto) | Actor | Supervisa el cumplimiento del SLA y elabora los reportes semestrales | Cliente |
| Sistema de Gestión de PQRs y Tickets "Comuníquese con Nosotros" | Sistema en alcance | Anexo Unisabana + Excel; soporta el proceso de gestión de PQRs | Cliente |
| Sitio Web Institucional / App Unisabana | Sistema externo | Canal de radicación en línea, integrado con Anexo Unisabana | Universidad |
| Microsoft 365 (Teams / Correo) | Sistema externo | Canal de comunicación con el solicitante; único software externo permitido por política de TI | Universidad |
| WhatsApp | Sistema externo | Canal de mensajería para radicar o consultar casos | Meta / proveedor |
| Central Telefónica | Sistema externo | Canal de atención telefónica | Universidad |
| Anexo Unisabana | Contenedor | Plataforma de tickets institucional; no exporta todos los campos que el área necesita | Universidad / Dirección de Tecnología |
| Consolidado de Casos | Infraestructura | Excel/Excel Online (Microsoft 365); repositorio real del proceso, sin control de versiones ni auditoría | Cliente |
| Registro de Correo | Contenedor | Bandeja de Outlook compartida; registro manual de casos recibidos por correo | Cliente |
| Registro de Llamadas | Contenedor | Planilla de transcripción manual de las solicitudes telefónicas | Cliente |
| Registro de WhatsApp | Contenedor | Transcripción manual de los mensajes recibidos por WhatsApp | Cliente |
| Reportes Semestrales | Contenedor | Tablas dinámicas de Excel; generación manual de los informes "Ayúdanos a mejorar" e "Infórmate aquí" | Cliente |

## 🔍 Investigación complementaria
### Tema investigado:
Buenas prácticas para modelar arquitecturas orientadas a servicio con C4, y requisitos de trazabilidad y gestión de quejas bajo ISO 9001/ISO 10002.

### Resumen:
La literatura sobre el modelo C4 destaca su estructura en cuatro niveles (Contexto, Contenedores, Componentes y Código) como una forma de comunicar la arquitectura de un sistema tanto a audiencias técnicas como no técnicas, y documenta su aplicación a casos reales para mostrar cómo facilita el mantenimiento y la toma de decisiones de diseño. Este enfoque es el que se siguió en el taller: el C1 comunica el "qué" del proceso a nivel de negocio, mientras que el C2 expone las decisiones tecnológicas (o, en este caso, la ausencia de ellas) detrás de cada canal — mostrando, por ejemplo, por qué Anexo Unisabana convive con un Excel paralelo en lugar de ser la única fuente de datos.

Por el lado de la gestión de quejas, ISO 9001:2015 vincula la atención de quejas de clientes con la gestión de no conformidades y las acciones correctivas (cláusula 9.1.2, ya revisada en el Taller 2 bajo la cláusula 8.5.2 de trazabilidad), y el estándar complementario ISO 10002:2018 recomienda que el proceso sea accesible y esté orientado a la mejora continua. Fuentes especializadas en implementación de ISO 9001 señalan que depender de un sistema basado en hojas de cálculo es una causa frecuente de documentación inadecuada frente a un auditor, y recomiendan un repositorio central con control de cambios — justamente el vacío que el diagrama C2 evidencia: un repositorio central (el Excel) que sí existe, pero sin control de versiones ni trazabilidad.

## 📚 Referencias
- [1] Kitsios, F. et al. *C4 Model: A Research Guide for Designing Software Architectures*. Zenodo, 2025. https://zenodo.org/records/15090175
- [2] Brown, S. *The C4 Model for Visualising Software Architecture*. https://c4model.com/
- [3] SimplerQMS. "Complaint Management: Definition, Requirements, Process, and Software" (ISO 9001:2015 cláusula 9.1.2 e ISO 10002:2018). https://simplerqms.com/complaint-management
- [4] isoTracker. "How to Set Up a Complaints Management System — ISO 9001 Requirements". https://www.isotracker.com/blog/how-to-set-up-a-complaints-management-system-iso-9001-requirements/
- Fuente asistida por IA: Claude (Anthropic), septiembre 2026 — apoyo en la estructuración de los diagramas C1/C2 y la redacción de este informe a partir de la ficha de caracterización del cliente y los talleres 1 y 2 del equipo.

---

_Este documento hace parte de la entrega del Taller 3 del curso AREM (Arquitectura Empresarial) - Universidad de La Sabana._
