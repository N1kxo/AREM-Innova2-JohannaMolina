# 📄 Informe Técnico del Taller

## 🔖 Nombre del Taller
_Taller 6 - Checklist de Cumplimiento Normativo_

## 👥 Integrantes del equipo
- Nicolas Esteban Muñoz Sendoya (nico9ms@hotmail.com, GitHub: N1kxo)
- Juan David Orozco Rodríguez (davidorozcoj1@gmail.com, GitHub: DavidOrozcoJ)

## 🧠 Descripción general del trabajo
El objetivo del taller fue verificar los aspectos legales, normativos y de cumplimiento aplicables al proceso de gestión de PQRSF ("Comuníquese con Nosotros") del Centro de Contacto de la Universidad de La Sabana, aplicando la misma metodología de 5 pasos usada en clase sobre el caso base GobData, pero ahora sobre el sistema real del cliente: Anexo Unisabana (plataforma de tickets) más el Excel Consolidado compartido en SharePoint, dentro de Microsoft 365.

## 🔧 Proceso de desarrollo
Se partió de los hallazgos ya documentados por el equipo en los Talleres 1 a 4 (BPMN, modelo de información, arquitectura C4 y mapa de infraestructura) para identificar qué datos maneja el proceso y qué brechas de control ya se habían detectado (falta de trazabilidad de ediciones en el Excel, servidor de Anexo Unisabana como instancia única, ausencia de alertas de SLA, y Alexander como único gestor de los tres canales manuales). Sobre esa base se aplicaron los 5 pasos de la guía: (1) identificación de datos y procesos sensibles, (2) construcción del checklist por categoría, (3) evaluación Cumple/Parcial con evidencia, (4) documentación del riesgo de cada Parcial en la hoja Brechas Identificadas, y (5) priorización con recomendación concreta. Se complementó con investigación puntual sobre la Política de Protección de Datos vigente de la Universidad (versión 5.0, aprobada por el Consejo Superior) y sobre el Decreto 1377 de 2013, que reglamenta la transferencia internacional de datos — relevante porque uno de los canales del proceso (WhatsApp Business) transmite datos a través de infraestructura de un tercero fuera de Colombia.

## 🧩 Análisis del modelo propuesto

**Datos y procesos sensibles identificados:**

| Dato / Proceso | Sensibilidad | Normativa aplicable |
|---|---|---|
| Cédula, nombre, correo, teléfono del solicitante | Dato personal | Ley 1581 de 2012 |
| Descripción del caso (puede incluir información disciplinaria o de salud) | Dato sensible | Ley 1581 (tratamiento reforzado) |
| Registro y edición de casos en el Excel Consolidado | Trazabilidad de gestión | ISO 27001 (control de accesos y logs) |
| Mensajes recibidos vía WhatsApp Business | Dato personal transmitido a un tercero (Meta) | Decreto 1377/2013 (transferencia internacional) |
| Escalamiento a Unidad Responsable/Facultad | Roles y acceso | ISO 27001 (roles y responsabilidades) |

**Checklist por categoría:** se agruparon 12 ítems en las mismas 6 categorías del caso base (Consentimiento, Seguridad ISO 27001, Protección de Datos, Prevención de Fugas, Retención, Roles y Responsabilidades), diligenciados en `entrega/checklist-cliente.xlsx`.

**Cómo representa las necesidades del cliente:** a diferencia de GobData, el checklist del cliente real refleja que la Universidad sí cuenta con una política institucional de protección de datos formal y con controles de seguridad heredados de Microsoft 365, pero que el proceso operativo específico de PQRSF (el Excel Consolidado y los canales manuales) no hereda automáticamente esos controles: 8 de los 12 ítems quedaron en ⚠️ Parcial, todos derivados de brechas ya identificadas en talleres anteriores más dos brechas nuevas encontradas en este taller (ausencia de procedimiento ARCO diferenciado, y falta de control sobre la transferencia internacional de datos vía WhatsApp).

**Supuestos tomados:**
- No se tuvo acceso directo a las Tablas de Retención Documental del Centro de Contacto ni a la configuración real de DLP/Purview de la Dirección de TI; los ítems 8, 9 y 10 se marcaron Parcial de forma conservadora ante la falta de evidencia, siguiendo el mismo criterio usado en el Taller 4 para los riesgos de infraestructura.
- Se asume que los datos de salud o disciplinarios eventualmente incluidos en la descripción de un caso PQRSF son tratados como dato sensible bajo Ley 1581, aunque el proceso no los distinga explícitamente de otros campos del caso.

## 📈 Diagrama final entregado
No aplica para este taller (el entregable es documental: checklist + informe + referencias). El insumo de motivación (brechas como `Constraint` ArchiMate) queda como base para el Taller 7.

## 📋 Tabla de actores, entidades o componentes

| Nombre del elemento | Tipo | Descripción | Responsable |
|---|---|---|---|
| Ciudadano/estudiante solicitante | Actor | Radica el caso PQRSF por correo, WhatsApp, llamada, sitio web o App | Cliente |
| Alexander (Gestor de Casos) | Actor | Registra y gestiona los casos en el Excel Consolidado y Anexo Unisabana | Cliente |
| Unidad Responsable / Facultad | Actor | Atiende casos escalados a Nivel 2/3 | Cliente |
| Anexo Unisabana | Componente de aplicación | Plataforma de tickets, no exporta todos los campos requeridos | Dirección de TI |
| Excel Consolidado (SharePoint) | Componente de datos | Fuente real de verdad del proceso, sin trazabilidad de ediciones | Dirección de TI / Centro de Contacto |
| WhatsApp Business Cloud | Componente externo (tercero) | Canal de recepción de PQRSF, procesa datos fuera de Colombia | Meta (tercero) |

## 🔍 Investigación complementaria
### Tema investigado:
Marco normativo colombiano de protección de datos personales aplicable a instituciones de educación superior, con énfasis en transferencia internacional de datos y en la política institucional vigente de la Universidad de La Sabana.

### Resumen:
La Universidad de La Sabana publica una Política de Protección de Datos (versión 5.0), aprobada por la Comisión de Asuntos Generales del Consejo Superior, en cumplimiento de la Ley 1581 de 2012 y sus decretos reglamentarios, con el fin de salvaguardar las garantías constitucionales del Artículo 15 sobre intimidad y el derecho a conocer, actualizar y rectificar la información personal recogida en sus bases de datos. La Universidad también está sujeta a inspección y vigilancia del Ministerio de Educación Nacional, lo que añade un marco sectorial adicional (educación superior) a la protección de datos general. Adicionalmente, la autorización de tratamiento que firman los estudiantes al matricularse cubre finalidades académicas, administrativas y de bienestar, pero no menciona explícitamente los canales de terceros (como WhatsApp Business) que hoy se usan para radicar PQRSF — de ahí la brecha detectada en transferencia internacional de datos bajo el Decreto 1377 de 2013.

Esto se relaciona directamente con el taller porque confirma que, aunque la Universidad cumple con la obligación general de tener una política de protección de datos, la aplicación concreta de esa política al proceso operativo de PQRSF (el Excel Consolidado, los logs de acceso, el canal de WhatsApp) es donde aparecen las brechas reales — exactamente el tipo de diferencia entre "cumplimiento en el papel" y "cumplimiento operativo" que la guía del taller advierte como error común a evitar.

## 📚 Referencias
Ver `entrega/referencias.md`.

---

_Este documento hace parte de la entrega del Taller 6 del curso AREM (Arquitectura Empresarial) - Universidad de La Sabana._
