# Plan de Gobernanza de la Implementación

## Cliente
Centro de Contacto (Contact Center) — Universidad de La Sabana.

## Integrantes del equipo
- Nicolas Esteban Muñoz Sendoya (GitHub: N1kxo)
- Juan David Orozco Rodríguez (GitHub: DavidOrozcoJ)

## Propósito de este documento
El Comité de Arquitectura del proyecto (Supervisora del Centro de Contacto, Gestor de Casos y el arquitecto o arquitecta de la solución que designe la Dirección de Tecnología) supervisa la ejecución de `plan-implementacion.md` durante sus 4 fases. Si una fase necesita desviarse del diseño TO-BE aprobado en el Taller 7 (por ejemplo, por una restricción de licenciamiento de Microsoft 365), esa desviación no se decide informalmente: pasa por el mecanismo de escalamiento definido más abajo.

## Principios Arquitectónicos

> El Taller 0 (`00-preliminary-vision/vision.md`) no definió una sección explícita de "3 principios arquitectónicos": definió 2 restricciones explícitas en las secciones Alcance y Justificación que, en la práctica, ya funcionaban como principios. Se toman esas 2 restricciones como el borrador del Taller 0, se confirman y se formalizan, y se agregan 3 principios nuevos que el proyecto sí necesitó, con evidencia de los Talleres 1 a 8.

| Principio | Declaración | Implicación (qué descarta) | Justificación en el proyecto (evidencia de los talleres) | Estado frente al Taller 0 |
|---|---|---|---|---|
| Evolución institucional, no reemplazo | Toda solución se construye sobre la suite Microsoft 365 institucional y sobre Anexo Unisabana, sin introducir software externo ni reemplazar la plataforma de tickets vigente. | Queda descartado proponer una plataforma de terceros (CRM externo, helpdesk SaaS) o migrar los datos fuera de M365; cualquier excepción exige ADR. | Es la restricción más explícita de la Visión del Taller 0 ("sin instalar software externo", "no reemplazar Anexo Unisabana"); sostiene directamente la decisión del Taller 7 de elegir una Lista de SharePoint (dentro de M365) en vez de Dataverse/Power Apps como plataforma nueva. | Confirmado (del borrador del Taller 0) |
| Trazabilidad por diseño | Todo cambio de estado sobre un caso PQRSF queda registrado automáticamente (quién, cuándo, qué campo cambió), sin depender de que una persona lo anote a mano. | Queda prohibido que un nuevo flujo de datos del proceso se construya sin un mecanismo de historial propio (por ejemplo, un nuevo Excel paralelo cuyo único historial sea el de versiones del archivo). | La falta de trazabilidad es el problema #1 de la Ficha de Caracterización (Taller 0) y la causa de las amenazas T2/T3 del STRIDE (Taller 5, Tampering/Repudiation); es la razón por la que la Opción A (historial nativo de Excel) quedó eliminada en la matriz de decisión del Taller 7: SharePoint guarda versiones del archivo completo y Excel muestra los cambios recientes por celda, pero ese registro se puede reiniciar, tiene vacíos y se pierde con ediciones desde versiones antiguas de Excel, así que no sirve como evidencia por caso (ver ADR #1). | Nuevo (necesario desde el Taller 1, formalizado en el Taller 7) |
| Seguridad y protección de datos por diseño | El acceso a los datos personales del proceso se segmenta por rol desde el diseño de cada componente nuevo, no se restringe después como parche. | Ningún componente nuevo (Lista de SharePoint, flujo de Power Automate) se activa en producción sin su permisos por rol/columna definidos y sin pasar por el análisis STRIDE correspondiente. | El Taller 5 (STRIDE) identificó T4 (Information Disclosure) como riesgo Alto sobre el Excel compartido; el Taller 6 (normatividad), tras la revisión del Corte 2, deja 10 de 12 ítems del checklist en Parcial. Este principio es la base de R6 y R7 en `matriz-riesgos.xlsx`. | Nuevo (necesario desde el Taller 5) |
| Bajo costo de adopción dentro del ecosistema conocido | Las soluciones priorizan herramientas que el Centro de Contacto ya usa o que están un paso de distancia de lo que ya conoce (Excel → Lista de SharePoint), antes que alternativas técnicamente superiores pero con mayor curva de aprendizaje. | Queda descartado proponer una plataforma de desarrollo a la medida (código propio) para un equipo de 1-2 personas sin perfil técnico de desarrollo. | Es el criterio explícito de mayor peso combinado (Complejidad 20% + Tiempo 20% = 40%) en la matriz de decisión ponderada del Taller 7, que eliminó la Opción C (Dataverse/Power Apps) por mayor complejidad frente a la Lista de SharePoint. | Nuevo (necesario desde el Taller 7) |
| Gobierno explícito de las excepciones | Ninguna desviación del diseño TO-BE aprobado se implementa sin pasar por el Comité de Arquitectura definido en este documento. | Queda prohibido que una fase del roadmap cambie de alcance o de herramienta sin una aprobación escrita registrada. | Es la lección directa del caso CrowdStrike (Taller 9, sección 9): un cambio sin control de excepción explícito puede propagarse a todo el sistema a la vez. Sostiene el mecanismo de escalamiento de este documento. | Nuevo (necesario desde este taller) |

No hay principios del borrador del Taller 0 que se retiren: las 2 restricciones originales se confirman sin contradicción en ningún taller posterior.

## Estándares Tecnológicos

| Categoría | Estándar definido | Por qué |
|---|---|---|
| Lenguaje / framework | Automatización low-code sobre Microsoft Power Platform (Power Automate); no se desarrolla software a la medida | Coherente con el principio "Bajo costo de adopción": el Centro de Contacto no tiene perfil de desarrollo propio, y el Taller 7 ya descartó Dataverse/Power Apps por mayor complejidad frente a esta opción |
| Tipo de comunicación | Conectores nativos de Microsoft 365 (SharePoint, Outlook, Teams) orquestados por Power Automate; no se construyen integraciones REST a medida | No hay desarrollo de software propio que exponer vía API; toda integración nueva permanece dentro del mismo tenant de M365, sosteniendo el principio "Evolución institucional, no reemplazo" |
| Infraestructura | Nube Microsoft 365 institucional (SharePoint Online, Power Automate, Microsoft Purview) para los componentes nuevos; continuidad del Servidor Anexo Unisabana on-premise mediante el BCP/DRP de la fase Corto plazo | Respeta la restricción institucional de no introducir infraestructura externa (Taller 0) y resuelve el único riesgo de infraestructura on-premise real (R5, SPOF del servidor) sin migrarlo fuera de la universidad |

## Mecanismo de gobierno

| Rol | Responsabilidad | Persona / equipo sugerido |
|---|---|---|
| Sponsor / aprobador final | Aprueba el cierre de cada fase y autoriza excepciones al diseño TO-BE | Supervisora del Centro de Contacto |
| Representante operativo | Valida que el resultado de cada fase funciona en el día a día del proceso | Gestor de Casos |
| Arquitecto/a de la solución | Verifica que la implementación de cada fase coincide con el TO-BE del Taller 7 y con el tablero integrado del Taller 8 | Persona que designe la Dirección de Tecnología al recibir el proyecto (el equipo Innova2 entrega el diseño el 17/11/2026 y no continúa en la ejecución) |
| Punto de contacto técnico | Ejecuta o supervisa la configuración de SharePoint, Power Automate, Purview y el BCP/DRP del servidor | Dirección de Tecnología (equipo TI institucional) |

## Cadencia de seguimiento
Reunión mensual del Comité de Arquitectura mientras dure la ejecución del roadmap (aproximadamente 6 meses, de la fase Quick win a la fase Largo plazo de `plan-implementacion.md`). Al cierre de cada fase, el arquitecto o arquitecta de la solución presenta el resultado visible de esa fase (ver tabla en `plan-implementacion.md`) en la misma reunión.

## Criterio de conformidad
Una fase se cierra solo si el arquitecto o arquitecta de la solución valida que el resultado coincide con el diseño TO-BE del Taller 7 (`07-opportunities-solutions/to-be-aplicaciones-final.drawio` y `to-be-tecnologia-final.drawio`) y con el tablero integrado del Taller 8 (`08-integracion-vistas/tablero-integrado-cliente.drawio`). No es una opinión subjetiva: es una comparación elemento por elemento contra esos dos entregables ya aprobados.

## Escalamiento
Cualquier desviación del diseño aprobado (por ejemplo, que Microsoft Purview no esté disponible en el licenciamiento institucional actual) debe proponerla quien la detecte, evaluarla el arquitecto o arquitecta de la solución frente a los Principios Arquitectónicos de este documento, y debe quedar aprobada por escrito por la Supervisora del Centro de Contacto antes de continuar la fase. Si la desviación es lo suficientemente importante como para cambiar el diseño TO-BE mismo (no solo un detalle de implementación), se registra como una nueva ADR siguiendo `procedimiento-cambios.md`.

## Métricas de gobierno

| Métrica | Qué mide (principio) | Fórmula | Fuente que ya existe | Línea base | Umbral |
|---|---|---|---|---|---|
| M1 · Trazabilidad de cambios | Si se cumple **Trazabilidad por diseño** | (Casos de la muestra en los que se puede saber quién cerró el caso y cuándo ÷ casos de la muestra) × 100 | Muestra mensual de 20 casos cerrados, revisada con el historial que exista en ese momento: hoy, versiones del archivo en SharePoint y "Mostrar cambios" de Excel; en el TO-BE, el historial de la Lista | Por medir en la primera revisión del comité (diciembre de 2026). Se espera baja: "Mostrar cambios" solo conserva cambios recientes y se puede reiniciar | Menos de 90% en los casos del piloto al cerrar Mediano plazo, o menos de 100% en todos los casos al cerrar Largo plazo |
| M2 · Cumplimiento normativo | Si se cumple **Seguridad y protección de datos por diseño** | (Ítems del checklist de normatividad en estado "Cumple" ÷ 12) × 100 | `06-normatividad/checklist-cliente.xlsx` (Taller 6), ya existe | 17% (2 de 12 ítems en "Cumple", tras pasar los ítems 3 y 4 a Parcial en la revisión del Corte 2) | Por debajo de 42% (5/12: ARCO, retención y roles) al cerrar Quick win; de 50% (6/12: + BCP/DRP probado) al cerrar Corto plazo; o de 67% (8/12: + registro de cambios y DLP) al cerrar Mediano plazo |
| M3 · Tiempo de dictamen del comité (mide al propio comité) | Si el **gobierno explícito de las excepciones** estorba en vez de ayudar | Días hábiles entre la propuesta de una ADR o excepción y su dictamen | Registro de ADR en `procedimiento-cambios.md` (fechas Propuesta → Dictamen) | No tiene línea base: el registro de ADR empieza a existir con este plan | Más de 10 días hábiles |

---

_Este documento hace parte de la entrega final del Taller 9 del curso AREM (Arquitectura Empresarial) - Universidad de La Sabana._
