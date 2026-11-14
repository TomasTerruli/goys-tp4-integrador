# Revisión del protocolo experimental

> **Responsable:** Pilar Giannelli
> **Archivos revisados:** `demo/README.md`, `demo/analisis.md` y los nueve registros de `demo/resultados/`.

## 1. Verificación de los datos

Volví a contar cada registro con un script: comprobaciones, éxitos, fallos, rachas de fallos y la comprobación en la que avisaría cada criterio. Los resultados coinciden con las tablas de `demo/README.md` y `demo/analisis.md`:

| Prueba | Fallos | Racha (`icmp_seq`) | C1 avisa en | C2 avisa en | Recupera en |
|--------|--------|--------------------|-------------|-------------|-------------|
| normal-01 a 03 | 0 | — | — | — | — |
| breve-01 | 4 | 8–11 | 8 | 10 | 12 |
| breve-02 | 2 | 5–6 | 5 | no avisa | 7 |
| breve-03 | 2 | 6–7 | 6 | no avisa | 8 |
| sostenida-02 | 7 | 5–11 | 5 | 7 | 12 |
| sostenida-03 | 8 | 6–13 | 6 | 8 | 14 |
| sostenida-01 (exploratoria) | 13 | 11–23 | 11 | 13 | 24 |

Todas las pruebas tienen 30 comprobaciones numeradas sin saltos, y cada prueba con corte tiene una sola racha de fallos.

## 2. Qué está claro en el protocolo

- La red, el comando de ping y la forma de cortar el enlace están descritos y se pueden repetir.
- Los dos criterios están bien definidos: C1 avisa con el primer fallo y C2 con el tercero seguido. Los dos cuentan una notificación por incidente.
- Qué es un incidente (una racha de fallos seguidos) y cuándo se cierra (con la primera respuesta) está escrito.
- La demora se mide en comprobaciones y no en segundos, y está explicado por qué: los registros no tienen hora.
- Se aclara que los criterios se aplicaron a mano y que no hay alertas automáticas.
- `sostenida-01` está separada como prueba exploratoria, y se dice que eso se decidió después de medir.

## 3. Lo que falta definir o aclarar

**1. Qué interrupción requiere intervención.** Es lo único que bloquea las conclusiones. Sin esta definición no se pueden calcular la precisión ni la exhaustividad de cada criterio, que son los atributos que usa la bibliografía para comparar estrategias [4]. Según [2, cap. 4] y [4], la definición debería salir de un objetivo de servicio. Estas son algunas opciones, aplicadas a los cinco incidentes de la comparación principal:

| Opción: requiere intervención si… | Incidentes relevantes | C1 (5 avisos) | C2 (3 avisos) |
|-----------------------------------|-----------------------|---------------|---------------|
| A. Hay cualquier corte (objetivo: ningún fallo) | los 5 | precisión 5/5, exhaustividad 5/5 | precisión 3/3, exhaustividad 3/5 |
| B. Se pierden más de 3 de las 30 comprobaciones (objetivo: 90% de respuestas) | breve-01, sostenida-02 y 03 | precisión 3/5, exhaustividad 3/3 | precisión 3/3, exhaustividad 3/3 |
| C. Se pierden 5 o más comprobaciones | sostenida-02 y 03 | precisión 2/5, exhaustividad 2/2 | precisión 2/3, exhaustividad 2/2 |

La tabla muestra lo mismo que dice la bibliografía: cuál criterio "gana" depende del objetivo que se elija. Hay que tener cuidado con una definición del tipo "requiere intervención si hay 3 fallos seguidos", porque coincide con el umbral de C2 y le daría un resultado perfecto por construcción. La definición la tenemos que acordar entre los tres y, como se decide después de medir, conviene decirlo en el artículo, igual que se hizo con `sostenida-01`.

**2. Por qué tres fallos.** El valor de *N* = 3 del criterio 2 está registrado como decisión en `progreso.md`, pero no está explicado. Alcanza con una línea que diga cómo se eligió, aunque sea un valor ilustrativo.

**3. La recuperación no notifica.** Ninguno de los dos criterios avisa cuando se vuelve a la normalidad. Está bien para esta comparación, pero conviene aclararlo, porque la bibliografía también evalúa cuánto tarda una alerta en apagarse después de la solución (tiempo de restablecimiento) [4].

**4. Fallas intermitentes.** No se probaron. La bibliografía señala justamente ese caso como el punto débil de las alertas por duración mínima [4]. Ya figura en las limitaciones de `demo/analisis.md`; en el artículo conviene mencionarlo junto con la discusión del criterio 2.

**5. Formato de `demo/README.md`.** El archivo tiene caracteres escapados (`\#`, `\-`, `1\.`, `&#x20;`), que salen de copiar el texto desde otro editor. En GitHub los títulos y las listas no se ven con formato. Se arregla sacando la barra invertida (`\#` → `#`, `\-` → `-`, `1\.` → `1.`) y cambiando `&#x20;` por espacios.

## 4. Referencias usadas en esta revisión

Se usa la numeración de `documento/referencias.md`: [2] Beyer *et al.* (eds.), *Site Reliability Engineering* (2016), cap. 4; [4] Thurgood *et al.*, "Alerting on SLOs" (2018).
