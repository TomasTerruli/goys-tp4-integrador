# 1. Introducción

Para investigar una falla en un sistema necesitamos información sobre su funcionamiento. La observabilidad permite estudiar su comportamiento a partir de señales como métricas, registros de eventos (logs) y trazas. Las métricas muestran valores numéricos, los logs registran eventos y las trazas permiten seguir una solicitud entre distintos componentes [1].

Contar con esa información también plantea una decisión: cuándo generar una alerta. Avisar ante el primer fallo permite advertir una interrupción rápidamente. Esperar varios fallos consecutivos puede evitar avisos por interrupciones cortas, pero también retrasa la detección. Que una notificación sea útil depende de si la situación requiere alguna acción, según las necesidades del servicio [2], [4].

En este trabajo revisamos criterios de alertado dentro de la observabilidad de servicios. La pregunta que guía la investigación es: ¿cómo influyen los criterios de activación de alertas en la cantidad de notificaciones y en el tiempo de detección de una falla? Para abordarla, analizamos bibliografía sobre monitoreo, objetivos de servicio y alertas, junto con avances recientes y aspectos de seguridad.

Como complemento, realizamos una experiencia en GNS3 con dos equipos conectados a un switch. Registramos pruebas de conectividad con ping, tanto sin interrupciones como con cortes manuales del enlace. Sobre los mismos registros aplicamos dos criterios: avisar ante el primer fallo o esperar tres fallos consecutivos. La comparación se hizo manualmente; no implementamos un sistema de alertas automáticas.

Esta experiencia permite observar cómo cambia la cantidad de avisos según el criterio elegido. Como los registros no incluyen marcas temporales, expresamos la demora de detección en comprobaciones y no en segundos. Además, las pruebas evalúan conectividad ICMP: no permiten asegurar que una aplicación funcione ni generalizar los resultados a una red real.

El artículo presenta la revisión del estado del arte, las innovaciones y los desafíos de seguridad. Luego describe la experiencia, compara los resultados y desarrolla las conclusiones y limitaciones.

## Referencias citadas en esta introducción

La numeración corresponde a `documento/referencias.md`.

[1] OpenTelemetry Authors, "Observability primer," *OpenTelemetry Documentation*. [En línea]. Disponible: https://opentelemetry.io/docs/concepts/observability-primer/

[2] B. Beyer, C. Jones, J. Petoff y N. R. Murphy, Eds., Site Reliability Engineering: How Google Runs Production Systems. Sebastopol, CA, EE. UU.: O’Reilly Media, 2016. Cap. 4, “Service level objectives”, y cap. 6, “Monitoring distributed systems”. [En línea]. Disponible: https://sre.google/sre-book/table-of-contents/

[4] S. Thurgood *et al.*, "Alerting on SLOs," en *The Site Reliability Workbook: Practical Ways to Implement SRE*, B. Beyer, N. R. Murphy, D. K. Rensin, K. Kawahara y S. Thorne, Eds. Sebastopol, CA, EE. UU.: O'Reilly Media, 2018, cap. 5. [En línea]. Disponible: https://sre.google/workbook/alerting-on-slos/
