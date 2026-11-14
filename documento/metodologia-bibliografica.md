# Metodología de la revisión bibliográfica

Realizamos una revisión narrativa sobre criterios de alertado y detección de fallas. Organizamos las fuentes alrededor de cuatro temas: observabilidad y monitoreo, objetivos de servicio, calidad de las alertas y seguridad de los registros.

Buscamos las fuentes con un buscador web general, usando términos en inglés como *alerting on SLOs*, *alert fatigue*, *alert storm*, *alert aggregation*, *Prometheus alerting rules*, *ICMP*, *active and passive measurement*, *syslog security* y *log management*. Cuando encontrábamos una fuente, la abríamos en su sitio original: el RFC Editor para los RFC, el sitio de NIST para sus guías, los sitios oficiales de Google SRE, Prometheus y OpenTelemetry, y la ACM Digital Library, dblp, arXiv y las páginas de las conferencias para los artículos. No usamos bases como Google Scholar, SciELO o Redalyc.

Para elegir las fuentes tuvimos en cuenta cuatro criterios: que tuvieran relación directa con la pregunta de investigación; que el autor o la institución fueran identificables; que los artículos estuvieran revisados por pares o aceptados en una conferencia; y que fueran de los últimos tres años cuando las usábamos para hablar de innovaciones. Siempre preferimos el documento original antes que una reseña. Por eso descartamos una afirmación sobre fatiga de alarmas que solo encontramos en reseñas y no pudimos comprobar en el artículo original.

El relevamiento quedó documentado en `documento/fuentes-investigacion.md`. Para cada fuente registramos sus datos bibliográficos, el tipo de publicación, su relación con la pregunta de investigación y su estado de verificación.

El conjunto reúne documentación oficial de OpenTelemetry y Prometheus, capítulos de los libros de Google sobre SRE, documentos RFC y NIST, y artículos académicos. La documentación técnica aporta conceptos y ejemplos de funcionamiento; los artículos permiten discutir problemas, resultados e innovaciones. En el artículo citamos seis de esas fuentes, que están en `documento/referencias.md`.

Incluimos publicaciones anteriores para explicar conceptos y protocolos, junto con trabajos recientes para el apartado de innovaciones. El antecedente sobre fatiga de alarmas en el ámbito sanitario se utiliza únicamente para contextualizar el concepto, no como evidencia de resultados en redes informáticas.

La revisión de las referencias se registró mediante consultas a páginas oficiales, registros bibliográficos y publicaciones originales. Las verificaciones incompletas quedaron señaladas para resolverlas antes de la entrega. Distinguimos los borradores y preprints de las versiones publicadas.

Para organizar la comparación consideramos cuándo se activa una alerta, qué notificaciones puede generar, cómo afecta la detección y cuáles son sus limitaciones. Luego relacionamos esos aspectos con los dos criterios aplicados manualmente a los registros de GNS3.

La revisión no pretende cubrir toda la literatura disponible. No contamos con un registro completo de consultas y resultados descartados, por lo que no la presentamos como una revisión sistemática. Sus conclusiones se limitan a las fuentes analizadas y a la experiencia realizada.