# Resumen Ejecutivo del Proyecto Arquitectónico

## Nombre del Cliente
Centro de Contacto (Contact Center) — Universidad de La Sabana. Contacto: Johanna Andrea Molina, Supervisora del Centro de Contacto.

## Objetivo General del Proyecto
El Centro de Contacto gestiona los casos PQRSF ("Comuníquese con Nosotros") mediante Anexo Unisabana y un Excel paralelo mantenido manualmente por el gestor de casos. Ese Excel es, en la práctica, la fuente de verdad real del proceso, pero no tiene control de versiones, ni auditoría de cambios, ni alertas de vencimiento de SLA. El proyecto analizó esta arquitectura actual bajo la metodología TOGAF ADM y propone una solución construida enteramente dentro de Microsoft 365 (sin software externo, respetando la restricción institucional) que cierra las brechas de trazabilidad, continuidad y cumplimiento normativo identificadas, con una ruta de implementación concreta.

## Vistas Arquitectónicas Cubiertas

| Vista | Alcance de la Solución |
|---|---|
| Procesos de Negocio | Modelado BPMN del proceso de gestión y seguimiento de casos PQRSF, con el punto crítico de pérdida de trazabilidad resaltado (Taller 1) |
| Información / Datos | Modelo ER (`TICKET`, `USUARIO_SOLICITANTE`, `CANAL`, `CATEGORIA`, `AREA_RESPONSABLE`, `REGISTRO_AUDITORIA`) y diagrama de contexto del proceso (Taller 2) |
| Aplicaciones / Sistemas | C1 y C2 del modelo C4: Anexo Unisabana, Excel/Consolidado de Casos, registros manuales por canal (Taller 3) |
| Infraestructura | Mapa de infraestructura con el Servidor Anexo Unisabana como punto único de falla (Taller 4) |
| Seguridad | Análisis STRIDE (8 amenazas, T1–T8) sobre el flujo de registro y enrutamiento de un caso (Taller 5) |
| Cumplimiento Normativo | Checklist contra Ley 1581 de 2012, Decreto 1377/2013 e ISO/IEC 27001 (Taller 6) |
| Solución e Integración | TO-BE de aplicaciones y tecnología, matriz de brechas y tablero integrado de las 5 vistas (Talleres 7 y 8) |

## Hallazgos Clave

- El Excel compartido —no Anexo Unisabana— es la verdadera fuente de datos del proceso, y no tiene control de versiones ni registro de quién cambia qué.
- El Servidor Anexo Unisabana es una instancia única on-premise, sin redundancia ni plan de recuperación documentado.
- 8 de los 12 ítems del checklist de cumplimiento normativo (Taller 6) están en estado "Parcial", principalmente por falta de un procedimiento ARCO documentado.
- El proceso depende de una sola persona (el gestor de casos) para registrar, clasificar y enrutar todos los casos, sin un rol de respaldo documentado.

## Recomendaciones Principales

- Migrar el Consolidado de Casos de Excel a una **Lista de SharePoint** con historial nativo y permisos por rol/columna (WP1).
- Automatizar el registro de auditoría y las alertas tempranas de SLA con **Power Automate**, reemplazando la actualización manual del estado en Excel.
- Implementar un **Plan de Continuidad (BCP/DRP)** para el Servidor Anexo Unisabana (WP2).
- Documentar el **procedimiento ARCO**, evaluar la **transferencia internacional de datos vía WhatsApp Business** y definir una política de retención/anonimización (WP3), cerrando la brecha normativa identificada en el Taller 6.

## Plan de Implementación (resumen)

| Fase | Qué se implementa | Duración estimada |
|---|---|---|
| Quick win | Procedimiento ARCO, evaluación del uso de WhatsApp Business y manual de respaldo del gestor de casos | 0–1 mes |
| Corto plazo | Plan de Continuidad (BCP/DRP) del Servidor Anexo Unisabana | 1–2 meses |
| Mediano plazo | Piloto de la Lista de SharePoint, permisos por rol y Microsoft Purview | 2–4 meses |
| Largo plazo | Rollout completo + automatización con Power Automate | 4–6 meses |

_Ver el detalle completo, con la relación de cada fase a los riesgos identificados en el curso, en [`09-presentacion-final/plan-implementacion.md`](09-presentacion-final/plan-implementacion.md)._

## Reflexión Final
Este ejercicio permitió al equipo aplicar los conceptos de arquitectura empresarial sobre un proceso real de la Universidad, con un cliente real que validó cada entrega: desde entender por qué un Excel paralelo termina siendo la fuente de verdad de un proceso, hasta traducir ese diagnóstico en una solución que el Centro de Contacto puede implementar con las herramientas que ya tiene, sin depender de software externo ni de un presupuesto de desarrollo que el área no tiene.

---

_Este resumen ejecutivo forma parte de la entrega final del curso AREM - Arquitectura Empresarial - Universidad de La Sabana._
