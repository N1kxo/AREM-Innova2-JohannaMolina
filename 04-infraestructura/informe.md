# 📄 Informe Técnico del Taller

## 🔖 Nombre del Taller
_Taller 4 - Mapa de Infraestructura y Diagnóstico Técnico (Parte 2 — Aplicación al Cliente Real)_

## 👥 Integrantes del equipo
- Nicolas Esteban Muñoz Sendoya (nico9ms@hotmail.com / N1kxo)
- Juan David Orozco Rodriguez (davidorozcoj1@gmail.com / DavidOrozcoJ)

## 🧠 Descripción general del trabajo
El objetivo del taller fue construir el mapa de infraestructura tecnológica que soporta hoy el proceso de gestión de PQRs ("Comuníquese con Nosotros") del Centro de Contacto de la Universidad de La Sabana, y realizar un diagnóstico técnico priorizado de sus debilidades. El ejercicio parte directamente del Taller 3 (modelo C4, C1/C2), que ya había identificado los componentes lógicos del sistema (Anexo Unisabana, el Consolidado de Casos en Excel, y los canales de correo, WhatsApp y llamadas); este taller baja esos mismos componentes a su capa de infraestructura física/lógica: dónde corren, en qué red, y con qué nivel de redundancia.

## 🔧 Proceso de desarrollo
Se aplicó la metodología de 5 pasos de la guía del taller:

1. **Identificar componentes**: a partir del C2 del Taller 3 se listaron los componentes de infraestructura: el servidor de Anexo Unisabana, el archivo de Excel/Excel Online del Consolidado de Casos, Exchange Online y Teams (Microsoft 365), la central telefónica/PBX, WhatsApp Business (Meta Cloud API), y los dispositivos de cada actor (Solicitante, Alexander, Unidad Responsable, Johanna).
2. **Agrupar por zona/capa**: se definieron cinco zonas — Clientes/Dispositivos, Red Institucional (campus de la universidad), Infraestructura On-Premise (administrada por la Dirección de Tecnología, fuera del control directa del área), Nube Microsoft 365, y Servicios Externos de Terceros (WhatsApp).
3. **Conectar los componentes**: se trazó el tráfico real: el Solicitante llega a Anexo Unisabana por la red institucional (web/app), a Exchange Online y WhatsApp por internet, y a la central telefónica por la red de voz; Alexander es el único componente que toca las cinco zonas, porque es quien transcribe manualmente los canales sin integración hacia el Excel.
4. **Marcar redundancia y capacidad**: se identificaron tres componentes sin redundancia conocida — el servidor de Anexo Unisabana, el archivo del Consolidado de Casos, y la red/VPN institucional — y un cuarto elemento crítico que no es un servidor sino un rol: Alexander, único responsable de los tres canales manuales.
5. **Diagnosticar y priorizar**: se construyó la tabla de diagnóstico técnico (ver abajo), clasificando cada hallazgo en disponibilidad, rendimiento/integridad o escalabilidad, y priorizándolo por impacto.

## 🧩 Análisis del modelo propuesto
- **Estructura del modelo:** a diferencia del caso base de RedExpress, donde la infraestructura es propia y desplegada por el equipo técnico de la empresa (API Gateways regionales, balanceador, base de datos distribuida), la infraestructura del cliente real es mayormente infraestructura de terceros que el equipo no administra ni tiene forma de auditar directamente: Anexo Unisabana lo opera la Dirección de Tecnología de la universidad, y Microsoft 365 lo opera Microsoft. Por eso varios de los riesgos del mapa se marcaron como "supuestos" (⚠️ instancia única) en lugar de hallazgos confirmados con métricas de infraestructura.
- **Representación de las necesidades del cliente:** el mapa hace explícito por qué persisten los problemas de la ficha de caracterización: Anexo Unisabana como plataforma de tickets es un posible punto único de falla para el canal web/app; el Consolidado de Casos en Excel, aunque vive en la nube redundante de Microsoft, es en la práctica un **único archivo compartido sin control de versiones**, lo que explica la falta de trazabilidad (Problema #1) y el riesgo de sobrescritura de datos; y Alexander, como único punto de registro manual de correo, WhatsApp y llamadas, es un cuello de botella humano que limita cuánto puede escalar el proceso sin agregar más personal o automatizar esos canales.
- **Diferencias con el caso base:** en RedExpress los riesgos son de rendimiento e infraestructura pura (latencia, escalabilidad geográfica de servidores); en el cliente real, el riesgo más severo no está en un servidor sino en un **proceso y un archivo compartido sin gobierno de datos**, lo cual es consistente con un sistema construido sobre herramientas ofimáticas y no sobre software a la medida.
- **Supuestos tomados:** se asumió que la red institucional y el servidor de Anexo Unisabana no tienen redundancia visible desde la perspectiva del área de Servicio al Cliente, ya que el equipo no tiene acceso a la documentación de infraestructura de la Dirección de Tecnología; esto se declara explícitamente como supuesto y no como hallazgo confirmado.

## 📈 Diagrama final entregado
- `entrega/mapa-final.drawio` — Mapa de infraestructura del cliente real

## 📋 Tabla de diagnóstico técnico

| Componente | Riesgo diagnosticado | Categoría | Impacto si ocurre | Prioridad |
|---|---|---|---|---|
| Servidor Anexo Unisabana (instancia única) | Punto único de falla | Disponibilidad | Se pierde el canal principal de radicación (web/app institucional) | Alta |
| Consolidado de Casos — Excel/Excel Online (archivo único compartido) | Sin control de versiones ni bloqueo de edición concurrente | Rendimiento / Integridad de datos | Sobrescrituras accidentales, pérdida de cambios, imposibilidad de auditar quién modificó qué (choca con ISO 9001) | Alta |
| Alexander (Gestor de Casos) | Único responsable de registrar manualmente correo, WhatsApp y llamadas | Escalabilidad | Ante ausencia o pico de volumen, los tres canales manuales se atrasan o se pierden | Alta |
| Red / VPN institucional | Posible enlace único hacia Anexo Unisabana y Microsoft 365 (supuesto, sin visibilidad de la topología real) | Disponibilidad | Una falla de red aislaría simultáneamente el correo, el Excel y Anexo Unisabana | Media |
| Central Telefónica / PBX | Redundancia no visible desde el área; gestionada externamente por la universidad | Disponibilidad | Pérdida del canal telefónico de radicación | Baja/Media |

## 🔍 Investigación complementaria
### Tema investigado:
Buenas prácticas de arquitectura de infraestructura híbrida (cloud + on-premise) y riesgos de gobernanza de datos en hojas de cálculo compartidas usadas como sistema de registro.

### Resumen:
La investigación sobre infraestructura híbrida confirma que distribuir el tráfico y las aplicaciones entre nube y on-premise reduce el riesgo de cuellos de botella y de puntos únicos de falla, pero solo cuando esa distribución es deliberada; en el caso del cliente, la combinación de Anexo Unisabana (on-premise) y Microsoft 365 (nube) no es una estrategia híbrida diseñada para redundancia, sino dos sistemas independientes que no se respaldan entre sí — si Anexo Unisabana falla, Microsoft 365 no puede sustituirlo como canal de radicación, y viceversa. La misma fuente señala que la disponibilidad mejora cuando se distribuye el procesamiento entre ambos entornos, lo que sugiere que el hallazgo de mayor prioridad (Anexo Unisabana sin redundancia visible) sería el primer punto a resolver en una eventual propuesta de arquitectura objetivo.

Sobre el riesgo del Consolidado de Casos, la literatura de gestión de datos coincide en que las hojas de cálculo compartidas, cuando se usan como sistema de registro central de un proceso de negocio, generan errores de entrada difíciles de detectar, múltiples versiones no reconciliadas y ausencia de controles de gobernanza — exactamente el patrón que el mapa de infraestructura expone en el componente marcado como riesgo de mayor prioridad junto con Anexo Unisabana, y que ya había sido señalado desde el Taller 3 como la causa raíz de la falta de trazabilidad exigida por ISO 9001.

## 📚 Referencias
- [1] Cassidy, R. "Top Cybersecurity Best Practices for the Hybrid Cloud". Exabeam, 2020. https://www.exabeam.com/blog/infosec-trends/top-cybersecurity-best-practices-for-the-hybrid-cloud/
- [2] Hewlett Packard Enterprise. "What Is Hybrid Cloud Infrastructure?". https://www.hpe.com/us/en/what-is/hybrid-cloud-infrastructure.html
- [3] Oracle. "10 Common Spreadsheet Risks and Solutions for Businesses". https://www.oracle.com/analytics/spreadsheet-risks
- Fuente asistida por IA: Claude (Anthropic), septiembre 2026 — apoyo en la estructuración del mapa de infraestructura, el diagnóstico priorizado y la redacción de este informe a partir del modelo C4 del Taller 3 y la ficha de caracterización del cliente.

---

_Este documento hace parte de la entrega del Taller 4 del curso AREM (Arquitectura Empresarial) - Universidad de La Sabana._
