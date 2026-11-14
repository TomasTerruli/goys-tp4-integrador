# De las métricas a las acciones: calidad de las alertas y detección de fallas en servicios

> **TP4 — Proyecto Integrador** · Gestión Operativa y Seguridad en Redes (GOYS) · UTN FR La Plata
> **Modalidad B:** Artículo de Revisión del Estado del Arte, con una mini simulación en GNS3.

## Integrantes

| Integrante | Usuario de GitHub | Qué hace cada uno |
|------------|-------------------|-------------------|
| Pilar Giannelli | `pilargiannelli` | Busca y revisa las fuentes, redacta la parte de investigación y hace la revisión académica |
| Tomás Terruli | `TomasTerruli` | Analiza los resultados de la simulación, integra el artículo, arma el póster y cierra el repositorio |
| Martina Castellani | `Marticaste144` | Arma y ejecuta la simulación, organiza el artículo y cierra la demo |

Los resultados los interpretamos entre los tres y las conclusiones las acordamos juntos.

## Tema

Observabilidad de servicios: criterios de alertado para detectar fallas y reducir notificaciones innecesarias. Queremos ver cómo pasar de las métricas a avisos que realmente le sirvan a quien opera una red o un servicio.

## Pregunta de investigación

¿Cómo influyen los criterios de activación de alertas en la cantidad de notificaciones y en el tiempo de detección de una falla?

## Objetivo

Revisar qué se sabe hoy sobre la calidad de las alertas (alertas accionables, SLI/SLO, fatiga de alarmas e innovaciones recientes) y sumarle una mini simulación donde comparamos dos criterios de alerta sobre los mismos registros de conectividad:

- **Criterio 1:** avisar con el primer fallo.
- **Criterio 2:** avisar recién después de varios fallos seguidos.

Para cada criterio contamos cuántas notificaciones se emiten, cuántos incidentes se detectan y cuánto tarda en detectarlos.

## Alcance

- **Revisión bibliográfica:** observabilidad, estrategias de alertado, SLI/SLO, fatiga de alarmas, innovaciones de los últimos tres años, seguridad y protección de los registros, y desafíos. El artículo cita 10 fuentes verificadas: documentación oficial (OpenTelemetry y Prometheus), los libros de Google sobre SRE, los RFC 792 y 5424, la guía NIST SP 800-92 y tres artículos académicos (COLA, AlertGuardian y Tariq *et al.*). El relevamiento completo está en `documento/fuentes-investigacion.md`.
- **Mini simulación:** dos equipos VPCS conectados a un switch en GNS3. Desde PC1 ejecutamos `ping` hacia PC2 (30 comprobaciones por prueba) y provocamos cortes manuales del enlace. Obtuvimos nueve registros: tres sin cortes, tres intentos de corte breve, dos cortes sostenidos y una ejecución exploratoria que presentamos aparte.
- **Límites:** la simulación complementa la revisión y no se generaliza a redes reales. Los dos criterios se aplicaron manualmente sobre los registros; no implementamos un sistema de alertas automático.

El detalle está en [`documento/alcance.md`](documento/alcance.md).

## Resultado principal

Sobre las ocho pruebas de la comparación principal (5 incidentes), avisar ante el primer fallo generó **5 notificaciones** y esperar tres fallos consecutivos generó **3**. La diferencia aparece solo en los cortes breves; en los sostenidos, los dos criterios detectaron el incidente y el segundo lo hizo dos comprobaciones más tarde. Ningún criterio es mejor en general: depende de qué interrupciones requieren intervención, algo que debería salir de un objetivo de servicio.

## Entregables

| Entregable | Archivo final | Editable |
|------------|---------------|----------|
| Artículo de revisión (7 páginas + portada) | [`documento/articulo-grupo07.pdf`](documento/articulo-grupo07.pdf) | [`documento/articulo-grupo07.docx`](documento/articulo-grupo07.docx) |
| Póster A1 (infografía) | [`poster/poster-grupo07.pdf`](poster/poster-grupo07.pdf) | [`poster/poster-grupo07.html`](poster/poster-grupo07.html) |
| Mini simulación | [`demo/README.md`](demo/README.md), [`demo/analisis.md`](demo/analisis.md), registros en [`demo/resultados/`](demo/resultados/) | Proyecto [`demo/GOYS-Observabilidad-Grupo07.gns3project`](demo/GOYS-Observabilidad-Grupo07.gns3project) |
| Topología y capturas | [`diagramas/topologia.png`](diagramas/topologia.png), [`capturas/`](capturas/) | — |
| Bitácora y checklist | [`progreso.md`](progreso.md), [`checklist-entrega.md`](checklist-entrega.md) | — |

## Cómo ejecutar la demo

Requisitos: GNS3 2.2.x con VPCS (probado con GNS3 2.2.61 en Windows y VPCS 0.6.2). No necesita internet.

1. En GNS3: **File → Import portable project** y elegir `demo/GOYS-Observabilidad-Grupo07.gns3project`.
2. Iniciar todos los equipos (botón ▶).
3. Abrir la consola de PC1 y de PC2 y comprobar las direcciones con `show ip` (PC1 `192.168.10.1/24`, PC2 `192.168.10.2/24`).
4. Desde PC1 ejecutar:
   ```
   ping 192.168.10.2 -c 30 -i 1000 -w 500
   ```
5. Para provocar un corte: clic derecho sobre el enlace Switch1–PC2 → **Suspend**, y después **Resume**. Los pings perdidos aparecen como `timeout`.
6. Aplicar los criterios sobre la salida: **C1** avisa en el primer `timeout` y **C2** en el tercer `timeout` seguido (ver [`demo/analisis.md`](demo/analisis.md)).

Si la demo falla el día de la presentación, los registros originales de las nueve pruebas están en `demo/resultados/` y las capturas en `capturas/`.

## Archivos de trabajo

| Archivo | Qué tiene |
|---------|-----------|
| [`documento/estructura.md`](documento/estructura.md) | Organización del artículo usada para integrarlo |
| [`documento/introduccion.md`](documento/introduccion.md) | Introducción (versión de trabajo) |
| [`documento/estado-del-arte.md`](documento/estado-del-arte.md) | Estado del arte, innovaciones, oportunidades y desafíos (versión extendida) |
| [`documento/metodologia-bibliografica.md`](documento/metodologia-bibliografica.md) | Cómo buscamos y elegimos las fuentes |
| [`documento/referencias.md`](documento/referencias.md) y [`documento/fuentes-investigacion.md`](documento/fuentes-investigacion.md) | Referencias y relevamiento de fuentes |
| [`documento/revision-protocolo.md`](documento/revision-protocolo.md) | Revisión de los datos y opciones para definir qué requiere intervención |
| [`poster/hallazgos.md`](poster/hallazgos.md) | Hallazgos seleccionados para el póster |

El artículo final numera las citas por orden de aparición (formato IEEE); los archivos de trabajo conservan la numeración de `documento/referencias.md`.

## Cómo trabajamos en GitHub

Trabajamos en un único fork del grupo (`TomasTerruli/goys-tp4-integrador`) y en una sola rama compartida, `grupo-07`. Antes de empezar, cada uno trae los cambios de los demás con `git pull origin grupo-07`; al terminar, hace su propio commit y lo sube a la misma rama. Cuando el trabajo esté completo, se abre un único Pull Request desde `grupo-07` hacia el repositorio de la cátedra (`rodriguezemautn/goys-tp4-integrador`, rama `main`), sin hacer merge.

---

## Guía de la cátedra

> **GOYS** — Gestión Operativa y Seguridad en Redes · UTN FR La Plata
> **Peso:** 60% de la nota del bloque Redes/Operación
> **Muestra:** Feria de Redes (última clase del bloque)

Este es el **repositorio base del Trabajo Práctico Integrador (TP4)**. Un integrante del grupo lo **forkea**, agrega al resto como **colaboradores**, y todo el trabajo colaborativo del TP4 se hace acá.

---

### 1. ¿Qué es el TP4?

El TP4 integra **las 3 áreas de la materia** —**Redes**, **Operación (Gestión)** y **Seguridad**— en un proyecto con mayor autonomía:

| Área | Qué cubre |
|------|-----------|
| 🔌 **Redes** | Topología, routing (BGP/OSPF), switching, VXLAN, containers, L2/L3 |
| ⚙️ **Operación (Gestión)** | Automatización (Ansible), monitoreo (Prometheus/Grafana), orquestación (K3s), GitOps |
| 🛡️ **Seguridad** | Segmentación, network policies, mTLS, Zero Trust, Cilium/Hubble |

Se muestra al final en una **Feria de Redes** (formato congreso): cada grupo monta una **estación** con póster, documento y demo funcionando, y la defiende frente a docentes, compañeros e invitados.

Cada grupo elige **UNA** de las 3 modalidades (sección 2).

---

### 2. Modalidades (elegir UNA)

| Modalidad | Qué es | Template |
|-----------|--------|----------|
| **A — Proyecto de Desarrollo** | Diseñar e implementar una solución que integre redes, containers, automatización y monitoreo | `templates/template-modalidad-a.md` |
| **B — Artículo de Revisión del Estado del Arte** | Investigar el estado del arte de una tecnología, sus innovaciones emergentes y oportunidades | `templates/template-modalidad-b.md` |
| **C — Trabajo de Campo** | Analizar una red real (o caso de estudio) y proponer mejoras | `templates/template-modalidad-c.md` |

#### ¿Cómo integra cada modalidad las 3 áreas?

| Área | A — Desarrollo | B — Estado del Arte | C — Trabajo de Campo |
|------|----------------|---------------------|----------------------|
| 🔌 **Redes** | Topología + routing + containers **implementados** | Estado del arte de una tecnología de red | Relevamiento de la red real |
| ⚙️ **Operación** | Ansible + monitoreo **automatizados** | Network automation, observabilidad | Propuesta de automatización/monitoreo |
| 🛡️ **Seguridad** | Network policies, segmentación, mTLS | Zero Trust, eBPF security, mTLS | Segmentación + mejoras de seguridad |

> ⚠️ **La seguridad NO es opcional en ninguna modalidad.** Sin importar cuál elijan, el proyecto debe mostrar cómo aborda la seguridad de la red.

> 📖 El detalle de cada modalidad está en su template. El formato del documento (portada UTN, A4, estilo) está en `templates/template-documento.md`.

---

### 3. Arrancar: fork + grupo + colaboradores

#### 3.1. Un integrante hace el fork

1. Ese integrante entra a este repositorio y hace **Fork** (botón arriba a la derecha).
2. Ahora tiene una copia en su cuenta: `tu-usuario/goys-tp4-integrador`.

#### 3.2. Sumar colaboradores (compañeros + docentes)

1. En el fork, ir a **Settings → Collaborators and teams → Add people**.
2. Agregar a **cada integrante** del grupo (por su usuario de GitHub).
3. Agregar también a los **docentes** para que puedan revisar el trabajo:
   - `ofalabel` — **Osvaldo Falabella** (titular de la materia)
   - `rodriguezemautn` — **Emanuel Rodríguez** (bloque Redes/Operación)
4. Cada uno acepta la invitación (llega por mail/notificación).

> ⚠️ **Todos los integrantes deben aparecer como colaboradores y con commits propios.** Si alguien no aparece en el historial de git, no acredita el TP.

#### 3.3. Clonar y configurar identidad

```bash
# Cada integrante clona el fork
git clone https://github.com/<grupo>/goys-tp4-integrador.git
cd goys-tp4-integrador

# Configurar identidad (usar el mail de la facultad)
git config user.name "Apellido, Nombre"
git config user.email "tu-email@alumnos.frlp.utn.edu.ar"
```

---

### 4. Estructura del repositorio

```
goys-tp4-integrador/
├── README.md              # esta guía
├── progreso.md            # bitácora del grupo (qué hizo cada uno)
├── templates/             # templates de modalidad + documento + checklist
├── documento/             # documento final (PDF + fuente)
├── poster/                # póster A1 (editable + PDF) + guía
├── demo/                  # código/scripts de la demo (estación)
├── capturas/              # capturas y evidencias
└── diagramas/             # diagramas de arquitectura/topología
```

---

### 5. Trabajo colaborativo (cómo laburar en grupo)

#### Flujo recomendado (ramas cortas + pull requests)

```bash
# 1. Actualizar antes de empezar
git pull origin main

# 2. Rama por tarea
git checkout -b documento-introduccion

# 3. Trabajar y commitear seguido (mensajes claros)
git add .
git commit -m "documento: escribe introducción y objetivos"

# 4. Subir y abrir PR
git push origin documento-introduccion
# En GitHub: abrir Pull Request → otro integrante revisa → merge

# 5. Volver a main y repetir
git checkout main
git pull origin main
```

#### Reglas de oro

- **Commits de TODOS**: cada integrante commitea con su usuario. Verificar al final con `git log --oneline`.
- **No pushear directo a `main` sin revisar** (usar PRs para el documento y decisiones grandes).
- **Pull antes de push** para no pisar trabajo ajeno.
- **Nada de binarios/secretos**: no subir contraseñas, IPs privadas reales ni archivos pesados (ver `.gitignore`).

---

### 6. Entregables (comunes a las 3 modalidades)

| Elemento | Descripción |
|----------|-------------|
| 📄 **Documento** | 5-7 págs (modalidades A y B) o 6-8 (C), en PDF, en `documento/` |
| 🖼️ **Póster A1** | Sintetiza problema, metodología, resultados y conclusiones. Autocontenido. En `poster/` |
| 💻 **Demo / estación** | Funciona en la laptop del grupo, **offline**. En `demo/` |
| 📂 **Repositorio** | Documento, fuentes, configs, capturas, póster editable + PDF |

> ✅ Checklist completo en `templates/checklist-entrega.md`. Usarlo antes del ensayo y antes de la Feria.

---

### 7. Cronograma

| Momento | Qué pasa |
|---------|----------|
| Clase 2 (jueves 20/8) | Se forman los grupos (3-4), los mismos para toda la cursada |
| Durante la cursada | Cada clase alimenta el TP4; el grupo define modalidad y tema |
| Jueves 29/10 | Muestra TP3 + preparación/ensayos del TP4 |
| Jueves 5/11 | **Feria de Redes** — muestra y defensa |

---

### 8. Evaluación (60%)

La nota combina **dimensión técnica (60%)** y **"Saber Ser" (40%)** — trabajo en equipo, comunicación, compromiso, pensamiento crítico, ética.

- **Promoción directa:** TP4 >= 6 (con todos los TPs >= 6 y 75% de asistencia).
- **Cursada regular:** TP4 >= 4 (más el resto de condiciones).

> ⚠️ **Ausencia injustificada en la Feria = TP4 desaprobado.**

---

**GOYS — UTN FR La Plata** · Titular: Osvaldo Falabella · Bloque Redes/Operación: Emanuel Rodríguez
