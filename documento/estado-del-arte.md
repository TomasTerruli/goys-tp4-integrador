# Estado del arte, innovaciones y desafíos

> **Responsable:** Pilar Giannelli
> Secciones 2, 3 y 4 del artículo (plantilla de la modalidad B). La numeración de las citas sigue `documento/referencias.md`.

---

## 2. Estado del arte

### 2.1. De la métrica al aviso

El monitoreo consiste en juntar y mostrar en tiempo real datos numéricos de un sistema, como la cantidad de solicitudes, los errores o los tiempos de respuesta [2, cap. 6]. Pero una métrica sola no le avisa a nadie. Para eso están las alertas: notificaciones para una persona, que pueden llegar como ticket, como correo o como *page*, el aviso más urgente, que va directo a quien está de guardia [2, cap. 6].

El monitoreo de caja blanca usa datos internos del sistema. El de caja negra, en cambio, prueba desde afuera lo mismo que vería un usuario. Este último detecta síntomas que ya están pasando, así que sirve para avisos urgentes, aunque no anticipa fallas [2, cap. 6]. Para elegir qué mirar, el mismo capítulo propone cuatro "señales doradas": latencia, tráfico, errores y saturación [2, cap. 6].

El ping que usamos en la simulación es una prueba de este tipo: mira desde afuera si el otro equipo responde. Hay que tener en cuenta que ICMP no garantiza la entrega: ni el datagrama ni el mensaje de control tienen asegurado que lleguen [3]. Por eso un ping perdido, por sí solo, no alcanza para decir que hay una caída.

### 2.2. Alertas accionables

No toda condición anormal merece un aviso. Según el capítulo sobre monitoreo del libro de Google, un *page* tiene que reservarse para situaciones urgentes, sobre las que se pueda actuar y que afecten a los usuarios. Además, cada aviso debería necesitar criterio humano, no una respuesta automática, y tratar un problema nuevo [2, cap. 6]. La razón es práctica: una persona puede responder con verdadera urgencia pocas veces por día antes de cansarse. Si una regla genera avisos que siempre se pueden ignorar porque no pasa nada, no debería despertar a nadie [2, cap. 6].

Cuando hay demasiadas alertas o tienen poco valor, la gente empieza a leerlas por arriba o a ignorarlas, y así se puede pasar por alto un aviso real y alargar una caída [2, cap. 6]. Por eso el capítulo resume la meta como tener mucha señal y poco ruido [2, cap. 6].

### 2.3. SLI y SLO como criterio para decidir

Para saber si una falla es importante hace falta contra qué compararla. Para eso están los objetivos de servicio. Un SLI es una medida numérica bien definida de algún aspecto de la calidad del servicio, como la latencia, la tasa de errores o la disponibilidad. Un SLO es el valor objetivo, o el rango, que se espera para ese SLI. Un SLA es un acuerdo con los usuarios que tiene consecuencias si no se cumple [2, cap. 4].

El mismo libro recomienda empezar por lo que les importa a los usuarios y no por lo que es fácil de medir. También dice que no hay que exigir el 100%, porque eso puede frenar la innovación y salir caro. Lo que se hace es aceptar un margen en el que el SLO puede no cumplirse, que se llama presupuesto de error [2, cap. 4]. Esto cambia la pregunta de cuándo alertar. Ya no se trata de avisar ante cualquier anomalía, sino cuando algo pone en riesgo el objetivo.

### 2.4. Estrategias para activar una alerta

Thurgood *et al.* proponen cuatro atributos para evaluar una estrategia de alertas [4]:

- **Precisión:** qué parte de las alertas corresponde a eventos importantes.
- **Exhaustividad (*recall*):** qué parte de los eventos importantes generó una alerta.
- **Tiempo de detección:** cuánto tarda en salir la notificación.
- **Tiempo de restablecimiento:** cuánto sigue activa la alerta después de que se solucionó el problema.

Con esos atributos comparan seis estrategias, de la más simple a la más completa [4]. La más simple avisa apenas la tasa de errores supera el objetivo en una ventana corta. Tiene buen tiempo de detección y buena exhaustividad, pero poca precisión: en su ejemplo podría disparar hasta 144 alertas por día aunque el servicio siga cumpliendo el SLO [4].

Otra estrategia exige que la condición dure un tiempo mínimo antes de avisar. Así puede ganar precisión, porque no avisa por los picos más cortos que esa duración. A cambio pierde exhaustividad y tarda más en detectar [4]. El problema es que la espera es la misma sin importar la gravedad: una caída total tarda lo mismo en avisar que una falla del 0,2%. Además, si la métrica se normaliza un momento, el contador vuelve a cero, así que una falla que va y viene puede no avisar nunca. Por eso los autores no la recomiendan como criterio para alertas sobre SLO [4]. Esta estrategia es la que más se parece a nuestro criterio 2, el de varios fallos seguidos.

Las estrategias más avanzadas miran la tasa de consumo del presupuesto de error, o sea, qué tan rápido se gasta el margen permitido. La que recomiendan combina varias tasas con una ventana larga y una corta, y solo avisa si las dos superan el umbral. Por ejemplo, para un SLO de 99,9%, un *page* si se consume el 2% del presupuesto en una hora y un ticket si se consume el 10% en tres días. La desventaja es que tiene muchos parámetros para ajustar [4].

Las herramientas actuales usan estas ideas. En Prometheus el trabajo está dividido en dos partes. Por un lado, Prometheus evalúa las reglas de alerta. Una regla sin la cláusula `for` se activa en la primera evaluación en que se cumple la condición. Con `for`, la alerta queda "pendiente" y recién pasa a "disparada" si la condición se mantiene durante ese tiempo [5]. También existe `keep_firing_for`, que deja la alerta activa un rato después de que la condición dejó de cumplirse [5]. Por otro lado, la documentación aclara que estas reglas no son un sistema completo de notificaciones: hace falta otra capa que resuma los avisos, limite cuántos se mandan, permita silenciarlos y tenga en cuenta las dependencias entre alertas. Esa capa es Alertmanager [5].

### 2.5. Fatiga de alertas

El problema de tener demasiados avisos no es solo de la operación de redes. También afecta a la seguridad. Tariq *et al.* revisaron la literatura y las soluciones de la industria sobre fatiga de alertas en los centros de operaciones de seguridad (SOC). Identifican cuatro causas principales y agrupan las soluciones en tres enfoques: automatización, ayuda al analista y colaboración entre personas e IA [6].

### 2.6. Tabla comparativa

En la tabla separamos dos cosas que a veces se mezclan: cuándo se **activa** una alerta y cómo se **entrega** la notificación. Las primeras cuatro estrategias deciden la activación; las dos últimas trabajan sobre alertas que ya se activaron.

| Estrategia | Qué controla | Cuándo actúa | Efecto en la cantidad de notificaciones | Efecto en la demora | Limitaciones | Fuentes |
|------------|--------------|--------------|-----------------------------------------|---------------------|--------------|---------|
| Umbral inmediato (primer fallo) | Activación | En la primera evaluación en que se cumple la condición | Avisa por cualquier pico que cumpla la condición; en el ejemplo de [4], hasta 144 alertas por día aunque se cumpla el SLO | Avisa apenas se cumple la condición; la demora depende de cada cuánto se evalúa la regla | En el análisis de [4] tiene poca precisión | [4], [5] |
| Duración mínima (`for`, *N* fallos seguidos) | Activación | Cuando la condición se mantiene un tiempo o durante *N* comprobaciones | Puede bajar, porque no avisa por picos más cortos que esa duración | Suma la espera elegida, igual para cualquier gravedad | Una falla que va y viene puede no activar nunca la alerta | [4], [5] |
| Ventana larga | Activación | Cuando el error acumulado en una ventana larga supera el umbral | En el análisis de [4], mejora la precisión frente al umbral inmediato | Aceptable en ese análisis | La alerta sigue activa mucho después de la solución | [4] |
| Tasa de consumo del presupuesto de error, con varias ventanas | Activación | Cuando el presupuesto se gasta rápido en una ventana larga y en una corta | En el análisis de [4], buena precisión | Más corta cuanto más grave es la falla | Muchos parámetros para ajustar | [4] |
| Capa de notificaciones (Alertmanager) | Entrega | Después de que la alerta se activó | Puede resumir, limitar y silenciar notificaciones | No cambia cuándo se activa la alerta | Hay que configurar bien las dependencias entre alertas | [5] |
| Agregación y resumen con IA (COLA, AlertGuardian) | Entrega y, en AlertGuardian, mejora de las reglas | Sobre las alertas ya activadas | En sus evaluaciones redujeron mucho la cantidad de alertas a revisar | No cambia cuándo se activa cada alerta | Probadas con datos de grandes empresas; las propuestas necesitan revisión humana | [7], [8] |

### 2.7. Limitaciones del estado actual

Las estrategias más completas necesitan un SLO bien definido y bastante tráfico. Con pocos datos, un solo error pesa demasiado: si un servicio recibe 10 solicitudes por hora y falla una, la tasa de error es del 10%, y con un SLO de 99,9% eso dispararía un aviso inmediato [4]. Además, herramientas como Alertmanager se concentran en configurar y disparar reglas, pero no evalúan si esas reglas son buenas [8].

---

## 3. Innovaciones emergentes

Los trabajos de los últimos años ya no buscan solo el umbral correcto. Intentan trabajar sobre las alertas que ya se generaron: agruparlas, resumirlas y mejorar las reglas que las producen.

**Agregación de alertas con conocimiento operativo (COLA, 2024).** El problema que ataca es concreto: cuando falla un sistema en la nube, se dispara una avalancha de alertas relacionadas que nadie puede revisar una por una [7]. Los métodos que había comparaban el texto de las alertas o usaban estadística. Pero dos alertas relacionadas pueden estar escritas de forma muy distinta, y la estadística anda mal con alertas que aparecen poco. COLA combina las dos cosas: un módulo estadístico resuelve los casos claros y solo los pares dudosos pasan a un modelo de lenguaje, que razona con los procedimientos operativos de cada alerta [7]. En tres conjuntos de datos reales de un proveedor de nube obtuvo valores F1 de entre 0,901 y 0,930, y en esas pruebas superó a los métodos con los que se comparó [7]. Lo nuevo es que, además de los datos de las alertas, usa lo que dejaron escrito los operadores.

**Gestión del ciclo de vida de las alertas (AlertGuardian, 2025).** Yu *et al.* plantean que la fatiga de alertas viene de manejar mal todo el ciclo de vida de una alerta: cómo se crea la regla, cuándo se dispara, cómo se atiende y cómo se mejora después [8]. AlertGuardian trabaja en tres etapas. Primero filtra el ruido con un modelo de grafos, después resume las alertas con un modelo de lenguaje que busca información relevante y, por último, usa varios agentes para proponer mejoras a las reglas [8]. En cuatro conjuntos de datos reales redujo las alertas en un 94,8% y acertó el 90,5% de los diagnósticos de fallas. Además propuso mejoras para 1.174 reglas, de las cuales los ingenieros aceptaron 375, un 32% [8]. Lo innovador es que no se queda en el momento del aviso: vuelve sobre las reglas para que el problema no se repita. Y ese 32% muestra que la herramienta propone, pero la decisión sigue siendo humana.

**Colaboración entre personas e IA en seguridad (2025).** La revisión de Tariq *et al.* sobre fatiga de alertas en los SOC no busca solo un umbral mejor. Revisa las soluciones de la investigación y de la industria desde tres enfoques: automatización, ayuda al analista y colaboración entre personas e IA [6]. A partir de esa revisión identifica cuatro causas principales de la fatiga, analiza qué les falta a las soluciones actuales y propone líneas de investigación que usan IA [6]. Los autores esperan que esas ideas sirvan también fuera de los SOC, en otros ámbitos de mando y control [6].

Las tres propuestas apuntan a bajar la cantidad de avisos sin perder información. La diferencia con lo que vimos en la sección 2 es que no actúan solo en el momento de decidir si se avisa, sino también antes y después.

---

## 4. Oportunidades y desafíos

### 4.1. Definir qué requiere intervención

Ninguna estrategia de alertas se puede evaluar sin decidir antes qué evento es importante. La precisión y la exhaustividad dependen de esa definición [4], y esa definición sale del SLO [2, cap. 4], [4]. En la práctica no siempre está claro. Y elegir un objetivo no es solo una cuestión técnica: bajar el SLO o alargar la ventana de alerta es una decisión de producto, con ventajas y costos [4].

### 4.2. Servicios con poco tráfico y fallas intermitentes

Dos situaciones siguen sin una solución sencilla. La primera son los servicios con poco tráfico, donde una sola falla cambia mucho la tasa de error [4]. La segunda son las fallas que van y vienen: las estrategias basadas en duración mínima pueden no avisar nunca si la condición se corta antes de llegar al umbral [4]. Prometheus ofrece `keep_firing_for`, que mantiene la alerta activa un tiempo después de que la condición deja de cumplirse [5]. Eso ayuda una vez que la alerta ya se disparó, pero no resuelve el caso de una alerta que nunca llega a dispararse.

### 4.3. Seguridad de los registros y de las alertas

Las alertas salen de los registros. Si los registros no son confiables, los avisos tampoco. La guía de NIST advierte que un registro mal protegido, guardado o en tránsito, se puede alterar o destruir, y que así una actividad maliciosa puede pasar desapercibida [10]. Las fuentes que revisamos proponen medidas concretas en cuatro puntos:

- **Integridad.** NIST recomienda calcular un resumen (*hash*) de cada archivo de registro y guardarlo aparte, protegido, para poder detectar si alguien lo cambió [10]. En syslog, el RFC 5424 aclara que el protocolo no detecta mensajes viejos reenviados (*replay*) ni impide que alguien en el medio modifique un mensaje, y menciona la firma criptográfica como una forma de evitar que se alteren las marcas de tiempo [9].
- **Acceso y confidencialidad.** Los registros pueden tener datos sensibles, como contraseñas o contenido de correos [10]. Por eso conviene limitar quién puede verlos y proteger el tráfico de registros con una red separada o con cifrado [10]. Syslog por sí solo no da confidencialidad: el RFC 5424 recomienda usar el transporte con TLS [9].
- **Conservación.** Los archivos de registro tienen un tamaño máximo. Cuando se llenan, sobrescriben lo más viejo o dejan de registrar, así que para cumplir los plazos de conservación hace falta archivarlos, y lo archivado también tiene que estar protegido [10]. Si los registros pueden servir como evidencia, NIST recomienda guardar también los originales [10].
- **Hora confiable.** Cada equipo pone la hora con su propio reloj. Si el reloj está mal, después no se puede reconstruir el orden de los eventos, por eso NIST recomienda sincronizarlos, por ejemplo con NTP [10]. El RFC 5424 incluye un campo para indicar si el equipo estaba sincronizado con una fuente de hora externa [9].

En nuestra simulación, por ejemplo, los registros no tienen hora, así que la demora de detección solo se puede medir en comprobaciones y no en segundos.

El otro lado del problema es la fatiga en los equipos de seguridad, que ya tiene revisiones sistemáticas dedicadas [6]. El razonamiento es el mismo que en operación: cuando hay demasiados avisos, uno real se puede pasar por alto [2, cap. 6]. Por eso reducir el ruido no es solo una cuestión de comodidad, sino también de seguridad.

### 4.4. Inteligencia artificial con supervisión humana

Las herramientas basadas en modelos de lenguaje muestran buenos resultados, pero fueron evaluadas con datos de grandes empresas [7], [8], y solo una parte de sus propuestas termina siendo aceptada por los ingenieros [8]. La revisión de Tariq *et al.* también propone usar IA, pero analiza las limitaciones de las soluciones que ya existen [6]. Quedan abiertas preguntas sobre cómo validar estas herramientas, cómo adaptarlas a organizaciones más chicas y cómo repartir las decisiones entre la herramienta y las personas.

### 4.5. Oportunidades

Estos desafíos dejan lugar para trabajar en el diseño de reglas de alerta, en la definición de objetivos de servicio y en la gestión segura de registros. Son tareas que mezclan redes, operación y seguridad.
