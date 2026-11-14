# Organización del artículo

Título: De las métricas a las acciones: calidad de las alertas y detección de fallas en servicios.

Modalidad B: revisión del estado del arte con una experiencia práctica como complemento.

Este archivo organiza la integración del paper. No reemplaza los documentos que ya escribió cada integrante.

## Resumen y palabras clave

Presentar el problema, el objetivo, la revisión bibliográfica, la comparación realizada y el principal resultado. Redactar el resumen cuando esté integrado el artículo.

Palabras clave propuestas: observabilidad, monitoreo, criterios de alerta, conectividad, fatiga de alarmas.

## 1. Introducción

Usar documento/introduccion.md.

Presentar el tema, la pregunta de investigación, el objetivo y los límites de la experiencia.

## 2. Revisión del estado del arte

Usar documento/estado-del-arte.md.

Incluir:

- Observabilidad y monitoreo.

- Alertas que requieren una acción.

- Indicadores y objetivos de servicio: SLI y SLO.

- Estrategias de activación y gestión de notificaciones.

- Tabla comparativa de estrategias.

- Fatiga de alarmas y tormentas de alertas.

## 3. Innovaciones, desafíos y seguridad

Integrar los apartados correspondientes de documento/estado-del-arte.md.

Presentar avances recientes, sus limitaciones y los desafíos de proteger los registros y gestionar las alertas de seguridad.

Distinguir los resultados de otros estudios de lo que observamos en nuestro laboratorio.

## 4. Metodología

### 4.1. Revisión bibliográfica

Explicar cómo buscamos y seleccionamos las fuentes, tomando como base documento/fuentes-investigacion.md.

Indicar los temas buscados, los tipos de fuentes y los criterios de selección que realmente utilizamos. No presentar la búsqueda como una revisión sistemática si no seguimos ese método.

### 4.2. Experiencia en GNS3

Resumir demo/README.md y la metodología de demo/analisis.md.

Describir la topología, el comando de ping, los cortes manuales, los dos criterios y la forma de contar las notificaciones.

Aclarar que se obtuvieron nueve registros y que uno se presenta por separado como exploratorio. Explicar el motivo de esa separación.

## 5. Resultados y discusión

Usar demo/analisis.md y los registros de demo/resultados/.

Incluir:

- Resultados por escenario.

- Comparación de notificaciones entre ambos criterios.

- Demora de detección expresada en comprobaciones.

- Efecto de incluir la prueba exploratoria.

- Relación entre los resultados y la bibliografía.

- Limitaciones de las pruebas.

No clasificar una alerta como innecesaria sin explicar qué interrupciones requieren intervención. Si se presentan distintas interpretaciones, aclarar los supuestos de cada una.

## 6. Conclusiones

Responder la pregunta de investigación a partir de la bibliografía y los resultados.

Explicar qué se gana y qué se pierde al esperar varios fallos consecutivos. No afirmar que un criterio es mejor para todos los servicios.

## Referencias

Usar documento/referencias.md.

Conservar la correspondencia entre cada cita y su fuente. Incluir en la bibliografía final las fuentes citadas en el artículo.

## Criterios de integración

- Ajustar el artículo a la extensión pedida de 5 a 7 páginas.

- Evitar repetir explicaciones entre la revisión, la metodología y los resultados.

- Mantener los registros originales y las instrucciones completas en el repositorio.

- Revisar las conclusiones entre los tres integrantes.
