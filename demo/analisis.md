# Análisis de los registros: comparación de criterios de alerta

> **Responsable:** Tomás Terruli
> **Fuentes:** los nueve registros de `demo/resultados/` y el procedimiento de `demo/README.md`.
> Los criterios se aplicaron **manualmente** sobre los registros. No implementamos alertas automáticas.

---

## 1. Metodología

### 1.1. Cómo se obtuvieron los registros

Resumen de lo documentado en `demo/README.md`:

- **Red:** dos VPCS (PC1 192.168.10.1/24 y PC2 192.168.10.2/24) conectados a un Ethernet switch en GNS3 2.2.61 (VPCS 0.6.2, Windows).
- **Comprobación:** PC1 ejecuta `ping 192.168.10.2 -c 30 -i 1000 -w 500`. Son 30 solicitudes ICMP por prueba, con una espera configurada de 1000 ms entre envíos y hasta 500 ms para cada respuesta.
- **Interrupción:** *Suspend* y *Resume* manual del enlace Switch1–PC2.
- **Pruebas:** nueve ejecuciones: tres normales, tres intentos de corte breve, dos cortes sostenidos (buscando unos 10 s de corte) y una ejecución exploratoria (`sostenida-01`).

### 1.2. Cómo se leyeron los registros

Cada línea del registro es una **comprobación**, identificada por su `icmp_seq`:

| Línea en el registro | Resultado |
|----------------------|-----------|
| `84 bytes from 192.168.10.2 icmp_seq=N ...` | **Éxito** |
| `192.168.10.2 icmp_seq=N timeout` | **Fallo** |

Un **incidente** es una racha de fallos consecutivos: empieza en el primer `timeout` después de un éxito y termina en el primer éxito siguiente.

### 1.3. Criterios aplicados

| Criterio | Cuándo notifica | Cuántas veces |
|----------|-----------------|---------------|
| **C1: primer fallo** | En el primer `timeout` de la racha | Una notificación por incidente |
| **C2: tres fallos consecutivos** | En el tercer `timeout` seguido de la racha | Una notificación por incidente; si la racha tiene menos de 3 fallos, no notifica |

**Regla del análisis:** para esta comparación, consideramos cerrado el incidente con la primera respuesta exitosa. Contamos únicamente notificaciones de apertura.

### 1.4. Cómo se mide la demora de detección

**Los registros no tienen marcas temporales**, así que **no calculamos segundos de detección**. La demora se expresa en **comprobaciones**: cuántas comprobaciones fallidas necesita cada criterio para notificar, contadas desde la primera fallida.

- C1 notifica en la 1.ª comprobación fallida.
- C2 notifica en la 3.ª comprobación fallida, o sea 2 comprobaciones después que C1.

Aunque el comando tenía configurado un intervalo de 1000 ms, no convertimos comprobaciones a segundos: la hora real de cada envío no quedó registrada.

### 1.5. Prueba exploratoria

`sostenida-01.txt` **se trata como prueba exploratoria**. Es un registro válido, pero surgió de un intento distinto: se buscaba un corte breve, se observaron 13 comprobaciones fallidas consecutivas y se guardó como sostenida. Por eso se muestra aparte y **no entra en la comparación principal**. Esta separación se decidió **después de ejecutar las pruebas**.

---

## 2. Resultados por prueba

| Prueba | Comprobaciones | Fallos | Racha de fallos (`icmp_seq`) | C1 notifica en | C2 notifica en | Recupera en |
|--------|----------------|--------|------------------------------|----------------|----------------|-------------|
| normal-01 | 30 | 0 | — | — | — | — |
| normal-02 | 30 | 0 | — | — | — | — |
| normal-03 | 30 | 0 | — | — | — | — |
| breve-01 | 30 | 4 | 8–11 (4 fallos) | seq 8 | seq 10 | seq 12 |
| breve-02 | 30 | 2 | 5–6 (2 fallos) | seq 5 | no notifica | seq 7 |
| breve-03 | 30 | 2 | 6–7 (2 fallos) | seq 6 | no notifica | seq 8 |
| sostenida-02 | 30 | 7 | 5–11 (7 fallos) | seq 5 | seq 7 | seq 12 |
| sostenida-03 | 30 | 8 | 6–13 (8 fallos) | seq 6 | seq 8 | seq 14 |
| *sostenida-01 (exploratoria)* | *30* | *13* | *11–23 (13 fallos)* | *seq 11* | *seq 13* | *seq 24* |

Cada prueba con corte tuvo una sola racha de fallos, es decir, un solo incidente.

### Ejemplo de aplicación: breve-01

```
icmp_seq=7   éxito
icmp_seq=8   timeout   ← 1.er fallo → C1 notifica
icmp_seq=9   timeout   ← 2.º fallo
icmp_seq=10  timeout   ← 3.er fallo → C2 notifica
icmp_seq=11  timeout   ← 4.º fallo (ninguno vuelve a notificar)
icmp_seq=12  éxito     ← recuperación
```

En breve-02 y breve-03 la racha tuvo solo 2 fallos, así que C2 nunca llegó a 3 y no notificó.

---

## 3. Comparación de criterios

Totales sobre las **ocho pruebas incluidas en la comparación principal**, sin la exploratoria:

| Escenario | Pruebas | Incidentes (rachas) | Notificaciones C1 | Notificaciones C2 | Demora C1 | Demora C2 |
|-----------|---------|---------------------|-------------------|-------------------|-----------|-----------|
| Normal | 3 | 0 | 0 | 0 | — | — |
| Corte breve | 3 | 3 | 3 | 1 | 1.ª fallida | 3.ª fallida (solo breve-01) |
| Corte sostenido | 2 | 2 | 2 | 2 | 1.ª fallida | 3.ª fallida |
| **Total** | **8** | **5** | **5** | **3** | | |

Si se suma la prueba exploratoria, C1 tiene 6 notificaciones y C2 tiene 4. Las conclusiones no cambian.

### Lo que muestran los registros

1. **Sin cortes, ningún criterio notificó.** En las 90 comprobaciones de las pruebas normales no hubo fallos.
2. **En los cortes sostenidos, los dos criterios detectaron el incidente.** C2 lo hizo 2 comprobaciones después que C1.
3. **En los cortes breves, C1 notificó siempre (3 de 3) y C2 una sola vez (1 de 3).** C2 solo notificó en breve-01, donde se observaron 4 comprobaciones fallidas consecutivas, lo suficiente para superar su umbral.
4. **La diferencia en cantidad de notificaciones viene entera de los cortes breves:** 5 contra 3. En los sostenidos los dos criterios emiten lo mismo.

### Cómo se interpretan las notificaciones de los cortes breves

Todavía **no definimos qué interrupción requiere intervención**: depende del objetivo de servicio que acordemos (ver `documento/alcance.md`). Por eso no clasificamos ninguna notificación como falsa alarma. Las dos lecturas posibles son:

- **Si los cortes breves no requieren intervención:** C1 emitió 3 notificaciones innecesarias y C2 una (breve-01). C2 redujo el ruido sin dejar de detectar las dos interrupciones sostenidas incluidas en la comparación.
- **Si cualquier corte requiere intervención:** C2 dejó sin avisar 2 de los 5 incidentes (breve-02 y breve-03). C1 los detectó todos y antes.

El intercambio que muestra el laboratorio es este: **esperar más fallos reduce notificaciones a cambio de detectar más tarde y de ignorar los cortes más cortos que el umbral.**

---

## 4. Limitaciones

- **Sin marcas temporales:** la demora de detección se expresa en comprobaciones y no en segundos. La cantidad de `timeout` tampoco equivale directamente a segundos de corte.
- **Cortes manuales:** no empezaron en la misma comprobación ni duraron lo mismo. Un corte pensado como breve dio 4 fallos (breve-01) y otro dio 13 (sostenida-01).
- **Una sola racha por prueba:** las interrupciones repetidas o intermitentes quedaron fuera del experimento.
- **Pocas pruebas** (8 en la comparación principal) en un laboratorio controlado: los resultados ilustran el comportamiento de los criterios, no se generalizan a redes reales.
- **Solo conectividad ICMP:** que el ping responda no implica que un servicio funcione.
- **Criterios aplicados a mano** sobre los registros. No hay sistema de alertas automático.
