# Hallazgos para el póster

> **Responsable:** Pilar Giannelli
> Cinco hallazgos: tres de la bibliografía y dos del laboratorio. La numeración de las fuentes sigue `documento/referencias.md`. Los resultados del laboratorio salen de `demo/analisis.md`.

## De la bibliografía

**1. Esperar a que la falla dure tiene un costo.**
Pedir que la condición se mantenga un tiempo antes de avisar puede dar más precisión, pero detecta más tarde y puede dejar pasar fallas que van y vienen, porque el contador vuelve a cero con cada corte.
*Fuente:* [4] Thurgood *et al.*, "Alerting on SLOs" (2018).

**2. Un aviso solo sirve si alguien tiene que actuar.**
Si una regla manda avisos que siempre se pueden ignorar, la gente empieza a no leerlos y se puede pasar por alto uno real. La meta es tener mucha señal y poco ruido.
*Fuente:* [2] Beyer *et al.* (eds.), *Site Reliability Engineering* (2016), cap. 6.

**3. El exceso de avisos también es un problema de seguridad.**
Una revisión de 2025 sobre los centros de operaciones de seguridad identifica cuatro causas principales de la fatiga de alertas y agrupa las soluciones en tres enfoques: automatización, ayuda al analista y colaboración entre personas e IA.
*Fuente:* [6] Tariq *et al.*, *ACM Computing Surveys* (2025).

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
