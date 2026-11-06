# Reflexión Individual

## Nombre del Estudiante
Nicolás Esteban Muñoz Sendoya

## Rol en el equipo
Responsable técnico de las vistas de arquitectura actual (C4, infraestructura, seguridad con STRIDE y normatividad, talleres 3 a 6) y revisor de coherencia entre entregables y diapositivas en el cierre del proyecto.

## Aprendizajes Clave

- Modelar con los documentos reales del cliente y no con supuestos. Mi primera versión del C4 usaba herramientas que el cliente no tiene (Google Forms y Google Sheets); al contrastarla con sus documentos la rehice sobre Anexo Unisabana, el Excel compartido y Microsoft 365.
- Una restricción del cliente cambia el diseño. Que el Centro de Contacto no pudiera instalar software externo nos llevó a elegir una Lista de SharePoint y no Dataverse/Power Apps.
- Decidir con criterios explícitos. La matriz ponderada del Taller 7 mostró que la opción con mayor puntaje no era la correcta, porque fallaba en lo más importante para el cliente: la trazabilidad.
- Las vistas de arquitectura se conectan entre sí. El mismo Excel compartido aparece como riesgo en infraestructura, como amenaza en STRIDE y como brecha en normatividad.

## Retos Superados

- Contradicciones entre talleres que señaló el profesor. Por ejemplo, Anexo Unisabana aparecía on-premise en el mapa de infraestructura y dentro de Microsoft 365 en el DFD de STRIDE. Las corregimos alineando las zonas del DFD con las del mapa.
- Nombres propios como componentes de la arquitectura. Un funcionario aparecía como componente en repositorios públicos, y eso es un problema de modelado (se modelan roles, no personas) y de datos personales (Ley 1581). Los reemplazamos por roles.
- Presentar en poco tiempo. Los compañeros nos pidieron menos texto en las diapositivas y mejor ritmo; para el cierre recortamos contenido en lugar de hablar más rápido.

## Áreas por Mejorar

- Prevenir las incoherencias desde el inicio, no solo detectarlas al final. En futuros trabajos intentaré mantener un glosario único de nombres, roles y zonas para las entregas que lo necesiten, y hacer una revisión cruzada antes de cada entrega.
- Validar con el cliente antes de modelar. Varios supuestos de infraestructura no los pudimos confirmar con la Dirección de Tecnología en este caso; de ahora en adelante intentaré agendar esas dudas desde el inicio de cada entrega, con el cliente para el que vaya a diseñar una solución.
- Manejo del tiempo y de las diapositivas en las sustentaciones, con ensayos cronometrados y menos texto por diapositiva.
- Hacer visible mi trabajo en el repositorio. El profesor notó que los commits de este proyecto venían de una sola cuenta; aunque trabajamos los dos en conjunto, parecía que solo uno trabajaba en los repositorios. En adelante subiré mis aportes desde mi cuenta o intentaré repartir mejor el trabajo para que se refleje en los commits.

## Contribución Personal

Mi mayor aporte fue detectar y corregir incoherencias entre los entregables. Al inicio nos pasaba que talleres distintos decían cosas que no tenían sentido entre sí (la herramienta del cliente, las zonas de infraestructura, los nombres). Revisé la consistencia entre talleres y diapositivas, y esa revisión evitó que el cliente recibiera una propuesta construida sobre supuestos. También usé la IA como herramienta de apoyo, no como reemplazo: me ayudó a redactar y a revisar, pero siempre contrasté sus resultados con los documentos reales del cliente. Así fue como detecté el error del primer C4.

---

_Esta reflexión individual hace parte de la entrega final del curso AREM - Arquitectura Empresarial - Universidad de La Sabana._