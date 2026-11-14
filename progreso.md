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
- [ ] Los tres: acordar cómo interpretar la necesidad de intervención y redactar las conclusiones, aclarando los supuestos utilizados.

### Ronda 3 — revisar y entregar

- [x] Pilar: revisar citas, referencias, precisión del contenido y aspectos de seguridad; resolver las verificaciones bibliográficas pendientes y seleccionar los hallazgos principales para la infografía.
- [ ] Pilar: buscar en IEEE Xplore el DOI de AlertGuardian en las actas de ASE 2025 (no aparece en los registros consultados).
- [ ] Martina: comprobar que el proyecto GNS3 se pueda importar y reproducir; preparar la demo de 3–5 minutos y verificar que funcione sin internet; dejar listas las configuraciones, capturas y registros.
- [ ] Tomás: integrar y dar formato al paper, armar el póster A1 con los aportes del grupo y exportar ambos a PDF.
- [ ] Tomás: actualizar README, bitácora y checklist; comprobar que estén todos los entregables y sus archivos editables.
- [ ] Los tres: revisar la versión final, ensayar la exposición y comprobar el contenido del único PR de entrega.

## Notas

- El `.gitignore` excluye los archivos `*.log`. Guardamos los registros de la simulación como `.txt` para incluirlos en el repositorio.
- `documento/referencias.md` conserva por ahora la numeración de `documento/fuentes-investigacion.md`. Al integrar el paper debemos comprobar que cada cita corresponda a la fuente correcta.
- NIST SP 800-92r1 sigue como borrador al 14/11/2026. La guía de 2006 se cita como [16] y el borrador de 2023 como [9]. Volver a fijarse antes de entregar.
- AlertGuardian se cita con la versión de conferencia (ASE 2025) y el preprint de arXiv. Falta el DOI de las actas.
- Al integrar el paper, usar `documento/referencias.md` (16 fuentes) como lista final y sacar las listas parciales de cada archivo.
- Mantener los registros originales y señalar las limitaciones de la experiencia al presentar los resultados.