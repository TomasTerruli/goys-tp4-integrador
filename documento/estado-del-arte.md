# Estado del arte, innovaciones y desafíos

> **Responsable:** Pilar Giannelli
> Secciones 2, 3 y 4 del artículo (plantilla de la modalidad B). La numeración de las citas sigue `documento/fuentes-investigacion.md`.

---

## 2. Estado del arte

### 2.1. De la métrica al aviso

El monitoreo consiste en juntar y mostrar en tiempo real datos numéricos de un sistema, como la cantidad de solicitudes, los errores o los tiempos de respuesta [2]. Pero una métrica sola no le avisa a nadie. Para eso están las alertas: notificaciones para una persona, que pueden llegar como ticket, como correo o como *page*, el aviso más urgente, que va directo a quien está de guardia [2].

El monitoreo de caja blanca usa datos internos del sistema; el de caja negra prueba desde afuera lo mismo que vería un usuario. Este último detecta síntomas que ya están pasando, así que sirve para avisos urgentes, aunque no anticipa fallas [2]. Para elegir qué mirar, el mismo capítulo propone cuatro "señales doradas": latencia, tráfico, errores y saturación [2].

El ping que usamos en la simulación es un ejemplo de medición activa. Según el RFC 7799, los métodos activos generan su propio flujo de paquetes para medir, mientras que los pasivos solo observan el tráfico que ya existe, sin modificarlo [7]. Además hay que tener en cuenta que ICMP no garantiza la entrega: ni el datagrama ni el mensaje de control tienen asegurado que lleguen [6]. Por eso un ping perdido, por sí solo, no alcanza para decir que hay una caída.

### 2.2. Alertas accionables

No toda condición anormal merece un aviso. Para Ewaschuk, un *page* tiene que reservarse para situaciones urgentes, sobre las que se pueda actuar y que afecten a los usuarios. Además, cada aviso debería necesitar criterio humano, no una respuesta automática, y tratar un problema nuevo [2]. La razón es práctica: una persona puede responder con verdadera urgencia pocas veces por día antes de cansarse. Si una regla genera avisos que siempre se pueden ignorar porque no pasa nada, no debería despertar a nadie [2].

Cuando hay demasiadas alertas o tienen poco valor, la gente empieza a leerlas por arriba o a ignorarlas, y así se puede pasar por alto un aviso real y alargar una caída [2]. Por eso el capítulo resume la meta como tener mucha señal y poco ruido [2].

### 2.3. SLI y SLO como criterio para decidir

Para saber si una falla es importante hace falta contra qué compararla. Para eso están los objetivos de servicio. Un SLI es una medida numérica bien definida de algún aspecto de la calidad del servicio, como la latencia, la tasa de errores o la disponibilidad. Un SLO es el valor objetivo, o el rango, que se espera para ese SLI. Un SLA es un acuerdo con los usuarios que tiene consecuencias si no se cumple [3].

El capítulo de Jones *et al.* recomienda empezar por lo que les importa a los usuarios y no por lo que es fácil de medir. También recomienda no apuntar a la perfección: se acepta un margen de error, el presupuesto de error, porque buscar el 100% frena la innovación [3]. Esto cambia la pregunta de cuándo alertar. Ya no se trata de avisar ante cualquier anomalía, sino cuando algo pone en riesgo el objetivo.

### 2.4. Estrategias para activar una alerta

Thurgood *et al.* proponen cuatro atributos para evaluar una estrategia de alertas [4]:

- **Precisión:** qué parte de las alertas corresponde a eventos importantes.
- **Exhaustividad (*recall*):** qué parte de los eventos importantes generó una alerta.
- **Tiempo de detección:** cuánto tarda en salir la notificación.
- **Tiempo de restablecimiento:** cuánto sigue activa la alerta después de que se solucionó el problema.

Con esos atributos comparan seis estrategias, de la más simple a la más completa [4]. La más simple avisa apenas la tasa de errores supera el objetivo en una ventana corta. Tiene buen tiempo de detección y buena exhaustividad, pero poca precisión: en su ejemplo podría disparar hasta 144 alertas por día aunque el servicio siga cumpliendo el SLO [4].

Otra estrategia exige que la condición dure un tiempo mínimo antes de avisar. Así puede ganar precisión, porque no avisa por los picos más cortos que esa duración. A cambio pierde exhaustividad y tarda más en detectar [4]. El problema es que la espera es la misma sin importar la gravedad: una caída total tarda lo mismo en avisar que una falla del 0,2%. Además, si la métrica se normaliza un momento, el contador vuelve a cero, así que una falla que va y viene puede no avisar nunca. Por eso los autores no la recomiendan como criterio para alertas sobre SLO [4]. Esta estrategia es la que más se parece a nuestro criterio 2, el de varios fallos seguidos.

Las estrategias más avanzadas miran la tasa de consumo del presupuesto de error, o sea, qué tan rápido se gasta el margen permitido. La que recomiendan combina varias tasas con una ventana larga y una corta, y solo avisa si las dos superan el umbral. Por ejemplo, para un SLO de 99,9%, un *page* si se consume el 2% del presupuesto en una hora y un ticket si se consume el 10% en tres días. La desventaja es que tiene muchos parámetros para ajustar [4].

Las herramientas actuales usan estas ideas. En Prometheus el trabajo está dividido en dos partes. Por un lado, Prometheus evalúa las reglas de alerta. Una regla sin la cláusula `for` se activa en la primera evaluación en que se cumple la condición. Con `for`, la alerta queda "pendiente" y recién pasa a "disparada" si la condición se mantiene durante ese tiempo [5]. También existe `keep_firing_for`, que deja la alerta activa un rato después de que la condición dejó de cumplirse [5]. Por otro lado, las alertas activas se mandan a Alertmanager, que es el que gestiona las notificaciones: saca los duplicados, agrupa alertas, las manda a quien corresponde y permite silenciarlas o inhibirlas [15]. La inhibición suprime las notificaciones de ciertas alertas si ya hay otras disparadas, y el silencio las apaga durante un tiempo, por ejemplo durante un mantenimiento [15].

### 2.5. Fatiga de alarmas y tormentas de alertas

El problema de tener demasiados avisos no es nuevo ni es solo de redes. El concepto de fatiga de alarmas viene del monitoreo de pacientes: es la falta de respuesta que aparece cuando hay tantas alarmas que el personal se satura y se desensibiliza [10]. En esa revisión, la fatiga se relaciona con una alta tasa de falsas alarmas, un bajo valor predictivo positivo, la falta de estandarización y la cantidad de equipos que alarman [10]. El valor predictivo positivo es la misma idea que la precisión de [4]: qué proporción de las alarmas era real.

En servicios informáticos aparece algo parecido, las tormentas de alertas: una falla viene acompañada de una gran cantidad de alertas, y eso hace muy difícil diagnosticarla a mano [11]. Zhao *et al.* hicieron el primer estudio empírico del tema con datos reales y propusieron detectar la tormenta y resumirla en unas pocas alertas representativas. En su evaluación redujeron en más de un 98% las alertas que había que revisar [11].

La fatiga también afecta a la seguridad. Tariq *et al.* revisaron la literatura y las soluciones de la industria sobre fatiga de alertas en los centros de operaciones de seguridad (SOC). Identifican cuatro causas principales y agrupan las soluciones en tres enfoques: automatización, ayuda al analista y colaboración entre personas e IA [12].

### 2.6. Tabla comparativa

| Estrategia | Cuándo avisa | Notificaciones innecesarias | Demora de detección | Punto débil | Fuentes |
|------------|--------------|-----------------------------|---------------------|-------------|---------|
| Umbral inmediato (primer fallo) | En cuanto se cumple la condición | Tiende a generar más: cualquier pico que cumpla la condición avisa | La menor de las opciones | Baja precisión | [4], [5] |
| Duración mínima (`for`, *N* fallos seguidos) | Si la condición se mantiene un tiempo o *N* comprobaciones | Puede generar menos: no avisa por picos más cortos que la duración | Mayor, y fija sin importar la gravedad | Fallas intermitentes pueden no avisar nunca | [4], [5] |
| Ventana larga | Si el error acumulado en muchas horas supera el umbral | Pocas en el análisis de [4] | Razonable | La alerta sigue activa mucho después de la solución | [4] |
| Tasa de consumo del presupuesto de error, con varias ventanas | Si el presupuesto se gasta rápido en la ventana larga y en la corta | Pocas en el análisis de [4] | Se adapta a la gravedad | Muchos parámetros para ajustar | [4] |
| Agrupación, inhibición y silencios (Alertmanager) | No decide cuándo alertar: gestiona las notificaciones de lo que ya se disparó | Agrupa duplicados y avisos relacionados | No la cambia | Hay que configurar bien las relaciones entre alertas | [15] |
| Agregación y resumen inteligente | Agrupa o resume alertas con estadística, grafos o modelos de lenguaje | En los estudios citados redujo mucho el volumen | No la cambia | Evaluada con datos de grandes empresas; sus propuestas necesitan revisión humana | [11], [13], [14] |

### 2.7. Limitaciones del estado actual

Las estrategias más completas necesitan un SLO bien definido y bastante tráfico. Con pocos datos, un solo error pesa demasiado: si un servicio recibe 10 solicitudes por hora y falla una, la tasa de error es del 10%, y con un SLO de 99,9% eso dispararía un aviso inmediato [4]. Además, herramientas como Alertmanager se concentran en configurar y disparar reglas, pero no evalúan si esas reglas son buenas [14].

---

## 3. Innovaciones emergentes

Los trabajos de los últimos años ya no buscan solo el umbral correcto. Ahora intentan trabajar sobre las alertas que ya se generaron: agruparlas, resumirlas y mejorar las reglas que las producen.

**Agregación de alertas con conocimiento operativo (COLA, 2024).** Kuang *et al.* parten de un problema concreto: cuando falla un sistema en la nube, se dispara una avalancha de alertas relacionadas que nadie puede revisar una por una [13]. Los métodos que había comparaban el texto de las alertas o usaban estadística. El problema es que dos alertas relacionadas pueden estar escritas de forma muy distinta, y la estadística anda mal con alertas que aparecen poco. COLA combina las dos cosas: un módulo estadístico resuelve los casos claros y solo los pares dudosos pasan a un modelo de lenguaje, que razona usando los procedimientos operativos estándar de cada alerta [13]. En tres conjuntos de datos reales de un proveedor de nube obtuvo valores F1 de entre 0,901 y 0,930, y en esas pruebas superó a los métodos con los que se comparó [13]. Lo nuevo es que, además de los datos de las alertas, usa lo que dejaron escrito los operadores.

**Gestión del ciclo de vida de las alertas (AlertGuardian, 2025).** Yu *et al.* plantean que la fatiga de alertas viene de una mala gestión de todo el ciclo de vida: cómo se crea la regla, cuándo se dispara, cómo se atiende y cómo se mejora después [14]. AlertGuardian trabaja en tres etapas. Primero filtra el ruido con un modelo de grafos, después resume las alertas con un modelo de lenguaje que busca información relevante y, por último, usa varios agentes para proponer mejoras a las reglas [14]. En cuatro conjuntos de datos reales redujo las alertas en un 94,8% y acertó el 90,5% de los diagnósticos de fallas. Además propuso mejoras para 1.174 reglas, de las cuales los ingenieros aceptaron 375, un 32% [14]. Este último dato muestra que la herramienta propone, pero la decisión sigue siendo humana.

**Seguridad y registros (2023–2025).** En los SOC, la revisión de Tariq *et al.* ubica la colaboración entre analistas e IA como una de las tres líneas para combatir la fatiga [12]. Y el borrador de 2023 de la revisión 1 del SP 800-92 de NIST, que todavía no tiene versión final, se enfoca en planificar la gestión de registros en toda la organización, no en detalles técnicos [9].

COLA y AlertGuardian buscan bajar la cantidad de avisos sin perder información. La diferencia con lo que vimos en la sección 2 es que no actúan solo en el momento de decidir si se avisa, sino también antes y después.

---

## 4. Oportunidades y desafíos

### 4.1. Definir qué requiere intervención

Ninguna estrategia de alertas se puede evaluar sin decidir antes qué evento es importante. La precisión y la exhaustividad dependen de esa definición [4], y para los autores esa definición sale del SLO [3], [4]. En la práctica no siempre está claro. Y elegir un objetivo no es solo una cuestión técnica: bajar el SLO o alargar la ventana de alerta es una decisión de producto, con ventajas y costos [4].

### 4.2. Servicios con poco tráfico y fallas intermitentes

Dos situaciones siguen sin una solución sencilla. La primera son los servicios con poco tráfico, donde una sola falla cambia mucho la tasa de error [4]. La segunda son las fallas que van y vienen: las estrategias basadas en duración mínima pueden no avisar nunca si la condición se corta antes de llegar al umbral [4]. Prometheus ofrece `keep_firing_for`, que mantiene la alerta activa un tiempo después de que la condición deja de cumplirse [5]. Eso ayuda una vez que la alerta ya se disparó, pero no resuelve el caso de una alerta que nunca llega a dispararse.

### 4.3. Seguridad de los registros y de las alertas

Las alertas salen de los registros. Si los registros no son confiables, los avisos tampoco. La guía de NIST de 2006 define la gestión de registros como el proceso de generarlos, transmitirlos, guardarlos, analizarlos y descartarlos, y le dedica un apartado específico a su protección [16]. El RFC 5424, que define el protocolo syslog, trata en sus consideraciones de seguridad problemas como la reproducción de mensajes viejos (*replay*), la entrega confiable, la integridad, la observación de los mensajes por terceros y la denegación de servicio [8]. También define campos para indicar la calidad de la hora de cada mensaje, por ejemplo si el reloj estaba sincronizado [8]. Esto importa porque, sin una hora confiable, no se puede calcular cuánto tardó una alerta ni reconstruir qué pasó.

El otro lado del problema es la fatiga en los equipos de seguridad, que ya tiene revisiones sistemáticas dedicadas [12]. El razonamiento es el mismo que en operación: cuando hay demasiados avisos, uno real se puede pasar por alto [2]. Por eso reducir el ruido no es solo una cuestión de comodidad, sino también de seguridad.

### 4.4. Inteligencia artificial con supervisión humana

Las herramientas basadas en modelos de lenguaje muestran buenos resultados, pero fueron evaluadas con datos de grandes empresas [13], [14]. Además, solo una parte de sus propuestas termina siendo aceptada por los ingenieros [14]. Quedan abiertas preguntas sobre cómo validarlas, cómo adaptarlas a organizaciones más chicas y cómo repartir las decisiones entre la herramienta y las personas.

### 4.5. Oportunidades

Estos desafíos abren espacio para trabajar en el diseño de reglas de alerta, la definición de objetivos de servicio y la gestión segura de registros: tareas que combinan redes, operación y seguridad.
