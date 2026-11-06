# Procedimiento de Actualización y Revisión de la Arquitectura

## Cliente
Centro de Contacto (Contact Center) — Universidad de La Sabana. Contacto: Johanna Andrea Molina.

## Integrantes del equipo
- Nicolas Esteban Muñoz Sendoya (nico9ms@hotmail.com — GitHub: N1kxo)
- Juan David Orozco Rodríguez (davidorozcoj1@gmail.com — GitHub: DavidOrozcoJ)

## Propósito de este documento
`plan-implementacion.md` ejecuta un cambio ya decidido (la migración a SharePoint, el BCP/DRP, el cumplimiento normativo); `plan-gobernanza.md` supervisa esa ejecución mientras ocurre. Este documento cubre qué pasa **después**: cuando el roadmap del curso ya se ejecutó y surge una necesidad de cambio nueva que nadie anticipó en este semestre.

## Disparadores de cambio

| Disparador | Ejemplo concreto para este cliente |
|---|---|
| Nueva regulación | Una modificación a la Ley 1581 de 2012 o al Decreto 1377/2013, o una actualización de la Política de Protección de Datos v5.0 de la Universidad de La Sabana |
| Cambio de proveedor tecnológico | Un cambio en el licenciamiento de Microsoft 365 institucional que afecte la disponibilidad de SharePoint, Power Automate o Microsoft Purview; o la descontinuación/actualización mayor de Anexo Unisabana por parte de la Dirección de Tecnología |
| Incidente de seguridad | Una fuga real de datos personales de PQRSF, vía el Excel/SharePoint compartido o vía el canal de WhatsApp |
| Crecimiento inesperado de demanda | Un aumento sostenido de +30% en el volumen semestral de casos PQRSF, que supere la capacidad del Gestor de los casos como único gestor de casos (riesgo R1) |
| Otro | (a) Si la evaluación de transferencia internacional vía WhatsApp Business (fase Quick win, riesgo R8) concluye que el canal no cumple el Decreto 1377/2013, se abre una ADR para migrar la recepción a un canal 100% Microsoft 365. (b) Si la central telefónica/PBX (riesgo R9, aceptado) registra caídas repetidas en un semestre, ese riesgo deja de ser aceptable y se revisa |

## Proceso de cambio
1. **Quién propone.** Cualquier integrante del Centro de Contacto (Supervisor, Gestor de casos, una Facultad/Unidad Responsable) o del equipo de arquitectura puede proponer un cambio, usando el registro de ADR de este documento como formulario.
2. **Quién evalúa.** El Comité de Arquitectura definido en `plan-gobernanza.md` (El supervisor del centro de contacto, el gestor de los casos, y el equipo de arquitectura) evalúa la propuesta contra los objetivos estratégicos de la Ficha de Caracterización (Taller 0) y contra los Principios Arquitectónicos de `plan-gobernanza.md`.
3. **En cuánto tiempo.** El Comité dictamina (acepta, rechaza o pide ajustes) en un máximo de **10 días hábiles** desde la propuesta — el mismo umbral de la métrica M3 de `plan-gobernanza.md`; si lo supera, la métrica avisa que el proceso mismo necesita revisión.

## Registro de decisiones (ADR)

Estas 2 ADR no son hipotéticas: formalizan decisiones que el equipo ya tomó durante el curso (Taller 7) y que sostienen directamente el Plan de Implementación.

| Campo | ADR #1 |
|---|---|
| Título | Migración del Consolidado de Casos a una Lista de SharePoint, en lugar de historial nativo de Excel o de Dataverse/Power Apps |
| Estado | Aceptada |
| Fecha de propuesta | Taller 7 (sesión de clase, Corte 2) — la fecha exacta de la sesión no quedó registrada en el repositorio del equipo |
| Fecha de dictamen | Misma sesión del Taller 7 en la que se construyó la matriz de decisión ponderada |
| Contexto | El Consolidado de Casos del Centro de Contacto vive hoy en un Excel compartido sin control de versiones ni auditoría (Talleres 2 y 3); el STRIDE del Taller 5 identificó esto como las amenazas T2/T3 (Tampering/Repudiation) y T4 (Information Disclosure). |
| Problema | Sin trazabilidad real, el equipo no puede cumplir el indicador institucional de oportunidad del 80% (Taller 0) ni demostrar quién hizo cada cambio, lo que además incumple parcialmente la Ley 1581 (Taller 6). |
| Decisión | Migrar el Consolidado de Casos a una **Lista de SharePoint** con historial nativo y permisos por rol/columna (Opción B de la matriz de decisión del Taller 7). |
| Alternativas consideradas | (a) **Historial nativo de Excel + notificación por Power Automate** (Opción A): obtuvo el mayor puntaje ponderado bruto (4.30 sobre criterios Trazabilidad 35%, Costo 25%, Complejidad 20%, Tiempo 20%), pero quedó **eliminada por el criterio eliminatorio de Trazabilidad** (puntaje de Trazabilidad menor a 4): Excel no ofrece un historial de cambios confiable incluso con notificaciones automáticas. (b) **Dataverse/Power Apps** (Opción C): puntaje ponderado de 3.05, descartada por mayor complejidad y tiempo de implementación frente al perfil del equipo del Centro de Contacto (sin desarrolladores dedicados). (c) **Lista de SharePoint** (Opción B): puntaje ponderado de 4.20, la mayor entre las opciones que sí superaban el criterio eliminatorio. |
| Consecuencias | (+) El Consolidado de Casos queda con historial de cambios nativo, auditable y con permisos por rol/columna, sin salir de Microsoft 365. (−) Se acepta la curva de aprendizaje del Gestor de los casos y el supervisor sobre una herramienta nueva (SharePoint Lists), y se posterga la automatización de notificaciones de SLA hasta que la migración de datos esté completa (fase Largo plazo del plan). **Trade-off aceptado:** se sacrifica la familiaridad inmediata del equipo con Excel a cambio de trazabilidad real y verificable. |

| Campo | ADR #2 |
|---|---|
| Título | Plan de Continuidad (BCP/DRP) para el Servidor Anexo Unisabana, en lugar de solo reforzar el monitoreo o migrar la plataforma |
| Estado | Aceptada |
| Fecha de propuesta | Taller 7 (sesión de clase, Corte 2) — la fecha exacta de la sesión no quedó registrada en el repositorio del equipo |
| Fecha de dictamen | Misma sesión del Taller 7, como parte de la priorización de soluciones (idea #2, clasificada como "quick win") |
| Contexto | El Taller 4 identificó el Servidor Anexo Unisabana como un punto único de falla: instancia única on-premise, sin redundancia ni plan de recuperación documentado (riesgo de infraestructura de nivel Alto). |
| Problema | Si el servidor falla, toda la operación de registro de tickets de Anexo Unisabana queda indisponible, sin ningún mecanismo de contingencia definido. |
| Decisión | Implementar un Plan de Continuidad (BCP/DRP) sobre la infraestructura actual: respaldo periódico y un procedimiento de recuperación documentado y probado. |
| Alternativas consideradas | (a) **Reforzar solo el monitoreo**, sin plan de recuperación: descartada porque no reduce el impacto si la falla ocurre, solo acorta el tiempo de detección. (b) **Migrar Anexo Unisabana a la nube** para eliminar el servidor on-premise de raíz: descartada porque el Taller 0 excluyó explícitamente de su alcance "reemplazar la plataforma de tickets institucional (Anexo Unisabana)" — es una decisión del cliente, no una limitación técnica. (c) **BCP/DRP sobre la infraestructura actual**: elegida, por ser la opción de menor costo y complejidad que sí reduce el impacto de una falla, respetando la restricción institucional. |
| Consecuencias | (+) El proceso puede recuperarse ante una falla del servidor en un tiempo acotado (RTO/RPO documentados), sin depender de que "no pase nada". (−) Persiste la dependencia de una única plataforma institucional de terceros (Anexo Unisabana): esta deuda se acepta conscientemente porque está fuera del alcance acordado con el cliente (Taller 0), no porque se haya pasado por alto. **Trade-off aceptado:** se sacrifica eliminar el punto único de falla de raíz (migrar) a cambio de respetar la restricción institucional y mantener bajo el costo y la complejidad de la intervención. |

### Plantilla mínima de ADR (para decisiones futuras)

| Campo | Contenido |
|---|---|
| Título | Nombre corto de la decisión |
| Estado | Propuesta / Aceptada / Reemplazada |
| Fecha de propuesta | Cuándo se pidió la decisión (alimenta la métrica M3) |
| Fecha de dictamen | Cuándo la aceptó o rechazó el Comité |
| Contexto | Qué disparó la necesidad de decidir |
| Problema | Qué problema concreto genera no decidir, o decidir mal |
| Decisión | Qué se decidió |
| Alternativas consideradas | Qué otras opciones se evaluaron y por qué no se eligieron |
| Consecuencias | (+) qué se gana · (−) qué se pierde o qué deuda se acepta · trade-off aceptado |

## Revisión periódica completa
Toda la arquitectura se revisa contra el estado real del proceso **cada semestre**, alineada al mismo ciclo semestral en que el Centro de Contacto ya produce los reportes "Ayúdanos a mejorar" e "Infórmate aquí" (Taller 3) — así la revisión arquitectónica no exige un calendario nuevo, se apoya en uno que el cliente ya tiene.

---

_Este documento hace parte de la entrega final del Taller 9 del curso AREM (Arquitectura Empresarial) - Universidad de La Sabana._
