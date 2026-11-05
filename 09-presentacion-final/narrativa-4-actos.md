# Narrativa de 4 Actos — Simulación de Comité de Arquitectura

## Cliente
Centro de Contacto (Contact Center) — Universidad de La Sabana. Contacto: Johanna Andrea Molina.

## Integrantes del equipo
- Nicolas Esteban Muñoz Sendoya (nico9ms@hotmail.com — GitHub: N1kxo)
- Juan David Orozco Rodríguez (davidorozcoj1@gmail.com — GitHub: DavidOrozcoJ)

> Este documento es la base de la presentación ejecutiva de máximo 10 minutos (Paso 1-2-5-6 de la guía del Taller 9). No reemplaza las diapositivas: es el guion y la selección de evidencia de donde se arman.

## Paso 1 — Los 4 actos

**Acto 1 — Problema.** "El Centro de Contacto de la Universidad de La Sabana pierde trazabilidad en cada caso PQRSF que gestiona: hoy el Excel compartido que realmente sostiene el proceso se edita a mano, sin que quede registro de quién cambió qué ni cuándo, y sin alertas cuando un caso está por vencer su SLA."

**Acto 2 — Análisis.** "La arquitectura actual combina Anexo Unisabana (que no exporta todos los campos necesarios) con un Excel paralelo que es, en la práctica, la verdadera fuente de datos — corriendo sobre un servidor sin redundancia ni plan de recuperación, y con 8 de 12 ítems de cumplimiento normativo en estado Parcial."

**Acto 3 — Solución.** "Se propone migrar ese Excel a una Lista de SharePoint con historial nativo y permisos por rol, automatizar el registro de auditoría y las alertas de SLA con Power Automate, y cerrar las brechas de continuidad y cumplimiento — todo dentro de Microsoft 365, sin introducir ninguna herramienta externa, priorizada frente a Dataverse/Power Apps por menor complejidad y curva de adopción."

**Acto 4 — Plan de Implementación.** "La solución se ejecuta en 4 fases a lo largo de unos 6 meses (noviembre 2026 a mayo 2027): empieza por el paquete de cumplimiento (ARCO, evaluación de WhatsApp) y el respaldo del rol del gestor de casos (bajo esfuerzo, corrige una brecha de cumplimiento urgente) y termina con la automatización completa de Power Automate (mayor esfuerzo, pero es la que cierra el riesgo que más afecta el día a día)."

## Paso 2 — Evidencia clave por acto

| Acto | Vista de origen | Qué mostrar (un solo elemento, no todo el taller) |
|---|---|---|
| Problema | Vista de negocio (Taller 1) | El BPMN con el punto crítico resaltado en ámbar: "Actualizar estado a Cerrado en Excel" |
| Análisis | Mapa de infraestructura + STRIDE (Taller 4 / 5) | El Servidor Anexo Unisabana y el Excel compartido marcados ⚠️, con las amenazas T2/T3/T4 señaladas encima |
| Solución | Tablero integrado (Taller 8) | El hilo "Registro, enrutamiento y cierre de un caso PQRSF" con los elementos NUEVO (Lista de SharePoint, Power Automate) resaltados en verde |
| Plan de Implementación | Matriz de riesgos (Taller 9, Paso 3) + roadmap | El Gantt de 4 fases con el riesgo que corrige cada una |

## Paso 3 — Matriz de riesgos
Ver [`matriz-riesgos.xlsx`](matriz-riesgos.xlsx) — 9 riesgos reales, 7 dominios cubiertos (Negocio, Procesos, Datos, Aplicaciones, Infraestructura, Seguridad, Gobierno TI), con un riesgo aceptado de forma explícita (R9, caída de la central telefónica/PBX: hay otros cuatro canales de radicación y la PBX está fuera del control del área).

## Paso 4 — Plan de implementación
Ver [`plan-implementacion.md`](plan-implementacion.md) — 4 fases, gestión del cambio con las personas, y un resultado visible por fase comprobado fuera de TI.

## Paso 5 — Preparar la defensa

| Pregunta probable del panel | Respuesta basada en evidencia |
|---|---|
| "¿Por qué no reemplazan Anexo Unisabana si tiene tantas limitaciones?" | "El Taller 0 excluyó explícitamente reemplazar la plataforma de tickets institucional del alcance del proyecto — es una decisión del cliente, no una limitación técnica nuestra; por eso la solución se construye alrededor de Anexo Unisabana, no en su lugar (ver ADR #2, `procedimiento-cambios.md`)." |
| "¿Cómo garantizan que la Lista de SharePoint realmente mejora la trazabilidad y no es solo cambiar de Excel a otro Excel?" | "El Taller 7 comparó tres opciones con una matriz ponderada: la opción de quedarse en Excel con notificaciones (Opción A) tuvo el puntaje bruto más alto, pero quedó eliminada por el criterio eliminatorio de Trazabilidad — Excel no ofrece un historial de cambios confiable aunque se le agreguen alertas. La Lista de SharePoint sí lo tiene de forma nativa (ver ADR #1)." |
| "¿Por qué la migración completa (el riesgo de mayor prioridad) no es la primera fase?" | "Es la fase de mayor esfuerzo de desarrollo y depende de que el licenciamiento de Microsoft Purview esté confirmado; empezar por los quick wins de bajo esfuerzo (procedimiento ARCO, respaldo del rol) genera resultados visibles y cumplimiento urgente mientras se prepara la migración." |
| "¿Qué pasa si Alexander, el único gestor de casos, no está disponible?" | "Es el riesgo R1 de la matriz, dominio Negocio: se mitiga en la fase Quick win con un manual de respaldo del rol, documentado y aprobado por Johanna como parte del mismo procedimiento ARCO." |

## Paso 6 — Ensayo contra el tiempo

| Sección | Tiempo objetivo |
|---|---|
| Acto 1 — Problema | 2 min |
| Acto 2 — Análisis | 2 min |
| Acto 3 — Solución | 2 min |
| Acto 4 — Plan de Implementación | 2 min |
| Cierre y transición a preguntas | 2 min |
| **Total** | **10 min** |

> Pendiente del equipo: ensayar cronometrado (sesión del 07/11, ensayo con el docente) y ajustar qué integrante presenta cada acto — este documento da el guion, no reemplaza la práctica en vivo.

---

_Este documento es la base de trabajo de la Parte 1 (clase) del Taller 9 — curso AREM, Universidad de La Sabana._
