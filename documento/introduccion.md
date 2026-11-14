# 1. Introducción

Para investigar una falla en un sistema necesitamos información sobre su funcionamiento. La observabilidad permite estudiar su comportamiento a partir de señales como métricas, registros de eventos (logs) y trazas. Las métricas muestran valores numéricos, los logs registran eventos y las trazas permiten seguir una solicitud entre distintos componentes [1].

Contar con esa información también plantea una decisión: cuándo generar una alerta. Avisar ante el primer fallo permite advertir una interrupción rápidamente. Esperar varios fallos consecutivos puede evitar avisos por interrupciones cortas, pero también retrasa la detección. Que una notificación sea útil depende de si la situación requiere alguna acción, según las necesidades del servicio [4].

En este trabajo revisamos criterios de alertado dentro de la observabilidad de servicios. La pregunta que guía la investigación es: ¿cómo influyen los criterios de activación de alertas en la cantidad de notificaciones y en el tiempo de detección de una falla? Para abordarla, analizamos bibliografía sobre monitoreo, objetivos de servicio y alertas, junto con avances recientes y aspectos de seguridad.

Como complemento, realizamos una experiencia en GNS3 con dos equipos conectados a un switch. Registramos pruebas de conectividad mediante ping, tanto sin interrupciones como con cortes manuales del enlace. Sobre los mismos registros aplicamos dos criterios: avisar ante el primer fallo o esperar tres fallos consecutivos. La comparación se hizo manualmente; no implementamos un sistema de alertas automáticas.

Esta experiencia permite observar cómo cambia la cantidad de avisos según el criterio elegido. Como los registros no incluyen marcas temporales, expresamos la demora de detección en comprobaciones y no en segundos. Además, las pruebas evalúan conectividad ICMP: no permiten asegurar que una aplicación funcione ni generalizar los resultados a una red real.

El artículo presenta la revisión del estado del arte, las innovaciones y los desafíos de seguridad. Luego describe la experiencia, compara los resultados y desarrolla las conclusiones y limitaciones.

## Referencias citadas en esta introducción

La numeración corresponde al listado de fuentes del grupo y se unificará al integrar el artículo.

[1] OpenTelemetry. “Observability primer”. Documentación oficial.

https://opentelemetry.io/docs/concepts/observability-primer/

[4] Google. “Alerting on SLOs”. The Site Reliability Workbook, 2018.

https://sre.google/workbook/alerting-on-slos/

