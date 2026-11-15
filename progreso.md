# Bitácora del grupo — TP4

Registro de los aportes de cada integrante, las decisiones tomadas y las tareas pendientes.

## Integrantes

| Nombre | Usuario de GitHub | Rol en el grupo |
|--------|-------------------|-----------------|
| Pilar Giannelli | `pilargiannelli` | Búsqueda de fuentes, investigación, revisión académica y selección de hallazgos para el póster |
| Tomás Terruli | `TomasTerruli` | Metodología y análisis de resultados, integración del paper, póster y revisión final del repositorio |
| Martina Castellani | `Marticaste144` | Armado y ejecución de la simulación, introducción, estructura, referencias y preparación de la demo |

## Proyecto

- **Modalidad:** B — Artículo de Revisión del Estado del Arte, con una mini simulación.
- **Título:** De las métricas a las acciones: calidad de las alertas y detección de fallas en servicios.
- **Tema:** Observabilidad de servicios: criterios de alertado para detectar fallas y reducir notificaciones innecesarias.
- **Pregunta de investigación:** ¿Cómo influyen los criterios de activación de alertas en la cantidad de notificaciones y en el tiempo de detección de una falla?
- **Simulación:** red pequeña en GNS3 con dos equipos VPCS conectados a un switch. Comprobamos la conectividad con ping y comparamos avisar ante el primer fallo con esperar tres fallos consecutivos.

## Registro de avance

| Fecha | Qué se hizo | Quién | Commit |
|-------|-------------|-------|--------|
| 09/11/2026 | Preparé el borrador de la introducción con una referencia oficial de OpenTelemetry en `documento/introduccion.md`. | Martina | `0bdfa90` |
| 09/11/2026 | Revisé y fusioné el PR #1 de la introducción. | Tomás | `855296a` |
| 12/11/2026 | Delimité el tema y actualicé el README con título, integrantes, modalidad, objetivo, alcance y entregables. Creé `documento/alcance.md` y reuní 14 fuentes con sus datos, utilidad y estado de verificación en `documento/fuentes-investigacion.md`. | Pilar | `61eb53c` |
| 13/11/2026 | Armé la red en GNS3 y ejecuté nueve pruebas de conectividad. Guardé los registros, dos capturas, el proyecto portable y las instrucciones para reproducirlo. | Martina | `65f7a43` |
| 13/11/2026 | Revisé los nueve registros y documenté la metodología y la comparación de criterios en `demo/analisis.md`. Presenté `sostenida-01.txt` por separado como prueba exploratoria y expresé la demora en comprobaciones, porque los registros no tienen marcas temporales. | Tomás | `801dac3` |
| 13/11/2026 | Redacté el estado del arte, la tabla comparativa, las innovaciones y los desafíos de seguridad en `documento/estado-del-arte.md`. Actualicé la verificación de las fuentes y agregué la documentación de Alertmanager. Revisé los registros y dejé opciones para interpretar qué interrupciones requieren intervención en `documento/revision-protocolo.md`. | Pilar | `d311d8a` |
| 14/11/2026 | Adapté la introducción al enfoque de alertas, preparé la estructura del artículo, actualicé el alcance según las pruebas realizadas y organicé las referencias provisionales. | Martina | `aca7630`, `bae6c3e` |
| 14/11/2026 | Preparé el borrador de la metodología bibliográfica a partir del relevamiento documentado. Queda pendiente la revisión de Pilar. | Martina | `dd6561e` |
| 14/11/2026 | Revisión académica. Completé en `documento/metodologia-bibliografica.md` cómo buscamos y elegimos las fuentes. Comprobé que las citas del artículo coincidan con `documento/referencias.md` y corregí la lista de la introducción. Separé la guía NIST de 2006 ([16]) del borrador de 2023 ([9]), que sigue sin versión final. Completé AlertGuardian distinguiendo conferencia y preprint, y verifiqué en las fuentes originales las cifras de COLA, AlertGuardian y Zhao *et al.* En `documento/estado-del-arte.md` aclaré que Prometheus evalúa las reglas y Alertmanager gestiona las notificaciones, y saqué afirmaciones absolutas de la tabla. Elegí cinco hallazgos para el póster en `poster/hallazgos.md`. | Pilar | rama `grupo-07` |
| 14/11/2026 | Definí la bibliografía que se cita en mi sección (estado del arte, innovaciones y desafíos): libro de SRE (caps. 4 y 6), SRE Workbook (cap. 5), documentación de Prometheus, RFC 792 y Tariq *et al.* (2025). Ajusté las citas en `documento/estado-del-arte.md`, `documento/revision-protocolo.md` y `poster/hallazgos.md`, ordené `documento/referencias.md` y dejé en `documento/fuentes-investigacion.md` la correspondencia con el registro de fuentes. | Pilar | rama `grupo-07` |
| 14/11/2026 | Ajustes académicos en `documento/estado-del-arte.md`: desarrollé las innovaciones emergentes (COLA, AlertGuardian y Tariq *et al.*) explicando qué problema resuelven y por qué son nuevas; amplié la seguridad de los registros con medidas de integridad, acceso, conservación y hora confiable, con respaldo de RFC 5424 y NIST SP 800-92; y rehice la tabla comparativa separando activación de entrega y sin expresiones absolutas. Sumé esas cuatro fuentes, ya verificadas, a `documento/referencias.md` (quedan diez) y actualicé la cantidad en `documento/metodologia-bibliografica.md`. | Pilar | rama `grupo-07` |
| 14/11/2026 | Integré el artículo en `documento/articulo-grupo07.docx` y su PDF (portada + 7 páginas, formato de artículo académico en Times New Roman): resumen de 150 palabras, introducción, metodología (bibliográfica y GNS3), estado del arte, innovaciones emergentes, oportunidades y desafíos con seguridad, resultados y discusión con tres tablas y la topología, conclusiones y las 10 referencias finales de Pilar, incluidas sus ampliaciones de innovaciones (COLA, AlertGuardian y Tariq *et al.*), seguridad de los registros (NIST SP 800-92 y RFC 5424) y la tabla que separa activación y entrega. Recorté repeticiones y unifiqué las citas en formato IEEE por orden de aparición. Agregué la justificación de N = 3 y las fallas intermitentes en la discusión. Armé el póster A1 como infografía en `poster/poster-grupo07.html` y su PDF: el problema, la pregunta, la topología, los dos criterios, un mapa de las 240 comprobaciones generado desde los registros, los hallazgos de la bibliografía, las conclusiones y un QR al repositorio. Actualicé el README (entregables, resultado principal e instrucciones de la demo), la bitácora y el checklist (`checklist-entrega.md`). | Tomás | rama `grupo-07` |
| 15/11/2026 | Completé la investigación pedida por el profesor en `documento/estado-del-arte.md`: analicé los cuatro trabajos recientes (COLA, Zha *et al.*, AlertGuardian y Ndichu *et al.*) con problema, propuesta, evaluación, resultados, limitaciones y relación con el tema, y armé su tabla comparativa separando sus resultados de los nuestros. Documenté Keep y PagerDuty AIOps con sus fuentes oficiales, distinguiendo en Keep el núcleo libre de lo empresarial. Corregí la descripción de Prometheus y Alertmanager. Sumé siete fuentes a `documento/referencias.md` (quedan diecisiete), actualicé `documento/fuentes-investigacion.md`, la cantidad en `documento/metodologia-bibliografica.md` y dejé en `poster/hallazgos.md` el bloque para el póster. | Pilar | rama `grupo-07` |
| 15/11/2026 | Actualicé el README de la demo para describir el análisis realizado, aclarar los criterios C1 y C2, documentar la verificación del proyecto y relacionar la práctica con las investigaciones y productos revisados. | Martina | Consultar historial de Git |
| 15/11/2026 | Integré las correcciones del profesor en el artículo (`documento/articulo-grupo07.docx` y su PDF, portada + 7 páginas): los cuatro trabajos recientes con su análisis y la Tabla 2 comparativa, la Tabla 3 de Keep y PagerDuty AIOps, la distinción entre Prometheus (evalúa reglas) y Alertmanager (gestiona notificaciones), la sección 6.2 que separa lo que probamos de lo estudiado por otros, y el resumen y las conclusiones actualizados. Unifiqué las 17 citas en formato IEEE. Actualicé el póster con un bloque de investigaciones 2024–2026 y productos reales y un recuadro visible de limitaciones, y corregí los nombres de las fuentes tipográficas. Revisé ambos PDF (tablas, citas y QR). Actualicé el README, el checklist y la bitácora. | Tomás | rama `grupo-07` |

## Decisiones tomadas

- Elegimos la modalidad B, con una mini simulación como complemento de la revisión bibliográfica.
- El título, la pregunta y los objetivos están en `documento/alcance.md`.
- Trabajamos en un único fork grupal, sobre la rama compartida `grupo-07`, con commits propios de cada integrante. Al finalizar abriremos un único PR al repositorio del profesor y lo dejaremos abierto para su revisión.
- Martina se encarga de la práctica en GNS3. Tomás documenta la metodología y analiza los registros.
- En la ronda 3, Pilar realiza la revisión académica, Martina prepara la demo y Tomás integra el paper, arma el póster y revisa los entregables.
- Aplicamos los dos criterios manualmente sobre los mismos registros. No implementamos alertas automáticas.
- El criterio 1 avisa ante el primer fallo y el criterio 2 ante tres fallos consecutivos.
- Contamos una notificación por racha de fallos para cada criterio que alcanza su condición de activación. La recuperación corresponde a la primera respuesta exitosa y no se cuenta como otra notificación.
- No definimos antes de ejecutar las pruebas qué interrupciones requerían intervención. Por ahora presentamos interpretaciones alternativas, sin clasificar automáticamente los avisos como falsas alarmas.
- Conservamos `sostenida-01.txt` como registro exploratorio y lo presentamos fuera de la comparación principal. Esta separación se decidió después de ejecutar las pruebas.
- La comparación principal incluye ocho pruebas. También mostramos qué cambia al incluir la exploratoria.
- La demora de detección se expresa en comprobaciones, no en segundos, porque los registros no tienen marcas temporales.
- El artículo final numera las citas en formato IEEE por orden de aparición. Los archivos de trabajo conservan la numeración de `documento/referencias.md`.
- N = 3 se presenta en el artículo como valor ilustrativo, sin buscar un valor óptimo.
- Las pruebas evalúan conectividad ICMP, no el funcionamiento de una aplicación. Los resultados no se generalizan a redes reales.

## Pendientes

### Ronda 1 — organizar y preparar

- [x] Pilar: actualizar README, bitácora y alcance; reunir las fuentes iniciales.
- [x] Martina: diseñar la red en GNS3 y documentar el procedimiento en `demo/README.md`, tarea que inicialmente estaba asignada a Tomás.
- [x] Martina: adaptar la introducción al objetivo, preparar la estructura del artículo y crear el listado provisional de referencias.
- [x] Martina: preparar el borrador de la metodología bibliográfica en `documento/metodologia-bibliografica.md`.
- [x] Pilar: revisar que la metodología bibliográfica describa la búsqueda y selección que realizó.

### Ronda 2 — investigar y obtener resultados

- [x] Pilar: redactar el estado del arte, las innovaciones, los desafíos y la seguridad; revisar las fuentes y el protocolo, dejando indicadas las verificaciones pendientes.
- [x] Martina: realizar nueve ejecuciones: tres normales, tres de corte breve, dos de corte sostenido y una exploratoria; guardar registros, capturas y proyecto.
- [x] Tomás: documentar la metodología y comparar las notificaciones, las rachas detectadas y la demora de detección en `demo/analisis.md`.
- [x] Martina: preparar la introducción, la estructura y las referencias provisionales del artículo.
- [ ] Los tres: revisar y aprobar las conclusiones redactadas en el artículo (sección 6), que explican cuándo conviene cada criterio sin elegir una única definición de intervención.

### Ronda 3 — revisar y entregar

- [x] Pilar: revisar citas, referencias, precisión del contenido y aspectos de seguridad; resolver las verificaciones bibliográficas pendientes y seleccionar los hallazgos principales para la infografía.
- [ ] Martina: comprobar que el proyecto GNS3 se pueda importar y reproducir; preparar la demo de 3–5 minutos y verificar que funcione sin internet; dejar listas las configuraciones, capturas y registros.
- [x] Tomás: integrar y dar formato al paper, armar el póster A1 con los aportes del grupo y exportar ambos a PDF.
- [x] Tomás: actualizar README, bitácora y checklist; comprobar que estén todos los entregables y sus archivos editables.
- [x] Tomás: integrar las correcciones del profesor en el artículo y el póster (trabajos 2024–2026, Keep y PagerDuty AIOps) y actualizar README, checklist y bitácora.
- [ ] Los tres: leer las partes centrales de los cuatro trabajos y revisar el artículo y el póster del 15/11.
- [ ] Los tres: ensayar la demo completa en la computadora de la presentación y la exposición.
- [ ] Tomás: comprobar el único PR de entrega después de los últimos commits.

## Notas

- El `.gitignore` excluye los archivos `*.log`. Guardamos los registros de la simulación como `.txt` para incluirlos en el repositorio.
- `documento/referencias.md` tiene la lista final del artículo (17 fuentes, con numeración propia). La correspondencia con la numeración de `documento/fuentes-investigacion.md` está al principio de ese archivo.
- El artículo final (`documento/articulo-grupo07.docx` y su PDF) cita las 17 fuentes numeradas en formato IEEE por orden de aparición, así que los números difieren de `referencias.md`: [1] OpenTelemetry, [2] SRE, [3] Thurgood, [4] RFC 792, [5] reglas de Prometheus, [6] Alertmanager, [7] Tariq, [8] AlertGuardian, [9] COLA, [10] Zha, [11] Ndichu, [12] y [13] Keep, [14] y [15] PagerDuty, [16] NIST SP 800-92 y [17] RFC 5424.
- Mantener los registros originales y señalar las limitaciones de la experiencia al presentar los resultados.