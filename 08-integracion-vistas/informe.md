# 📄 Informe Técnico del Taller

## 🔖 Nombre del Taller
_Taller 8 - Integración de Vistas de Arquitectura (Parte 2 — Aplicación al Cliente Real)_

## 👥 Integrantes del equipo
- Nicolas Esteban Muñoz Sendoya (nico9ms@hotmail.com — GitHub: N1kxo)
- Juan David Orozco Rodríguez (davidorozcoj1@gmail.com — GitHub: DavidOrozcoJ)

## 🧠 Descripción general del trabajo
Este taller integra, en un solo tablero coherente, las cinco vistas de arquitectura ya construidas en los talleres anteriores para el proceso real del cliente — **gestión y seguimiento de casos PQRSF ("Comuníquese con Nosotros")** del Centro de Contacto de la Universidad de La Sabana (contacto: Johanna Andrea Molina) — mostrando no solo cómo funciona el proceso hoy (AS-IS), sino cómo queda después de aplicar la solución priorizada en el Taller 7 (TO-BE), siguiendo la metodología de 5 pasos de la guía del taller.

## 🔧 Proceso de desarrollo
Se siguieron los 5 pasos de la guía, tomando como única fuente los entregables ya producidos y versionados por el equipo en los Talleres 0 a 7 (sin inventar elementos nuevos):

1. **Organizar las 5 capas**: Negocio, Aplicaciones, Información, Infraestructura y Seguridad, representadas como bandas horizontales en el tablero (`tablero-integrado-cliente.drawio`).
2. **Ubicar los elementos de cada vista sin conectar**: se trasladó cada elemento tal como aparece en su taller de origen —
   - *Negocio*: carriles y actividades del BPMN (Taller 1).
   - *Aplicaciones*: contenedores del C2 (Taller 3) + los componentes NUEVO del TO-BE (Taller 7).
   - *Información*: las 6 entidades del ERD (Taller 2): `TICKET`, `USUARIO_SOLICITANTE`, `CANAL`, `CATEGORIA`, `AREA_RESPONSABLE`, `REGISTRO_AUDITORIA`.
   - *Infraestructura*: las zonas y componentes del mapa de infraestructura (Taller 4) + el `NUEVO: Plan de Continuidad (BCP/DRP)` del Taller 7.
   - *Seguridad*: las amenazas T1–T8 del análisis STRIDE (Taller 5) + las mitigaciones NUEVO del TO-BE (Taller 7).
3. **Trazar un hilo de negocio completo**: en vez de conectar las 5 capas completas de una vez (lo que satura el tablero, según advierte la guía), se trazó un único hilo de extremo a extremo — **"Registro, enrutamiento y cierre de un caso PQRSF"** — desde la radicación multicanal hasta el registro de auditoría y el cierre, incluyendo las conexiones NUEVO que introduce el TO-BE.
4. **Redactar la narrativa de coherencia** (sección siguiente), explicando por qué cada conexión tiene sentido con base en las decisiones ya tomadas en cada taller, no solo describiendo el diagrama.
5. **Verificar que ningún elemento quede huérfano**, mediante la tabla de verificación incluida más abajo.

## 🧩 Análisis del modelo propuesto

### Cómo se estructura el modelo entregado
El tablero (`tablero-integrado-cliente.drawio`) usa 5 bandas horizontales (una por capa) y, dentro de cada una, dos tipos de elementos diferenciados por color: los existentes **AS-IS** (azul, o rojo cuando ya fueron señalados como un riesgo en los talleres 4/5/6) y los **NUEVO** que introduce el TO-BE del Taller 7 (verde, borde punteado). Las flechas sólidas trazan relaciones AS-IS; las flechas verdes punteadas trazan relaciones NUEVO o de mitigación. Una leyenda en la esquina inferior izquierda del tablero resume esta convención.

### Narrativa de coherencia — hilo "Registro, enrutamiento y cierre de un caso PQRSF"
- El proceso de **Negocio** inicia con la **radicación multicanal** (Taller 1, carril Solicitante): el caso puede llegar por el sitio web/app Unisabana —que entra directo a **Anexo Unisabana**— o por correo/WhatsApp/llamada —que entra a los contenedores de **registro manual** del C2 (Taller 3)—, porque esos canales no tienen ninguna integración automática, tal como documentó el Taller 3.
- Desde ahí, **Anexo Unisabana** crea el `TICKET` y asocia al `USUARIO_SOLICITANTE` (Información, Taller 2); el registro manual clasifica el `CANAL` por el que llegó el caso. La actividad de **clasificación y enrutamiento** (Negocio) usa la compuerta XOR de nivel de reporte del Taller 1 para asignar la `CATEGORIA` y el `AREA_RESPONSABLE` correspondiente — exactamente la misma lógica de enrutamiento N1/N2/N3 descrita en el informe del Taller 1.
- Tanto Anexo Unisabana como el registro manual terminan consolidándose en el **Excel compartido** (Aplicaciones), porque — como explica el Taller 3 — Anexo Unisabana no exporta todos los campos requeridos y ese Excel es, en la práctica, la verdadera fuente de datos del proceso. Ese Excel corre sobre **SharePoint/Excel Online** y sobre el **Servidor Anexo Unisabana** (Infraestructura, Taller 4), dos componentes que el propio Taller 4 ya marcó con riesgo ⚠️ de disponibilidad/rendimiento, y que el STRIDE del Taller 5 traduce en las amenazas **T2/T3 (Tampering/Repudiation)** y **T4 (Information Disclosure)**: el mismo Excel sin control de versiones que el Taller 4 señala como riesgo de infraestructura es, en Seguridad, el activo donde se materializan esas tres amenazas — la coherencia entre capas no es casual, es la misma debilidad vista desde tres ángulos distintos (dato, infraestructura, seguridad).
- La actividad de **cierre** (Negocio) es, según el Taller 1, el "punto crítico" — actualizar el estado a Cerrado directamente en Excel sin que quede registro de quién ni cuándo —, lo cual el modelo conecta con una flecha roja hacia el Excel (riesgo) y, al mismo tiempo, con una flecha NUEVO verde hacia **Power Automate**, que es la solución propuesta ya en el Taller 1 para ese mismo punto crítico.
- Aquí entra el **TO-BE del Taller 7**: la solución priorizada por la matriz de decisión ponderada (Trazabilidad 35%, Costo 25%, Complejidad 20%, Tiempo 20%) fue la **Opción B — Lista de SharePoint**, no la Opción A (historial nativo de Excel), precisamente porque la Opción A quedó eliminada por el criterio eliminatorio de Trazabilidad. Por eso el tablero traza la migración del Excel hacia la **Lista de SharePoint — Consolidado PQRSF (NUEVO)**, que junto con **Power Automate (NUEVO)** cierra el ciclo: registra el `REGISTRO_AUDITORIA` automáticamente (resolviendo el punto crítico del Taller 1) y dispara las alertas de SLA (resolviendo el problema #4 de la ficha de caracterización, ya recogido en la Visión del Taller 0).
- En **Seguridad**, esas dos piezas NUEVO habilitan directamente las mitigaciones documentadas en el Taller 7, sección 3.3: el historial + permisos por rol de la Lista de SharePoint mitiga **T2/T3/T4/T7**, y las alertas de Power Automate mitigan **T5**. El propio Taller 7 es explícito en que **T1 (Spoofing), T6 (Elevation of Privilege) y T8 (Information Disclosure en el sitio web)** no quedan resueltas por este TO-BE y siguen como trabajo futuro — así se muestra en el tablero, sin pretender una cobertura que el equipo nunca reclamó.
- Finalmente, el **Plan de Continuidad (BCP/DRP, NUEVO)** para el Servidor Anexo Unisabana en Infraestructura responde directamente al riesgo de disponibilidad "instancia única, sin redundancia" que el Taller 4 calificó como Alto — es el Work Package 2 ("Continuidad del servicio") del Taller 7, un quick win independiente de la migración a SharePoint.

### Qué supuestos se tomaron
- Se mantiene el supuesto, ya declarado en los Talleres 2 y 3, de que el acceso al Excel compartido y a Anexo Unisabana está limitado al tenant institucional de Microsoft 365 (no expuesto públicamente).
- Se representa un solo hilo de negocio completo (registro → enrutamiento → cierre) en lugar de las 5 capas totalmente interconectadas, siguiendo la recomendación explícita de la guía del taller de no saturar el tablero; los demás elementos de cada capa (p. ej. Reportes Semestrales, Microsoft Purview) quedan ubicados en su capa y verificados como no huérfanos, aunque no formen parte del hilo principal trazado.
- Se dejan señaladas, sin resolver todavía en este tablero, dos inconsistencias menores entre entregables previos del propio equipo: (a) el informe del Taller 2 describe las entidades con los nombres "Caso PQRSF / HistorialCambios / Reapertura", mientras que el archivo `modelo-final-er.drawio` efectivamente entregado usa `TICKET / REGISTRO_AUDITORIA` y no incluye una entidad `Reapertura` separada — este tablero usa los nombres del `.drawio` por ser el artefacto realmente entregado y versionado; (b) la carpeta `01-bpmn/` no contiene el archivo `modelo-final.drawio` referenciado en su propio informe, por lo que la capa de Negocio de este tablero se reconstruyó a partir de la descripción textual del informe del Taller 1 (carriles, compuerta XOR, punto crítico), no del diagrama visual. Se recomienda que el equipo verifique y reincorpore ese archivo antes de la entrega final del Taller 9.

## 📈 Diagrama final entregado
- [`tablero-integrado-cliente.drawio`](./tablero-integrado-cliente.drawio) — tablero integrado con las 5 capas (Negocio, Aplicaciones, Información, Infraestructura, Seguridad), AS-IS + TO-BE (NUEVO), y el hilo "Registro, enrutamiento y cierre de un caso PQRSF" trazado verticalmente entre capas.

## ✅ Tabla de verificación (ningún elemento huérfano)

| Elemento | Capa | ¿Conectado hacia arriba? | ¿Conectado hacia abajo? |
|---|---|---|---|
| Radicación multicanal | Negocio | — (inicio del hilo) | Sí → Anexo Unisabana / Registro manual |
| Clasificación y enrutamiento (XOR) | Negocio | Sí ← Anexo Unisabana | Sí → Categoría / Área Responsable |
| Atención, cierre y registro de auditoría | Negocio | Sí ← Clasificación | Sí → Excel (riesgo) / Power Automate (NUEVO) |
| Anexo Unisabana | Aplicaciones | Sí ← Radicación | Sí → Servidor Anexo Unisabana / TICKET |
| Registro manual (correo/WhatsApp/llamadas) | Aplicaciones | Sí ← Radicación | Sí → Excel / WhatsApp Business |
| Consolidado de Casos — Excel ⚠️ | Aplicaciones | Sí ← Registro manual / Anexo Unisabana / Cierre | Sí → Excel Online / T2-T3-T4-T7 |
| NUEVO: Lista de SharePoint | Aplicaciones | Sí ← migración del Excel | Sí → Excel Online / mitigación de seguridad |
| NUEVO: Power Automate | Aplicaciones | Sí ← Cierre del caso / Lista de SharePoint | Sí → Microsoft 365 / REGISTRO_AUDITORIA / mitigación T5 |
| Reportes Semestrales | Aplicaciones | Sí ← Excel | — (fin de su hilo: salida informativa) |
| TICKET | Información | Sí ← Anexo Unisabana | — (entidad de datos, fin de su hilo) |
| USUARIO_SOLICITANTE | Información | Sí ← Radicación | — |
| CANAL | Información | Sí ← Registro manual | — |
| CATEGORIA | Información | Sí ← Clasificación | — |
| AREA_RESPONSABLE | Información | Sí ← Clasificación | — |
| REGISTRO_AUDITORIA | Información | Sí ← Power Automate (NUEVO) | — |
| Servidor Anexo Unisabana ⚠️ | Infraestructura | Sí ← Anexo Unisabana | Sí → T5 (riesgo) / NUEVO BCP/DRP |
| SharePoint/Excel Online ⚠️ | Infraestructura | Sí ← Excel / Lista de SharePoint | Sí → T2/T3/T4 (riesgo) |
| Red/VPN Institucional ⚠️ | Infraestructura | Sí ← Radicación | — (riesgo de disponibilidad documentado en Taller 4) |
| Microsoft 365 (Nube) | Infraestructura | Sí ← Power Automate | — |
| WhatsApp Business ⚠️ | Infraestructura | Sí ← Registro manual | Sí → T5 (transferencia internacional) |
| NUEVO: Plan de Continuidad (BCP/DRP) | Infraestructura | Sí ← Servidor Anexo Unisabana | — (control de mitigación, fin de su hilo) |
| T2/T3/T7 — Tampering/Repudiation | Seguridad | Sí ← Excel Online | Sí → mitigado por NUEVO historial SharePoint |
| T4 — Information Disclosure | Seguridad | Sí ← Excel Online | Sí → mitigado por NUEVO historial SharePoint |
| T5 — Denial of Service | Seguridad | Sí ← Servidor Anexo Unisabana / WhatsApp | Sí → mitigado por NUEVO alertas Power Automate |
| NUEVO: Historial + permisos por rol | Seguridad | Sí ← Lista de SharePoint | Sí → mitiga T2/T3/T4/T7 |
| NUEVO: Alertas automáticas de SLA | Seguridad | Sí ← Power Automate | Sí → mitiga T5 |
| T1, T6, T8 (sin mitigar) | Seguridad | Sí ← (amenazas del Taller 5 no cubiertas) | — (explícitamente fuera de alcance de este TO-BE, Taller 7) |

Ningún elemento del tablero queda sin al menos una conexión documentada (hacia arriba, hacia abajo, o ambas), cumpliendo el Paso 5 de la guía del taller.

## 📋 Tabla de actores, entidades o componentes (resumen de origen)

| Nombre del elemento | Capa | Taller de origen |
|---|---|---|
| Radicación, clasificación y cierre del caso | Negocio | Taller 1 (BPMN) |
| Anexo Unisabana, Excel, Registro manual, Reportes Semestrales | Aplicaciones | Taller 3 (C4 - C2) |
| Lista de SharePoint, Power Automate (NUEVO) | Aplicaciones | Taller 7 (Opportunities & Solutions, WP1) |
| TICKET, USUARIO_SOLICITANTE, CANAL, CATEGORIA, AREA_RESPONSABLE, REGISTRO_AUDITORIA | Información | Taller 2 (ERD) |
| Servidor Anexo Unisabana, Excel Online, Red/VPN, M365, WhatsApp Business | Infraestructura | Taller 4 (Mapa de infraestructura) |
| Plan de Continuidad BCP/DRP (NUEVO) | Infraestructura | Taller 7 (WP2 — Continuidad del servicio) |
| T1–T8 (amenazas STRIDE) | Seguridad | Taller 5 (STRIDE) |
| Mitigaciones NUEVO (historial, alertas) | Seguridad | Taller 7 (sección 3.3) |

## 🔍 Investigación complementaria
### Tema investigado:
Equivalencia entre el método de integración de vistas usado en este taller y la noción de "viewpoints" y "views" en ArchiMate / TOGAF, y buenas prácticas para evitar diagramas de arquitectura sobrecargados ("model clutter").

### Resumen:
La guía de ArchiMate y el propio TOGAF ADM coinciden en que una arquitectura empresarial no se comunica con un único diagrama "maestro" que conecte todo, sino con varias vistas (viewpoints) enfocadas en una audiencia o preocupación específica, integradas conceptualmente mediante los mismos elementos compartidos entre vistas (lo que aquí se logró usando los mismos nombres de entidades, contenedores y amenazas que ya existían en cada taller, en vez de inventar nuevos). La práctica de trazar un solo "hilo" de negocio de extremo a extremo antes de intentar conectar todas las capas —seguida en este taller— es consistente con la recomendación de evitar el sobrecargue visual ("clutter") de los diagramas de arquitectura: mostrar un recorrido completo y legible comunica mejor la coherencia del diseño que una malla de conexiones simultáneas entre decenas de elementos.

## 📚 Referencias
Ver [`referencias.md`](./referencias.md).

---

_Este documento hace parte de la entrega del Taller 8 (Integración de Vistas de Arquitectura) del curso AREM (Arquitectura Empresarial) - Universidad de La Sabana._
