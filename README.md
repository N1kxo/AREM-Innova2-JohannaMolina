
# Mejora de la gestión de PQRSF — Centro de Contacto, Universidad de La Sabana

**Equipo:** Innova2 · Nicolas Esteban Muñoz Sendoya, Juan David Orozco Rodriguez
**Curso:** Arquitectura Empresarial — Universidad de La Sabana

---

## 📌 En una frase

Analizamos cómo el Centro de Contacto gestiona hoy las peticiones, quejas, reclamos, sugerencias y felicitaciones ("Comuníquese con Nosotros") y proponemos mejoras para que cada caso quede trazado, se atienda a tiempo y los datos de los usuarios estén protegidos, usando las herramientas que la Universidad ya tiene.

## 🩺 El problema

Los casos llegan por correo, WhatsApp, llamadas, el sitio web y la app de la Universidad. Se registran en la plataforma de tickets, pero esta no permite exportar todos los campos que el área necesita. Por eso el equipo mantiene en paralelo un Excel compartido, que en la práctica es la fuente de verdad. Esto genera cuatro dolores:

- No queda registro de quién cambió qué y cuándo en el Excel, lo que complica la auditoría de calidad (ISO 9001).
- Los informes semestrales se arman a mano y toman mucho tiempo.
- No hay alertas cuando un caso se acerca a su fecha límite, y la meta es atender el 80 % a tiempo.
- Si el servidor de la plataforma falla, o si la persona que atiende los canales manuales no está, la operación se detiene.

## 💡 Lo que proponemos

- **Historial y control de cambios:** que cada modificación a un caso quede registrada (quién, cuándo y qué), con permisos por rol.
- **Alertas oportunas:** avisos automáticos antes de que un caso venza, para llegar a la meta del 80 % a tiempo.
- **Continuidad del servicio:** un plan de respaldo y recuperación para que una falla del servidor no deje al Centro de Contacto sin operar.
- **Cumplimiento normativo:** un paquete de documentación para la protección de datos personales (Ley 1581), con procedimiento para atender solicitudes de los titulares, evaluación del uso de WhatsApp Business frente a la transferencia internacional de datos y criterios de conservación y anonimización.
- **Implementación en fases**, empezando por lo más urgente y de menor costo, y sin salir del ecosistema que la Universidad ya usa.


## 🗺️ Cómo se implementa

Ver el **[Resumen Ejecutivo](resumen-ejecutivo.md)** — ahí está el detalle de beneficios esperados, fases de implementación y tiempos, en un solo documento pensado para el negocio, no para el equipo técnico.

## 📂 Si quiere ver el detalle técnico completo

Todo el análisis que sustenta esta propuesta está documentado carpeta por carpeta, siguiendo el método usado durante el proyecto:

| Carpeta | Qué contiene |
|---|---|
| `00-preliminary-vision/` | Contexto del cliente y visión de la solución |
| `01-bpmn/` | Cómo funciona hoy el proceso de negocio analizado |
| `02-modelo-informacion/` | Qué información maneja el negocio y cómo fluye |
| `03-arquitectura-c4/` | Los sistemas actuales y cómo están construidos |
| `04-infraestructura/` | Dónde corre todo hoy y qué riesgos técnicos tiene |
| `05-seguridad-stride/` | Análisis de seguridad de la información |
| `06-normatividad/` | Cumplimiento legal y normativo |
| `07-opportunities-solutions/` | La solución propuesta y qué brechas cierra |
| `08-integracion-vistas/` | Cómo se conecta todo lo anterior en una sola arquitectura |
| `09-presentacion-final/` | Presentación ejecutiva, plan de implementación y gobernanza |

## 👥 Contacto

Johanna Andrea Molina, johannaMR@unisabana.edu.co