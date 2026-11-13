# Bitácora del grupo — TP4

> Registrar acá el avance: qué hizo cada integrante, decisiones tomadas y pendientes.
> Esto ayuda a coordinar el trabajo y deja evidencia de la participación de todos.

---

## 👥 Integrantes

| Nombre | Usuario de GitHub | Rol en el grupo |
|--------|-------------------|-----------------|
| Pilar Giannelli | `pilargiannelli` | Búsqueda de fuentes, investigación y revisión |
| Tomás Terruli | `TomasTerruli` | documentación de la metodología y análisis de resultados. |
| Martina Castellani | `Marticaste144` | armado y ejecución de la simulación, organización del artículo y póster. |

##  Proyecto

- **Modalidad:** B — Artículo de Revisión del Estado del Arte, con una mini simulación
- **Título:** De las métricas a las acciones: calidad de las alertas y detección de fallas en servicios
- **Tema:** Observabilidad de servicios: criterios de alertado para detectar fallas y reducir notificaciones innecesarias
- **Pregunta de investigación:** ¿Cómo influyen los criterios de activación de alertas en la cantidad de notificaciones y en el tiempo de detección de una falla?
- **Simulación:** red chica en GNS3 donde comprobamos la conectividad con ping; comparamos avisar con el primer fallo contra esperar varios fallos seguidos.

## Registro de avance

| Fecha | Qué se hizo | Quién | Commit |
|-------|-------------|-------|--------|
| 09/11/2026 | Borrador de la introducción con una referencia oficial (OpenTelemetry) en `documento/introduccion.md` | Martina | `0bdfa90` |
| 09/11/2026 | Revisión y merge del PR #1 (introducción) | Tomás | `855296a` |
| 12/11/2026 | Ronda 1, paso 1: delimitación del tema. Actualicé el README con título, integrantes, modalidad, objetivo, alcance y entregables. Creé `documento/alcance.md` con la pregunta, los objetivos, los términos que usamos y los límites. Busqué y verifiqué 14 fuentes (documentación oficial, RFC/estándares y artículos académicos) y anoté sus datos y para qué sirve cada una en `documento/fuentes-investigacion.md` | Pilar | rama `pilar-delimitacion-tema` |
| 13/11/2026 | Armé la red en GNS3 y ejecuté nueve pruebas de conectividad. Guardé los registros, dos capturas, el proyecto portable y las instrucciones para reproducirlo. | Martina | Consultar historial de Git |
| 13/11/2026 | Revisé `demo/README.md` y los nueve registros de `demo/resultados/`. Documenté la metodología y comparé los dos criterios (primer fallo y tres fallos consecutivos, una notificación por incidente) en `demo/analisis.md`. Separé `sostenida-01.txt` como prueba exploratoria y expresé la demora en comprobaciones, porque los registros no tienen marcas temporales | Tomás | rama `grupo-07` |

##  Decisiones tomadas

- Modalidad B, con una mini simulación como complemento de la revisión.
- Pregunta de investigación y título definidos (ver `documento/alcance.md`).
- Los dos criterios se aplican sobre los mismos registros de conectividad.
- Queda pendiente acordar qué interrupciones requieren intervención. Por ahora presentamos dos interpretaciones posibles, sin clasificar las notificaciones como falsas alarmas.
- Si aplicamos los criterios a mano sobre una tabla, lo decimos así; no decimos que implementamos alertas automáticas.
- Los resultados de la simulación no se generalizan a redes reales.
- Cambio de tareas: Martina arma y ejecuta la simulación; Tomás documenta la metodología y analiza los resultados.
- Ronda 3 redistribuida: Pilar hace la revisión académica, Martina cierra la demo y Tomás integra el paper, arma el póster y cierra el repositorio.
- Criterio 2: avisar después de tres fallos consecutivos. Los dos criterios cuentan una notificación por incidente.
- `sostenida-01.txt` es un registro válido pero exploratorio: queda fuera de la comparación principal. Lo decidimos después de ejecutar las pruebas.
- La demora de detección se expresa en comprobaciones, no en segundos: los registros no tienen marcas temporales.

## Pendientes

### Ronda 1 — organizar y preparar
- [x] Pilar: actualizar README, progreso, alcance y buscar fuentes
- [x] Martina: diseñar la red en GNS3 y definir el procedimiento de las pruebas en `demo/README.md` (tarea que pasó de Tomás a Martina)
- [ ] Martina: adaptar la introducción al objetivo, armar el índice según la modalidad B, explicar cómo buscamos y elegimos las fuentes y crear `documento/referencias.md`

### Ronda 2 — investigar y obtener resultados
- [ ] Pilar: leer y verificar las fuentes; redactar alertas accionables, fatiga de alarmas, SLI/SLO, innovaciones recientes, desafíos y seguridad; revisar los criterios del protocolo
- [x] Martina: armar la topología y realizar nueve ejecuciones (tres normales, tres intentos de corte breve, dos cortes sostenidos y una ejecución exploratoria); guardar registros, capturas y proyecto
- [x] Tomás: documentar la metodología y comparar los dos criterios sobre los registros: notificaciones, incidentes detectados y demora de detección (`demo/analisis.md`)
- [ ] Los tres: definir qué interrupción requiere intervención, interpretar los resultados y acordar las conclusiones
- [ ] Martina: preparar la introducción, la estructura y las referencias del artículo

### Ronda 3 — revisar y entregar
- [ ] Pilar: revisar citas, referencias, precisión del contenido y aspectos de seguridad; seleccionar los hallazgos principales para la infografía
- [ ] Martina: verificar que el proyecto GNS3 se pueda importar y reproducir, preparar la demo de 3–5 minutos y comprobar que funcione sin internet; dejar listas las configuraciones, capturas y registros
- [ ] Tomás: integrar y dar formato al paper, armar el póster A1 con los aportes del grupo y exportar ambos a PDF; actualizar README, bitácora y checklist, comprobando que estén todos los entregables y sus editables

## Notas

- El `.gitignore` excluye los archivos `*.log`. Los registros de la simulación hay que guardarlos con otra extensión (por ejemplo `.txt` o `.csv`) para que se suban.
- La numeración de `documento/fuentes-investigacion.md` es provisional; la final va en `documento/referencias.md`.
- Hay dos fuentes para confirmar antes de entregar: NIST SP 800-92r1 todavía es un borrador (2023) y AlertGuardian es un preprint aceptado en ASE 2025.
