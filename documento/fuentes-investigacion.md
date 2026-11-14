# Fuentes relevadas — investigación bibliográfica

Primera búsqueda de fuentes sobre calidad de alertas, SLI/SLO y fatiga de alarmas. De cada una anotamos los datos en formato IEEE, qué tipo de fuente es y para qué nos sirve en la pregunta de investigación. La numeración es provisional; la final se arma en `documento/referencias.md`.

**Fecha de consulta de los recursos en línea:** 12 de noviembre de 2026.

## 1. Qué cubre cada fuente

| Requisito de la modalidad B | Fuentes |
|-----------------------------|---------|
| Artículo académico | [10], [11], [12], [13], [14] |
| Documentación técnica oficial | [1], [5], [15] |
| RFC o estándar | [6], [7], [8], [9], [16] |
| Avances de los últimos tres años | [9] (2023), [12] (2025), [13] (2024), [14] (2025) |
| Seguridad y protección de registros | [8], [9], [12], [16] |

| Tema | Fuentes |
|------|---------|
| Observabilidad y monitoreo | [1], [2], [7] |
| Alertas accionables | [2], [4] |
| SLI / SLO | [3], [4] |
| Criterios de activación (primer fallo o duración mínima) | [4], [5] |
| Reducción de ruido en herramientas (agrupación, inhibición, silencios) | [15] |
| Fatiga de alarmas y tormentas de alertas | [10], [11], [12] |
| Innovaciones recientes | [13], [14] |
| Conectividad ICMP (base de la simulación) | [6], [7] |

## 2. Fuentes

### Documentación técnica y libros de referencia

**[1]** OpenTelemetry Authors, "Observability primer," *OpenTelemetry Documentation*. [En línea]. Disponible: https://opentelemetry.io/docs/concepts/observability-primer/
- **Tipo:** documentación técnica oficial. Ya la usamos en el borrador de la introducción.
- **Para qué sirve:** define la observabilidad a partir de las señales que produce un sistema (métricas, logs y trazas). Nos da la base para la introducción.

**[2]** R. Ewaschuk, "Monitoring distributed systems," en *Site Reliability Engineering: How Google Runs Production Systems*, B. Beyer, C. Jones, J. Petoff y N. R. Murphy, Eds. Sebastopol, CA, EE. UU.: O'Reilly Media, 2016, cap. 6. [En línea]. Disponible: https://sre.google/sre-book/monitoring-distributed-systems/
- **Tipo:** libro técnico de referencia en la industria (versión en línea oficial).
- **Para qué sirve:** da principios para armar alertas. Dice que un buen sistema de alertas tiene mucha señal y muy poco ruido, y separa el síntoma ("qué está roto") de la causa ("por qué"). Lo usamos para explicar qué es una alerta accionable y por qué no conviene avisar cosas que no necesitan que alguien actúe.

**[3]** C. Jones, J. Wilkes, N. Murphy y C. Smith, "Service level objectives," en *Site Reliability Engineering: How Google Runs Production Systems*, B. Beyer, C. Jones, J. Petoff y N. R. Murphy, Eds. Sebastopol, CA, EE. UU.: O'Reilly Media, 2016, cap. 4. [En línea]. Disponible: https://sre.google/sre-book/service-level-objectives/
- **Tipo:** libro técnico de referencia en la industria.
- **Para qué sirve:** define SLI (lo que se mide), SLO (el valor objetivo para ese SLI) y SLA (el acuerdo con consecuencias). Nos ayuda a explicar cuándo una falla es lo bastante importante como para justificar una alerta.

**[4]** S. Thurgood *et al.*, "Alerting on SLOs," en *The Site Reliability Workbook: Practical Ways to Implement SRE*, B. Beyer, N. R. Murphy, D. K. Rensin, K. Kawahara y S. Thorne, Eds. Sebastopol, CA, EE. UU.: O'Reilly Media, 2018, cap. 5. [En línea]. Disponible: https://sre.google/workbook/alerting-on-slos/
- **Tipo:** libro técnico de referencia en la industria.
- **Para qué sirve (es la fuente más cercana a nuestra pregunta):**
  - Propone cuatro cosas para evaluar una estrategia de alertas: precisión (qué parte de las alertas corresponde a eventos importantes), exhaustividad o *recall* (qué parte de los eventos importantes generó alerta), tiempo de detección y tiempo de restablecimiento.
  - Compara seis estrategias, de la más simple a la más completa. Va desde el umbral inmediato hasta las alertas con varias ventanas y varias tasas de consumo del presupuesto de error.
  - El umbral inmediato detecta rápido pero tiene poca precisión. Es lo mismo que nuestro **criterio 1** (primer fallo).
  - Pedir que la condición dure un tiempo mínimo mejora la precisión, pero empeora la exhaustividad y el tiempo de detección, porque la espera es la misma sea cual sea la gravedad. Además, si la falla se corta un momento, la cuenta vuelve a cero, así que una falla que va y viene puede no avisar nunca. Es lo mismo que nuestro **criterio 2** (fallos seguidos), y es algo que podemos comprobar en la simulación.
  - Nos sirve de base para la tabla comparativa del estado del arte.

**[5]** Prometheus Authors, "Alerting rules," *Prometheus Documentation*. [En línea]. Disponible: https://prometheus.io/docs/prometheus/latest/configuration/alerting_rules/
- **Tipo:** documentación oficial de una herramienta de monitoreo muy usada.
- **Para qué sirve:** muestra cómo se aplican los dos criterios en una herramienta real. Una regla sin `for` se activa en la primera evaluación en que se cumple la condición (criterio 1). Con `for`, la alerta queda en estado *pending* y solo pasa a *firing* si la condición se mantiene durante ese tiempo (criterio 2). También existe `keep_firing_for`, que mantiene la alerta activa un rato después de que la condición deja de cumplirse. Por otro lado, separa la evaluación de reglas (Prometheus) del envío de notificaciones (Alertmanager), lo que nos ayuda a definir bien qué es una "notificación".

### RFC y estándares

**[6]** J. Postel, "Internet Control Message Protocol," RFC 792, sep. 1981. doi: 10.17487/RFC0792.
- **Tipo:** RFC (Internet Standard, STD 5).
- **Para qué sirve:** define los mensajes ICMP Echo y Echo Reply, que son los que usa el ping de la simulación. Aclara que ICMP informa problemas de la red pero no hace que IP sea confiable: no garantiza que un paquete o un mensaje de control llegue. Por eso un solo ping fallido no alcanza para decir que hay una caída.

**[7]** A. Morton, "Active and Passive Metrics and Methods (with Hybrid Types In-Between)," RFC 7799, may. 2016. doi: 10.17487/RFC7799.
- **Tipo:** RFC (informativo, IETF).
- **Para qué sirve:** distingue la medición activa, que genera sus propios paquetes para medir, de la pasiva, que solo observa el tráfico que ya existe. Nuestra simulación es una medición activa, y esto nos permite comentar que el tráfico de prueba puede afectar lo que se mide.

**[8]** R. Gerhards, "The Syslog Protocol," RFC 5424, mar. 2009. doi: 10.17487/RFC5424.
- **Tipo:** RFC (Proposed Standard).
- **Para qué sirve:** es el estándar para enviar mensajes de eventos. En la parte de seguridad habla, entre otras cosas, de la reproducción de mensajes (*replay*), la entrega confiable, la integridad, la observación de los mensajes y la denegación de servicio. También define campos para indicar la calidad de la hora de cada mensaje, por ejemplo si el reloj estaba sincronizado (`timeQuality`, `isSynced`). Lo usamos para el apartado sobre cómo proteger los registros en los que se basan las alertas.

**[9]** K. Scarfone y M. Souppaya, "Cybersecurity Log Management Planning Guide," NIST, Gaithersburg, MD, EE. UU., NIST SP 800-92r1 (Initial Public Draft), oct. 2023. doi: 10.6028/NIST.SP.800-92r1.ipd.
- **Tipo:** borrador de una guía de un organismo de estándares (NIST). Al 14/11/2026 sigue figurando como *Draft* en la página oficial; NIST todavía está procesando los comentarios públicos.
- **Para qué sirve:** muestra la tendencia reciente: la revisión se enfoca en planificar la gestión de registros en toda la organización y deja afuera los detalles de implementación. Se usa en la sección de innovaciones y hay que citarla como borrador.

**[16]** K. Kent y M. Souppaya, "Guide to Computer Security Log Management," NIST, Gaithersburg, MD, EE. UU., NIST SP 800-92, sep. 2006. doi: 10.6028/NIST.SP.800-92.
- **Tipo:** guía de un organismo de estándares (NIST). Es la versión final vigente.
- **Para qué sirve:** define la gestión de registros como el proceso de generar, transmitir, guardar, analizar y descartar los registros de seguridad, y tiene un apartado sobre su protección. Se usa en el apartado de seguridad de los registros.

### Artículos académicos

**[10]** M. Cvach, "Monitor alarm fatigue: An integrative review," *Biomedical Instrumentation & Technology*, vol. 46, no. 4, pp. 268–277, jul.–ago. 2012. doi: 10.2345/0899-8205-46.4.268.
- **Tipo:** revisión integrativa con revisión por pares (72 artículos).
- **Para qué sirve:** de acá viene el concepto de fatiga de alarmas, en el monitoreo de pacientes. Cuando hay demasiadas alarmas, y muchas son falsas o no importan, el personal se satura y deja de reaccionar. La revisión relaciona la fatiga con una alta tasa de falsas alarmas, un bajo valor predictivo positivo, la falta de estandarización y la cantidad de equipos que alarman. La usamos como antecedente del concepto, no como evidencia sobre redes.

**[11]** N. Zhao *et al.*, "Understanding and handling alert storm for online service systems," en *Proc. ACM/IEEE 42nd Int. Conf. Software Engineering: Software Engineering in Practice (ICSE-SEIP)*, 2020, pp. 162–171. doi: 10.1145/3377813.3381363.
- **Tipo:** artículo de conferencia con revisión por pares, con datos reales de un banco comercial.
- **Para qué sirve:** es el primer estudio con datos reales sobre las "tormentas de alertas", que pasan cuando una falla viene acompañada de muchísimas alertas a la vez y eso complica el diagnóstico. Proponen detectar la tormenta y resumirla en unas pocas alertas representativas. Muestra que la cantidad de notificaciones es un problema real y que se puede medir.

**[12]** S. Tariq, M. Baruwal Chhetri, S. Nepal y C. Paris, "Alert fatigue in security operations centres: Research challenges and opportunities," *ACM Computing Surveys*, vol. 57, no. 9, art. 224, pp. 1–38, abr. 2025. doi: 10.1145/3723158.
- **Tipo:** revisión sistemática en revista académica.
- **Para qué sirve:** revisa lo que hay en la literatura y en la industria sobre fatiga de alertas en los centros de operaciones de seguridad (SOC). Lo organiza en tres enfoques: automatización, ayuda al analista y colaboración entre personas e IA. Encuentra cuatro causas principales de la fatiga y propone líneas para investigar. Nos aporta la parte de seguridad y de desafíos.

**[13]** J. Kuang *et al.*, "Knowledge-aware alert aggregation in large-scale cloud systems: A hybrid approach," en *Proc. 46th Int. Conf. Software Engineering: Software Engineering in Practice (ICSE-SEIP '24)*, Lisboa, Portugal, 2024. doi: 10.1145/3639477.3639745.
- **Tipo:** artículo de conferencia con revisión por pares.
- **Para qué sirve:** presenta COLA, que agrupa las alertas que vienen de una misma falla. Para eso combina correlaciones entre alertas con un modelo de lenguaje que usa los procedimientos operativos de cada alerta. Es un ejemplo de innovación reciente para reducir notificaciones sin perder información.

**[14]** G. Yu *et al.*, "AlertGuardian: Intelligent alert life-cycle management for large-scale cloud systems," en *Proc. 40th IEEE/ACM Int. Conf. Automated Software Engineering (ASE 2025)*, Seúl, Corea del Sur, 2025, pp. 1–12. Versión preprint: arXiv:2601.14912 [cs.DC], ene. 2026. doi: 10.48550/arXiv.2601.14912.
- **Tipo:** artículo de conferencia (ASE 2025), con versión preprint en arXiv.
- **Para qué sirve:** trata la fatiga de alertas en todo el ciclo de vida de la alerta: se crea la regla, se dispara, alguien la atiende y después se mejora la regla. Usa modelos de grafos para filtrar ruido, modelos de lenguaje para resumir alertas y agentes para mejorar las reglas. Señala que herramientas como Alertmanager se enfocan en configurar y disparar reglas, pero no en si esas reglas son buenas. Representa lo más nuevo del área.

**[15]** Prometheus Authors, "Alertmanager," *Prometheus Documentation*. [En línea]. Disponible: https://prometheus.io/docs/alerting/latest/alertmanager/
- **Tipo:** documentación técnica oficial.
- **Para qué sirve:** explica la parte de las notificaciones. Alertmanager recibe las alertas de Prometheus, elimina duplicados, las agrupa, las envía al destinatario que corresponde y permite silenciarlas o inhibirlas. La inhibición suprime avisos de ciertas alertas si ya hay otras disparadas, y los silencios los apagan durante un tiempo. Lo usamos en la tabla comparativa como ejemplo de reducción de ruido después de que la alerta se dispara.

## 3. Estado de verificación

Revisiones del 13 y el 14 de noviembre de 2026, contra registros oficiales (RFC Editor, NIST, dblp, actas de conferencias, arXiv y las páginas de los propios autores).

| Fuente | Estado | Comentario |
|--------|--------|------------|
| [1], [2], [4], [5], [6], [8], [12], [13] | Confirmadas | Datos y contenido usado confirmados. |
| [3] | Confirmada | Autores confirmados en la página oficial del capítulo: "Written by Chris Jones, John Wilkes, and Niall Murphy with Cody Smith". |
| [7] | Confirmada | DOI 10.17487/RFC7799 confirmado en la ficha del RFC Editor. |
| [9] | Confirmada | Al 14/11/2026 la revisión 1 sigue figurando como borrador (*Draft*, 11/10/2023) en la página oficial de NIST: citarla así. |
| [16] | Confirmada | Guía final de 2006, separada de [9] en la revisión del 14/11/2026 porque el texto cita las dos versiones para cosas distintas. |
| [10] | Confirmada | Se usa solo la definición de fatiga de alarmas y sus causas. Se sacó la afirmación sobre retardos antes de alarmar, porque solo la encontramos en reseñas y no en el artículo. |
| [11] | Confirmada | Páginas 162–171 confirmadas en dblp y en el índice de las actas. El DOI lo tomamos de un enlace a la ACM Digital Library: conviene abrir https://doi.org/10.1145/3377813.3381363 una vez. |
| [14] | Parcialmente verificada | Confirmados: autores, título, publicación en ASE 2025 (IEEE, pp. 1–12) y todas las cifras usadas, en el resumen del preprint. Falta: el DOI de la versión de las actas no aparece en ningún registro consultado; por eso se cita también el preprint con su DOI de arXiv. |
| [15] | Confirmada | Fuente nueva (documentación oficial de Prometheus). Se agregó porque la tabla comparativa necesita un ejemplo de reducción de ruido en una herramienta real. |

Antes de entregar:
- [4], [5], [15]: volver a mirar las páginas, porque son documentos en línea que pueden cambiar.
- [9]: volver a fijarse si NIST publicó la versión final de SP 800-92r1.
- [14]: buscar el DOI de las actas de ASE 2025 en IEEE Xplore.
