# Narrativa de 4 Actos — Simulación de Comité de Arquitectura

## Cliente
Centro de Contacto (Contact Center) — Universidad de La Sabana.

## Integrantes del equipo
- Nicolas Esteban Muñoz Sendoya (GitHub: N1kxo)
- Juan David Orozco Rodríguez (GitHub: DavidOrozcoJ)

> Este documento es la base de la presentación ejecutiva de máximo 10 minutos (Pasos 1, 2, 5 y 6 de la guía del Taller 9). No reemplaza las diapositivas: es el guion y la selección de evidencia de donde salen.

## Paso 1 — Los 4 actos

**Acto 1 — Problema.** "El Centro de Contacto de la Universidad de La Sabana pierde trazabilidad en cada caso PQRSF: el Excel compartido que realmente sostiene el proceso se edita a mano, no deja un registro confiable de quién cambió cada caso ni cuándo, y no avisa cuando un caso está por vencer su SLA."

**Acto 2 — Análisis.** "La arquitectura actual combina Anexo Unisabana, que no exporta todos los campos, con un Excel paralelo que es la verdadera fuente de datos. El punto único de falla más grave no es un servidor sino un rol: el Gestor de Casos, sin respaldo. Además, el servidor de Anexo no tiene plan de recuperación y 10 de 12 ítems de cumplimiento normativo están en Parcial."

**Acto 3 — Solución.** "Proponemos migrar ese Excel a una Lista de SharePoint, donde cada caso tiene su propio historial y permisos por rol, y automatizar el registro de auditoría y las alertas de SLA con Power Automate. Todo queda dentro de Microsoft 365, sin herramientas externas. La elegimos frente a Dataverse/Power Apps por menor complejidad."

**Acto 4 — Plan de Implementación.** "La solución se ejecuta en 4 fases, de noviembre de 2026 a mayo de 2027. Empieza por lo urgente y barato (respaldo del Gestor de Casos, procedimiento ARCO, evaluación de WhatsApp) y termina con la automatización en Power Automate. La ejecuta un arquitecto o arquitecta que designe la Dirección de Tecnología, porque nuestro equipo entrega el proyecto al cerrar el semestre."

## Paso 2 — Evidencia clave por acto

| Acto | Vista de origen | Qué mostrar (un solo elemento, no todo el taller) |
|---|---|---|
| Problema | Vista de negocio (Taller 1) | El BPMN con el punto crítico resaltado en ámbar: "Actualizar estado a Cerrado en Excel" |
| Análisis | Mapa de infraestructura (Taller 4) + STRIDE (Taller 5) | El PC del Gestor de Casos y el Excel compartido marcados ⚠️, con las amenazas T2/T3/T4 sobre el Excel |
| Solución | Tablero integrado (Taller 8) | El hilo "Registro, enrutamiento y cierre de un caso PQRSF" con los elementos NUEVO (Lista de SharePoint, Power Automate) en verde |
| Plan de Implementación | Matriz de riesgos + roadmap (Taller 9) | Las 4 fases con el riesgo que corrige cada una |

## Paso 3 — Matriz de riesgos
Ver [`matriz-riesgos.xlsx`](matriz-riesgos.xlsx): 9 riesgos, 7 dominios (Negocio, Procesos, Datos, Aplicaciones, Infraestructura, Seguridad, Gobierno TI), tres críticos (R1, R2, R3) y un riesgo aceptado de forma explícita (R9, caída de la central telefónica: hay otros cuatro canales de radicación y la PBX está fuera del control del área).

## Paso 4 — Plan de implementación
Ver [`plan-implementacion.md`](plan-implementacion.md): 4 fases, quién ejecuta después del semestre, gestión del cambio con las personas y un resultado visible por fase comprobado fuera de TI.

## Paso 5 — Preparar la defensa

| Pregunta probable del panel | Respuesta basada en evidencia |
|---|---|
| "¿Por qué no reemplazan Anexo Unisabana si tiene tantas limitaciones?" | "El Taller 0 excluyó del alcance reemplazar la plataforma de tickets institucional. Es una decisión del cliente, no una limitación técnica nuestra; por eso la solución se construye alrededor de Anexo (ADR #2)." |
| "Si Anexo se queda y no exporta todo, ¿cómo cierran el doble registro (R2)?" | "No lo eliminamos del todo, lo reducimos. Llamadas, WhatsApp y correo se registran una sola vez, directo en la Lista. Para los casos web/app solo se anota el número de ticket de Anexo y los campos que Anexo no exporta. Eliminar esa última captura requiere una exportación desde Anexo, que depende de la Dirección de Tecnología: está como disparador de cambio." |
| "¿El Excel en SharePoint no tiene ya historial de versiones?" | "Sí: SharePoint guarda versiones del archivo y Excel muestra cambios recientes por celda. Lo verificamos en la documentación de Microsoft: ese registro de cambios se puede reiniciar, tiene vacíos y se pierde si alguien edita con una versión antigua de Excel; y las versiones son del archivo completo, no de cada caso. Por eso la Opción A no pasó el criterio eliminatorio de trazabilidad (ADR #1)." |
| "¿Por qué lo crítico de la migración no va primero?" | "Es la fase de mayor esfuerzo y depende de la licencia de Purview y de que TI designe al arquitecto. Lo crítico que sí se puede atacar ya, el respaldo del Gestor de Casos (R1), va en el quick win." |
| "¿Por qué el Gestor de Casos (R1) quedó crítico?" | "Porque en el Taller 4 lo identificamos como el punto único de falla más grave: es la única persona que registra los tres canales manuales. Sale con impacto y probabilidad altos, nivel 9, por encima del servidor (nivel 6)." |
| "¿Cómo saben que el BCP/DRP funciona?" | "La fase no se cierra con un documento: se cierra con el acta de una prueba de restauración real, con el tiempo medido frente al RTO, firmada por la Supervisora como testigo." |
| "¿Quién ejecuta esto cuando ustedes terminen el semestre?" | "Entregamos el proyecto a la Supervisora y a la Dirección de Tecnología, y recomendamos que TI designe un arquitecto o arquitecta de la solución. Ningún integrante del equipo aparece como responsable de una fase." |

## Paso 6 — Ensayo contra el tiempo

| Sección | Diapositivas | Tiempo objetivo | Presenta (propuesta, a confirmar entre los dos) |
|---|---|---|---|
| Apertura | 1–2 | 1 min | Juan David |
| Acto 1 — Problema | 3 | 2 min | Juan David |
| Acto 2 — Análisis | 4–5 | 2 min | Juan David |
| Acto 3 — Solución | 6–7 | 2 min | Nicolás |
| Acto 4 — Plan | 8–9 | 2 min | Nicolás |
| Cierre y transición a preguntas | 10 | 1 min | Nicolás |
| **Total** | | **10 min** | |

> Retroalimentación de los compañeros: menos texto y hablar más despacio. Si el ensayo cronometrado del 07/11 pasa de 10 minutos, se recorta contenido (por ejemplo, la diapositiva 7 se puede saltar y contar en 30 segundos), no se acelera la voz.

---

_Este documento es la base de trabajo de la Parte 1 (clase) del Taller 9 — curso AREM, Universidad de La Sabana._
