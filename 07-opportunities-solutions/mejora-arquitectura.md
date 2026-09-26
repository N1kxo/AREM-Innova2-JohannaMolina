# Mejora de Arquitectura (TO-BE) — Identificación y Priorización de Mejoras

## Cliente
Centro de Contacto, Universidad de La Sabana — proceso de gestión de PQRSF ("Comuníquese con Nosotros"). Contacto: Johanna Andrea Molina, Supervisora del Centro de Contacto.

## Integrantes del equipo
- Nicolas Esteban Muñoz Sendoya (nico9ms@hotmail.com — GitHub: N1kxo)
- Juan David Orozco Rodríguez (davidorozcoj1@gmail.com — GitHub: DavidOrozcoJ)

---

## 1. Diagnóstico inicial

**¿Qué procesos o tecnologías generan mayor fricción en la operación?** El Excel compartido (SharePoint), no Anexo Unisabana, es la fuente real de verdad del proceso — porque Anexo Unisabana no exporta todos los campos que el área necesita, el gestor de casos transcribe manualmente cada caso al Excel (Talleres 1-3). Esa doble captura es el origen de la mayoría de las brechas encontradas en los cuatro talleres siguientes.

**¿Qué problemas recurrentes señaló el cliente?** Según su ficha de caracterización: (1) no hay auditoría de quién edita el Excel ni cuándo, lo que impide certificar el proceso ante ISO 9001; (2) los reportes semestrales de gestión son lentos porque se arman a mano sobre el Excel; (3) Anexo Unisabana no exporta todos los campos requeridos; (4) no existen alertas tempranas cuando un caso se acerca a incumplir su SLA (meta del 80% a tiempo).

**¿Qué vulnerabilidades de seguridad o riesgos quedaron evidenciados en el análisis previo?**

| Vulnerabilidad / riesgo | Taller de origen |
|---|---|
| Servidor de Anexo Unisabana como instancia única on-premise, sin evidencia de plan de continuidad | Taller 4 |
| Excel compartido sin control de versiones | Taller 4 |
| El gestor de casos (Alexander) es un único punto de falla operativo para los 3 canales manuales | Taller 4 |
| T2/T3 (Tampering/Repudiation) — el Excel no registra quién ni cuándo modifica un caso | Taller 5 |
| T4 (Information Disclosure) — el Excel es el único repositorio de datos personales/sensibles del proceso | Taller 5 |
| T5 (Denial of Service) — ausencia de alertas tempranas de SLA | Taller 5 |
| T7 (Tampering) — la transcripción manual entre Anexo Unisabana y Excel introduce un punto de error/alteración | Taller 5 |
| 8 de 12 ítems del checklist normativo quedaron en ⚠️ Parcial (ARCO, BCP/DRP, logs, DLP, transferencia internacional WhatsApp, retención, anonimización, roles) | Taller 6 |

**Resumen del problema actual (foto del AS-IS):** el proceso de PQRSF opera hoy sobre dos sistemas desacoplados — Anexo Unisabana (ticketing oficial, incompleto) y un Excel compartido en SharePoint (la fuente real de datos) — sin trazabilidad de ediciones, sin plan de continuidad para el servidor de tickets, sin alertas de SLA y con ocho brechas de cumplimiento normativo abiertas. La mayoría de estas brechas comparten una misma raíz técnica: el Excel compartido no tiene control de acceso por rol, ni historial de cambios, ni controles de exportación.

---

## 2. Propuesta de mejoras

### 2.1 Lluvia de ideas (sin censura inicial)

| # | Idea de mejora | Tipo |
|---|---|---|
| 1 | Migrar el Consolidado de Excel a una Lista de SharePoint con historial de cambios y permisos por rol | Técnica |
| 2 | Automatizar la exportación completa de campos desde Anexo Unisabana (API/Power Automate), eliminando la doble transcripción manual | Técnica / Funcional |
| 3 | Sistema de alertas tempranas de SLA vía Power Automate (Teams/correo) antes del incumplimiento | Técnica / Proceso |
| 4 | Plan de continuidad y recuperación (BCP/DRP) documentado para el servidor de Anexo Unisabana | Técnica |
| 5 | Configurar políticas DLP de Microsoft Purview sobre la biblioteca del Consolidado | Técnica / Seguridad |
| 6 | Publicar y documentar un procedimiento ARCO específico dentro del flujo de PQRSF | Proceso / Cumplimiento |
| 7 | Evaluar la transferencia internacional de datos vía WhatsApp Business o migrar la recepción a un canal 100% M365 | Proceso / Cumplimiento |
| 8 | Definir Tabla de Retención Documental y proceso de anonimización específicos para PQRSF | Proceso / Cumplimiento |
| 9 | Documentar formalmente roles y permisos de acceso por Unidad Responsable/Facultad | Proceso |
| 10 | Encuesta corta de satisfacción al cerrar cada caso | Proceso / Comunicación |

### 2.2 Priorización (2-3 ideas seleccionadas, con justificación)

| Solución priorizada | Esfuerzo | Impacto | Quick win / Largo plazo | Justificación |
|---|---|---|---|---|
| Migrar el Consolidado a una Lista de SharePoint con historial y control de acceso por rol | Alto | Alto | Largo plazo | Cierra 4 brechas a la vez (auditoría ISO 9001, T2/T3/T4 de STRIDE, DLP del checklist, base para documentar roles); decidida mediante matriz de decisión ponderada frente a activar solo el historial nativo de Excel y frente a migrar a Dataverse/Power Apps |
| Plan de continuidad (BCP/DRP) para el servidor de Anexo Unisabana | Medio | Alto | Quick win | Único riesgo de disponibilidad total del canal formal de PQRSF (Taller 4); no depende de rediseñar el proceso, solo de documentar e implementar el procedimiento |
| Paquete de cumplimiento normativo (procedimiento ARCO + evaluación de transferencia WhatsApp + TRD/anonimización) | Bajo | Medio | Quick win | Las 3 ideas cierran brechas ya formalmente diagnosticadas en el Taller 6 (checklist); son de bajo esfuerzo porque son principalmente documentales, no requieren cambio de infraestructura |

Las ideas #2 (automatizar exportación de Anexo Unisabana), #3 (alertas de SLA) y #9 (documentar roles) quedan como backlog: son quick wins válidos y con hallazgo formal que las respalda, pero se priorizó primero la migración del Consolidado porque varias de ellas dependen de tener ya una fuente de datos con permisos y trazabilidad (por ejemplo, documentar roles de acceso solo tiene sentido una vez existe un sistema con control de acceso real que los aplique).

#### 2.1 Matriz de decisión ponderada — brecha "trazabilidad del Consolidado"

**Paso 1 — Problema en términos de impacto:** "El Excel compartido es la única fuente de verdad de los casos PQRSF, y ningún cambio a un caso queda con autor ni fecha: ante una auditoría interna ISO 9001 o un reclamo de un titular por Ley 1581 (Habeas Data), la Universidad no puede demostrar quién modificó un caso, cuándo, ni si hubo acceso indebido a datos sensibles (salud, disciplinarios) contenidos en él."

**Paso 2 — Último momento responsable:** el equipo no tuvo acceso a una fecha de auditoría ISO 9001 comunicada por el cliente — se deja como limitación explícita y como pregunta pendiente para Johanna Molina. Se recomienda decidir dentro de este semestre, para que el siguiente corte de auditoría ya cuente con trazabilidad.

**Paso 3 — Criterios y pesos** (pesos propuestos por el equipo a falta de validación directa con el negocio — pendiente de confirmar con la Dirección de TI y el Centro de Contacto):

| Criterio | Peso | Qué significa 5 | Qué significa 1 |
|---|---|---|---|
| Trazabilidad completa (autor + fecha + campo modificado) | 35% | Historial estructurado, exportable para auditoría | No identifica quién ni qué cambió |
| Costo (dentro de licenciamiento M365 ya contratado) | 25% | Sin costo adicional | Requiere licencia premium adicional |
| Complejidad / curva de aprendizaje para el gestor de casos | 20% | No cambia la herramienta de trabajo diario | Requiere aprender una herramienta nueva desde cero |
| Tiempo de implementación | 20% | Menos de 1 semana | Más de 10 semanas |

**Paso 4 — Opciones:**

| Opción | Descripción |
|---|---|
| A | Activar el historial de versiones nativo de Excel Online sobre el mismo archivo + notificación de cambios vía Power Automate |
| B | Migrar el Consolidado a una **Lista de SharePoint** (versionado nativo por elemento, permisos por columna, alertas automáticas) |
| C | Migrar a **Microsoft Dataverse/Power Apps** (formulario a medida, auditoría nativa, control de acceso granular) |

**Paso 5 — Consejo de quien sabe y a quien le afecta:** no se tuvo acceso directo a la Dirección de TI ni a Alexander (gestor de casos) para este ejercicio académico — se asume, como **supuesto explícito a confirmar**, que la licencia M365 ya contratada por la Universidad incluye SharePoint y Power Automate en su plan básico (evidencia indirecta: el equipo ya usa SharePoint y Teams hoy), pero que Dataverse/Power Apps premium probablemente requiere licenciamiento adicional no confirmado.

**Paso 6 — Puntuación:**

| Opción | Trazabilidad | Costo | Complejidad | Tiempo |
|---|---|---|---|---|
| A · Historial nativo Excel + notificación | **3** — registra versiones del archivo, pero no identifica con claridad qué campo cambió en una fila específica | **5** — sin costo adicional | **5** — no cambia la herramienta | **5** — configuración en días |
| B · Lista de SharePoint | **5** — historial nativo por elemento, con autor y fecha, exportable | **5** — incluido en M365 | **3** — requiere migrar el flujo de trabajo del gestor, capacitación breve | **3** — 4-6 semanas |
| C · Dataverse/Power Apps | **5** — auditoría nativa robusta, control de acceso granular | **2** — probable licencia adicional (supuesto) | **2** — diseño de app a medida, mayor curva | **2** — 8-10 semanas |

Totales ponderados: A = 3×0,35+5×0,25+5×0,20+5×0,20 = **4,30** · B = 5×0,35+5×0,25+3×0,20+3×0,20 = **4,20** · C = 5×0,35+2×0,25+2×0,20+2×0,20 = **3,05**.

**Cómo se lee este resultado:** A gana en el total, pero no cumple el objetivo del Paso 1 — una trazabilidad de 3 no basta para una auditoría ISO 9001 ni para responder a un titular bajo Ley 1581. Se aplica un **criterio eliminatorio: toda opción con Trazabilidad menor a 4 queda descartada, sin importar su total.** Con esa regla, A sale del juego y la decisión real es entre B (4,20) y C (3,05).

**Análisis de sensibilidad:** si el peso de Costo sube a 40% (redistribuyendo Trazabilidad 30%, Complejidad 15%, Tiempo 15%), B sigue ganando (4,40 vs. 2,90 de C). Si en cambio se ignorara el criterio eliminatorio y el peso de Trazabilidad bajara a 15% (Costo 40%, Complejidad 25%, Tiempo 20%), A volvería a ganar con 4,70 — pero el equipo no negocia el criterio eliminatorio de trazabilidad, porque no es una preferencia interna sino una exigencia legal (Ley 1581) y de calidad (ISO 9001).

**Paso 7 — Decisión:** implementar la migración del Consolidado a una **Lista de SharePoint** (opción B). **Trade-off aceptado:** se sacrifica algo de familiaridad inmediata del gestor con la interfaz de Excel (curva de aprendizaje breve) a cambio de trazabilidad completa sin costo adicional. **Alternativas descartadas:** A, por no cumplir el criterio eliminatorio de trazabilidad; C, por menor puntaje total y por licenciamiento adicional no confirmado.

**Paso 8 — Reevaluación:** revisar la decisión después de la primera auditoría ISO 9001 posterior a la migración, o antes si el volumen de casos crece al punto de que la Lista de SharePoint muestre limitaciones de rendimiento — en ese caso, reconsiderar la opción C.

**Uso de IA como copiloto:** se usó Claude (Anthropic) para generar el borrador de las 3 opciones y de los puntajes iniciales de la matriz; el equipo verificó cada puntaje contra las escalas del Paso 3 y ajustó la columna de Costo de la opción C (de "probable" a "supuesto explícito a confirmar con TI"), ya que no había evidencia directa del licenciamiento contratado por la Universidad.

---

## 3. Visualización TO-BE

### 3.1 Proceso mejorado
El registro de un caso PQRSF deja de depender de la doble transcripción manual (Anexo Unisabana → Excel): el gestor de casos registra una sola vez en la nueva **Lista de SharePoint - Consolidado PQRSF**, que ya incluye historial de cambios, permisos por rol y un flujo de Power Automate que dispara una alerta a Teams/correo cuando un caso se acerca a su límite de SLA, antes de incumplirlo.

### 3.2 Cambios en aplicaciones, infraestructura y flujos de información
El TO-BE de Aplicaciones extiende el C2 del Taller 3 reemplazando el Excel compartido por la **Lista de SharePoint - Consolidado PQRSF** (con historial y permisos por rol) y agregando un flujo de **Power Automate** para el historial de cambios y las alertas de SLA. El TO-BE de Tecnología extiende el mapa de infraestructura del Taller 4: el servidor de Anexo Unisabana pasa a contar con un **plan de continuidad documentado** (backup + procedimiento de recuperación — el alcance exacto, si incluye o no una segunda instancia física, queda pendiente de confirmar con la Dirección de TI según el presupuesto disponible), y el canal WhatsApp Business queda marcado como **pendiente de evaluación de transferencia internacional**. Ver diagramas completos en los anexos (`to-be-aplicaciones-final.drawio` y `to-be-tecnologia-final.drawio`).

### 3.3 Controles de seguridad integrados (Taller 5)
El TO-BE integra explícitamente mitigaciones para 4 de las 8 amenazas STRIDE del Taller 5:
- **T2/T3 (Tampering/Repudiation):** el historial nativo de la Lista de SharePoint registra autor y fecha de cada cambio, cerrando la brecha de repudio sobre el Consolidado.
- **T4 (Information Disclosure):** permisos por columna/rol en la Lista de SharePoint, en vez de un archivo Excel completo descargable por cualquiera con acceso a la carpeta compartida.
- **T7 (Tampering en la transcripción manual):** al concentrar el registro en un solo sistema, se elimina el paso de transcripción manual entre Anexo Unisabana y Excel, que era el punto de alteración accidental.
- **T5 (Denial of Service por falta de alertas):** el flujo de Power Automate de alertas tempranas de SLA ataca directamente esta amenaza.

Las 4 amenazas restantes del Taller 5 (Spoofing, Elevation of Privilege, y las variantes de Tampering/Information Disclosure no cubiertas arriba) no se resuelven con este TO-BE y quedan como trabajo futuro.

---

## 4. Análisis de beneficios y riesgos

| Mejora / Solución | Beneficio de negocio | Beneficio tecnológico/seguridad | Riesgo, limitación o dependencia de implementación |
|---|---|---|---|
| Lista de SharePoint con historial y permisos por rol | Reportes de gestión más rápidos (datos estructurados, no un Excel a mano); evidencia lista para auditoría ISO 9001 | Cierra T2/T3/T4 de STRIDE y 2 brechas del checklist normativo (logs, DLP) | Curva de aprendizaje para el gestor de casos; migración de la estructura y del histórico de casos ya registrados en el Excel actual |
| Plan de continuidad (BCP/DRP) para Anexo Unisabana | Continuidad del único canal formal de tickets ante una falla del servidor | Elimina el punto único de falla diagnosticado en el Taller 4 | Depende de la aprobación de presupuesto y de la disponibilidad de la Dirección de TI para diseñar e implementar el plan |
| Paquete de cumplimiento normativo (ARCO, transferencia WhatsApp, TRD/anonimización) | Reduce el riesgo legal/reputacional ante la Superintendencia de Industria y Comercio (SIC) | Cierra 3 de las 8 brechas del checklist del Taller 6 | Requiere coordinación con Gestión Documental y con el área jurídica de la Universidad, fuera del alcance directo del Centro de Contacto |

### 4.1 Capacidades de negocio y paquetes de trabajo

| Capacidad | Madurez AS-IS | Madurez TO-BE | Qué la explica |
|---|---|---|---|
| Registrar y clasificar solicitudes ciudadanas | 3 | 4 | Doble registro manual (Anexo Unisabana + Excel) hoy; un solo registro en la Lista de SharePoint en el TO-BE |
| Enrutar y escalar casos a la Unidad Responsable | 3 | 4 | Funciona, pero sin roles de acceso documentados (Taller 4/6) |
| Dar trazabilidad y auditar el ciclo de vida de un caso | 1 | 4 | Sin logs hoy (Talleres 4-6); historial nativo de la Lista de SharePoint en el TO-BE |
| Cumplir la normativa de protección de datos personales | 2 | 4 | 8 de 12 ítems Parcial en el checklist del Taller 6 |
| Generar reportes de gestión y cumplimiento de SLA | 2 | 3 | Reportes manuales hoy; datos estructurados en el TO-BE, aunque las alertas de SLA (idea #3) quedan en backlog |
| Garantizar la continuidad del servicio de atención | 2 | 4 | Servidor único sin BCP/DRP (Taller 4) |

**Paquetes de trabajo:**

| Paquete | Brechas que incluye | Capacidad que mejora | Tipo |
|---|---|---|---|
| WP1 · Trazabilidad y control de acceso del Consolidado | Migración a Lista de SharePoint (decisión de 2.1) | Dar trazabilidad y auditar (1→4), Cumplir normativa (2→4, parcial) | Largo plazo · 4-6 semanas |
| WP2 · Continuidad del servicio | Plan de continuidad para Anexo Unisabana | Garantizar continuidad (2→4) | Quick win · ~4-6 semanas |
| WP3 · Cumplimiento normativo documental | Procedimiento ARCO + evaluación WhatsApp + TRD/anonimización | Cumplir normativa (2→4, complemento de WP1) | Quick win · principalmente documental |

---

## Anexos
- Diagrama TO-BE de Aplicaciones: `entrega/to-be-aplicaciones-final.drawio`
- Diagrama TO-BE de Tecnología: `entrega/to-be-tecnologia-final.drawio`
- Matriz de brechas (Gap Analysis): `entrega/matriz-brechas.xlsx`

---

**Formato de entrega:** documento único de máximo 6 páginas + anexos, entregado como PDF con nombre `EquipoX_Mejora_Arquitectura.pdf`.

---

*Este documento hace parte de la entrega del Taller 7 (Opportunities & Solutions) del curso AREM - Universidad de La Sabana.*
