# Análisis de Caso Real y Tecnología

Este archivo documenta el análisis de un caso real, público y documentado, relacionado con el tema del taller, y la comparación de tecnologías que habrían evitado o mitigado lo ocurrido.

## Taller
_Taller 9 - Simulación de Comité de Arquitectura (cliente real: Centro de Contacto — Universidad de La Sabana)_

---

## 1. El caso

- **Nombre del caso:** Caída global de Microsoft 365 y Azure causada por un cambio de IP en un router de la WAN de Microsoft
- **Fecha:** 25/01/2023
- **Sector:** Proveedor de nube (afecta a todos los sectores que dependen de Microsoft 365, incluida la educación)
- **Fuentes:** Microsoft, *Preliminary Post Incident Review "Global WAN issues"*, historial de estado de Azure, tracking ID VSG1-B90 (citado textualmente por las fuentes de prensa siguientes; no se consultó directamente el documento original). The Register, 30/01/2023. BleepingComputer, 2023. Ver `referencias` al final.

## 2. Qué pasó
El 25 de enero de 2023, a las 07:05 UTC, Microsoft ejecutó un cambio planeado para actualizar la dirección IP de un router de su WAN. Según el reporte preliminar de Microsoft, el comando que se le dio al router hizo que este enviara mensajes a todos los demás routers de la WAN, que entonces recalcularon sus tablas de reenvío y, mientras lo hacían, no pudieron reenviar paquetes correctamente. Microsoft reconoció que el comando "no había sido verificado con nuestro proceso completo de calificación" en el router donde se ejecutó. Se vieron afectados la conectividad hacia Azure, Microsoft 365 y Power Platform. El monitoreo detectó el problema a las 07:12 UTC, la causa se identificó a las 08:20 UTC y la pérdida intermitente de paquetes no quedó resuelta del todo hasta cerca de las 12:43 UTC. Microsoft anunció que bloquearía la ejecución de comandos de alto impacto en dispositivos de red y que exigiría que toda ejecución de comandos siga guías de operación segura.

## 3. Qué falló, en el lenguaje del taller

| Elemento | Qué falló | Categoría | Impacto |
|---|---|---|---|
| Zona "Nube Microsoft 365" del mapa de infraestructura (Taller 4), de la que depende hoy el Excel compartido y, en el TO-BE, la Lista de SharePoint y Power Automate | El proveedor quedó inaccesible por horas por un cambio propio, sin que el cliente pudiera hacer nada | Disponibilidad (Taller 4) | El proceso PQRSF no puede registrar, actualizar ni consultar casos durante la caída |
| Flujos F4, F5 y F7 hacia el almacén D2 (Excel compartido) del DFD del Taller 5 | Los flujos no se pueden completar porque el almacén no responde | STRIDE: Denial of Service | Casos represados; los SLA siguen corriendo aunque el sistema esté caído |
| Procedimiento de cambios del proveedor (equivalente a la sección 6.3 de la guía) | Un comando sin la calificación completa llegó a producción y se propagó a toda la red | Gobierno del cambio | Una falla local se volvió global, como en el caso CrowdStrike de la guía |

## 4. Cómo se habría detectado antes
Del lado de Microsoft, con la regla que ellos mismos anunciaron después: que ningún comando de alto impacto se ejecute sin pasar por su proceso completo de calificación (el mismo principio de la verificación automática más aprobación humana de la sección 8 de la guía). Del lado del cliente, el incidente no se puede prevenir, pero sí detectar rápido y tener plan: si el Centro de Contacto recibe un aviso temprano de que Microsoft 365 está degradado, puede activar un procedimiento de contingencia en vez de enterarse porque el Excel "no carga".

## 5. Tecnologías que lo habrían evitado o mitigado

El Centro de Contacto no controla la red de Microsoft, así que la comparación se hace sobre cómo **detectar y responder** a una caída del proveedor.

| Criterio | Opción A: Estado del servicio de Microsoft 365 (Service Health del centro de administración, con notificaciones) | Opción B: Monitoreo sintético externo (por ejemplo, UptimeRobot) |
|---|---|---|
| Qué resuelve del caso | Avisa de incidentes reconocidos por Microsoft en los servicios del tenant (SharePoint, Exchange, Teams, Power Automate) | Detecta desde fuera que una URL no responde, incluso antes de que el proveedor lo reconozca |
| Costo y licenciamiento | Incluido en la suscripción de Microsoft 365; no requiere licencia adicional | Tiene plan gratuito con funciones limitadas (verificar condiciones vigentes antes de adoptarlo) |
| Dificultad de adopción para una organización como la del cliente | Requiere que alguien con rol administrativo en TI configure las notificaciones hacia el Centro de Contacto | Baja técnicamente, pero no puede monitorear páginas internas que exigen inicio de sesión sin configuración adicional |
| Soporte y disponibilidad en Colombia | Soporte de Microsoft ya contratado por la universidad | Servicio en línea sin soporte local; no hay relación contractual con la universidad |
| Riesgo que introduce | Depende de que Microsoft reconozca el incidente; no avisa de una falla que Microsoft aún no detecta | Es software externo no aprobado: choca con el principio "Evolución institucional, no reemplazo" de `plan-gobernanza.md` |

**Recomendación:** la Opción A. Ya está pagada, vive dentro de Microsoft 365 y respeta la restricción institucional que el cliente puso desde el Taller 0. La Opción B detecta antes, pero introducir una herramienta externa exigiría una ADR y la aprobación de la Dirección de Tecnología, y su ventaja (minutos de anticipación) no justifica ese costo de gobierno para un área de este tamaño.

## 6. Lección para el cliente real
El cliente tiene el mismo riesgo, y el TO-BE lo **aumenta**: hoy solo el Excel compartido vive en Microsoft 365, pero en la propuesta también viven ahí la Lista de SharePoint, el historial de auditoría y las alertas de SLA de Power Automate. Una caída como la de enero de 2023 detendría registro, seguimiento y alertas al mismo tiempo. El Plan de Continuidad de la fase Corto plazo (WP2, ADR #2) hoy solo cubre el Servidor Anexo Unisabana. Proponemos al comité ampliarlo con dos cosas de bajo esfuerzo: (1) que TI configure las notificaciones de estado del servicio de Microsoft 365 hacia Johanna y Alexander, y (2) un procedimiento de contingencia manual (registro temporal de casos y fecha de radicación, para pasarlos a la Lista cuando el servicio vuelva, sin perder el conteo del SLA). Este ajuste se tramitaría como una nueva ADR por el disparador "Incidente de seguridad / cambio de proveedor" de `procedimiento-cambios.md`.

---

## Referencias
- Microsoft. *Preliminary Post Incident Review — Global WAN issues* (tracking ID VSG1-B90). Historial de estado de Azure, enero 2023. Citado por las fuentes siguientes; el documento original no se consultó directamente.
- The Register. Artículo sobre la causa de la caída de Microsoft 365 (cambio de IP en router WAN). 30/01/2023. https://www.theregister.com/2023/01/30/wan_router_ip_address_change/ — consultado 05/11/2026.
- BleepingComputer. "Massive Microsoft 365 outage caused by WAN router IP change". 2023. https://www.bleepingcomputer.com/news/microsoft/massive-microsoft-365-outage-caused-by-wan-router-ip-change/
- Petri. "Microsoft 365 Outage Caused by WAN Router IP Address Change, Says Microsoft". 2023. https://petri.com/microsoft-365-outage-wan-router-ip-change/
- Fuente asistida por IA: Claude (Anthropic), noviembre 2026 — búsqueda de fuentes y redacción de este análisis.
