# Hallazgos para el póster

> **Responsable:** Pilar Giannelli
> Cinco hallazgos: tres de la bibliografía y dos del laboratorio. La numeración de las fuentes sigue `documento/referencias.md`. Los resultados del laboratorio salen de `demo/analisis.md`.

## De la bibliografía

**1. Esperar a que la falla dure tiene un costo.**
Pedir que la condición se mantenga un tiempo antes de avisar puede dar más precisión, pero detecta más tarde y puede dejar pasar fallas que van y vienen, porque el contador vuelve a cero con cada corte.
*Fuente:* [4] Thurgood *et al.*, "Alerting on SLOs" (2018).

**2. Muchos avisos juntos también son un problema.**
Cuando una falla dispara muchísimas alertas a la vez ("tormenta de alertas"), cuesta encontrar la causa. Resumir la tormenta en unas pocas alertas representativas redujo en más de un 98% las alertas que había que revisar.
*Fuente:* [11] Zhao *et al.*, ICSE-SEIP 2020.

**3. Las herramientas nuevas reducen avisos, pero la última palabra la tienen las personas.**
AlertGuardian redujo las alertas un 94,8% en cuatro conjuntos de datos reales. De las 1.174 reglas que propuso mejorar, los ingenieros aceptaron 375 (32%).
*Fuente:* [14] Yu *et al.*, ASE 2025.

## Del laboratorio (GNS3, 8 pruebas de la comparación principal)

**4. La diferencia entre criterios aparece solo en los cortes breves.**

| | C1: primer fallo | C2: tres fallos seguidos |
|---|---|---|
| Notificaciones en total (5 incidentes) | 5 | 3 |
| Cortes breves (3) | 3 | 1 |
| Cortes sostenidos (2) | 2 | 2 |
| Pruebas sin cortes (3) | 0 | 0 |

*Fuente:* `demo/analisis.md`.

**5. Avisar menos implica avisar más tarde.**
En los cortes sostenidos, C2 notificó 2 comprobaciones después que C1. En dos cortes breves de 2 fallos, C2 no notificó.
*Fuente:* `demo/analisis.md`.

## Para tener en cuenta al armar el póster

- Los hallazgos 4 y 5 son resultados de un laboratorio chico (8 pruebas, solo ping) y no se generalizan a redes reales. Conviene ponerlo en una línea cerca de la tabla.
- Hasta que el grupo acuerde qué interrupción requiere intervención, no hay que llamar "falsas alarmas" ni "avisos innecesarios" a las notificaciones de los cortes breves.
- La demora está en comprobaciones, no en segundos, porque los registros no tienen hora.
