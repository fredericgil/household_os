# CLAUDE.md — Plantilla de proyecto de análisis funcional

> Copia este archivo como `CLAUDE.md` en la raíz de cada nuevo proyecto de análisis funcional.
> Actualiza la sección de estructura con el nombre real del proyecto.
> **Versión de la plantilla: 2.0** — ver `CHANGELOG.md` en este repo (`sdd-template`) para el historial de cambios. Si mejoras esta plantilla en un proyecto concreto, retropropaga el cambio aquí y añade la entrada al changelog.

---

## Checklist de inicio de proyecto

Antes de escribir el primer documento:

- [ ] Repositorio creado con `.gitignore` configurado (excluye `.env`, `.env.local`, `*.pem`, `*.key`)
- [ ] `README.md` creado con texto provisional (se completará como README de portfolio al cerrar el proyecto)
- [ ] `decisions.md` creado con la plantilla de entrada vacía
- [ ] **Perfil de proyecto elegido y registrado en `decisions.md`** (ver sección "Perfiles de proyecto" más abajo)
- [ ] `docs/00_glosario.md` creado (puede estar vacío)
- [ ] `docs/01_brief_proyecto.md` creado con la estructura mínima
- [ ] `.env.example` creado si el proyecto tiene variables de entorno (valores de ejemplo, nunca reales)
- [ ] `LICENSE` creado (MIT para código; CC BY 4.0 si el entregable principal es documentación)
- [ ] Repositorio configurado en GitHub: descripción de una línea, topics relevantes (ej.: `analisis-funcional`, `portfolio`, `sdd`)
- [ ] Primer commit con solo la estructura vacía — antes de escribir contenido

---

## Perfiles de proyecto

No todo proyecto de portfolio necesita las diez fases completas. Antes de escribir el Project Brief, elegir un perfil y registrarlo en `decisions.md` como primera decisión del proyecto ("Perfil elegido: [Completo/Ligero] — razón").

### Perfil Completo

Para el proyecto central del portfolio: dominio de negocio real, múltiples roles/actores, al menos un flujo con reglas de negocio no triviales. Sigue las diez fases documentadas en este archivo sin recortes.

### Perfil Ligero

Para herramientas de apoyo, utilidades internas o exploración de una técnica concreta: alcance acotado a uno o dos roles, sin reglas de negocio complejas. Cambios respecto al perfil Completo:

- **00 Glosario, 01 Brief:** sin recortes — son los documentos que anclan todo lo demás.
- **02 User Stories:** la valoración INVEST (equivalente a 02b) se hace inline al final del mismo documento, no en archivo separado.
- **03 + 04 + 05 fusionados** en un único `03_requisitos.md`: reglas de negocio, requisitos funcionales en EARS, requisitos no funcionales, y una tabla de requisitos con columna MoSCoW y método de verificación (RTM y resumen MoSCoW combinados). Se conservan las señales de alarma del MoSCoW (no >80% Must, al menos un Won't justificado).
- **06 Modelo de datos:** se omite como archivo si el dominio tiene una sola entidad simple; si hay ≥2 entidades con relaciones o una entidad con ciclo de vida propio, se documenta igual que en el perfil Completo.
- **07 Mapa de flujos:** se omite como archivo si todos los flujos son lineales (sin puntos de decisión con ramas divergentes); en ese caso los pasos se documentan como criterios de aceptación de las user stories. Si algún flujo tiene ramas, se documenta igual que en el perfil Completo.
- **08 Especificación de interfaz:** se omite si el proyecto no tiene UI o tiene una sola vista trivial (ej. un CLI de una acción).
- **09 Plan de verificación:** sin recortes de disciplina, pero puede ser una tabla más corta si hay pocos requisitos Must.

Regla general: fusionar u omitir un documento es una decisión, no una omisión silenciosa — se declara en `decisions.md` antes de escribir el Brief. Si a mitad de proyecto se descubre que el alcance creció más allá de lo previsto para un perfil Ligero, se registra el cambio de perfil como nueva decisión y se retoman las fases separadas desde ese punto.

---

## Metodología

Este proyecto sigue **Spec Driven Development**. No se avanza a una fase sin haber completado, revisado y marcado como baseline los artefactos de la fase anterior.

### Ciclo de vida de cada documento

```
borrador → revisión → baseline
```

- **borrador:** primera redacción completa.
- **revisión:** relectura con ojos de stakeholder — identificar inconsistencias, vacíos y ambigüedades antes de avanzar.
- **baseline:** aprobado por el usuario. El campo `Estado:` de la cabecera cambia a `baseline [fecha]`. Solo desde baseline se avanza a la siguiente fase.

El campo `Estado:` en la cabecera de cada documento refleja en cuál de los tres estados se encuentra. Ejemplo: `Estado: baseline 2026-07-01`.

### Secuencia de fases (perfil Completo; ver variantes del perfil Ligero arriba)

```
Project Brief          ←── incluye diagrama de contexto y consideración de privacidad
     │
     ├── Glosario      ←── documento vivo; empieza aquí y crece con cada fase
     ↓
User Stories
     └──→ Revisión INVEST (02b)  ←── cierra la fase; ninguna historia avanza sin superarla
     ↓
BRD / PRD              ←── sin sección de flujos; puede incluir visión general en prosa
     ↓
Matriz de requisitos   ←── RTM real; incluye columna MoSCoW y método de verificación para RF y RNF
     ↓
Resumen MoSCoW         ←── 1 página; cierra el loop: limpia downstream de Won't/Could
     ↓
Modelo de datos        ←── solo entidades de Must + Should
     ↓
Mapa de flujos         ←── fuente única de autoridad sobre flujos
     ↓
Especificación de interfaz
     ↓
Plan de verificación   ←── cierra el ciclo entre requisitos y pruebas
     ↓
── Consistencia entre documentos ──   ←── verificación cruzada antes de escribir código
     ↓
Código
     ↓
Revisión spec-vs-implementación  ←── compila gaps del desarrollo + detecta gaps nuevos
     ↓
README de portfolio    ←── orienta al evaluador externo; último paso
```

### Consistencia entre documentos

Antes de escribir código, verificar las tres trazas:

1. ¿Cada término de dominio introducido en cualquier documento aparece en el glosario?
2. ¿Cada RF de la RTM está cubierto por al menos una vista en la especificación de interfaz?
3. ¿Cada flujo del mapa de flujos es trazable a al menos un RF en la RTM?

Si alguna traza falla, corregir los documentos afectados antes de continuar.

### Gap durante implementación

Cuando durante el código se detecta que un requisito es incorrecto, imposible o diferente a lo especificado:

1. **Clasificar el gap:**
   - (a) Implementar diferente — la spec era incorrecta; se corrige y se implementa la versión correcta.
   - (b) Diferir — fuera del alcance de esta versión; se mueve a Should o se anota como deuda.
   - (c) Cancelar — el requisito no tiene valor suficiente para implementarse.
2. **Actualizar el documento de spec afectado** — antes de implementar el cambio.
3. **Registrar en `decisions.md`** con título "Gap detectado: [descripción]", la clasificación y la razón.
4. **Actualizar la RTM** si el cambio afecta al caso de prueba o al método de verificación.
5. Continuar la implementación.

No implementar un cambio sin haber actualizado la spec primero. El código debe seguir a los documentos, no al revés.

La revisión spec-vs-implementación al cierre compila todos los gaps ya registrados en `decisions.md` durante el desarrollo, más cualquier gap que no se detectó hasta ese momento.

---

## Estándares por documento

---

### 00 — Glosario

Documento vivo. Se abre con el Project Brief y se actualiza en cada fase.

**Formato de entrada estándar:**

```
**[Término]** — [Definición completa en una o dos frases].
Relacionado con: [término1], [término2].
Introducido en: [nombre del documento donde aparece por primera vez].
Actualizado: [fecha si la definición evolucionó respecto a la versión anterior].
```

**Qué debe contener:**
- Todos los términos de dominio específicos del proyecto: estados, roles, conceptos de proceso, nombres de entidades.
- Cualquier término que un lector externo podría malinterpretar.

**Reglas:**
- Ningún término de dominio nuevo se introduce en un documento sin añadirlo al glosario en la misma sesión.
- Antes de baselinar cualquier documento, Claude verifica que todos los términos de dominio que introduce están en el glosario.
- Si la definición de un término evoluciona, actualizar la entrada y añadir la fecha de actualización. No sobreescribir sin rastro.

---

### 01 — Project Brief

**Estructura mínima:**

1. **Problema** — qué ocurre hoy y por qué es un problema. Incluir al menos un dato o estimación que cuantifique el impacto ("el proceso consume X minutos", "ocurre con frecuencia Y"). Si no hay dato disponible, documentarlo como supuesto.
2. **Solución propuesta** — una línea.
3. **Diagrama de contexto** — diagrama simple (puede ser texto) que muestra: el sistema, sus usuarios y sus dependencias externas (servicios de email, bases de datos, sistemas de terceros). Contextualiza el sistema para cualquier lector que llegue nuevo.
4. **Stakeholders** — todos los afectados, no solo los usuarios directos. *(Los stakeholders incluyen a los usuarios directos; la tabla de Usuarios desglosa solo quienes interactúan con el sistema.)* Técnica: por cada objetivo del proyecto, preguntar "¿quién se beneficia?" y "¿quién se ve afectado negativamente?". Documentar en una tabla: stakeholder / relación con el proyecto / interés principal.
5. **Usuarios** — tabla: rol / objetivo principal. Solo los que interactúan directamente con el sistema.
6. **Objetivos del proyecto** — medibles.
7. **Alcance** — dentro / fuera, explícito. Si hay ambigüedad, resolverla aquí o registrarla como decisión pendiente.
8. **Restricciones y supuestos** — revisar que cubren las categorías relevantes: tiempo, presupuesto, técnica, organizativa, regulatoria (para restricciones); usuarios, datos, infraestructura, proceso (para supuestos). Si alguna categoría está vacía, justificar por qué no aplica.
9. **Consideración de privacidad** — si el sistema maneja datos personales (nombres, emails, identificadores): ¿qué datos se almacenan? ¿durante cuánto tiempo? ¿quién tiene acceso? ¿qué ocurre con ellos al finalizar o resetear el sistema?
10. **Criterios de éxito** — deben ser SMART: específicos, medibles y con plazo. "Los usuarios podrán usar la aplicación" no es un criterio de éxito válido. **Estos criterios se retoman literalmente al cierre del proyecto (ver "README de portfolio") para confrontarlos contra el resultado real — no se archivan una vez escritos.**
11. **Horizonte temporal** — aunque sea aproximado.
12. **Decisiones pendientes** — lista viva. Toda decisión pendiente debe resolverse antes de que el documento al que afecta llegue a baseline.

---

### 02 — User Stories

**Formato:** Como [rol], quiero [acción], para [objetivo].
**Criterios de aceptación:** Dado / Cuando / Entonces.

**Organización:** agrupar por dominio funcional (acceso, gestión de usuarios, proceso principal, administración). No por orden de implementación.

**Definition of Done** (definir al inicio del documento y no cambiarla):
- Formato correcto (Como / quiero / para).
- Al menos un criterio de aceptación del camino principal y uno del camino alternativo o de error.
- Revisión INVEST (02b, o inline en perfil Ligero) superada sin ningún criterio en 🔴.
- Todas las dependencias documentadas explícitamente.

Una historia no se considera cerrada hasta que la revisión INVEST la haya superado.

**Estándares de calidad:**
- IDs secuenciales y limpios. Si una historia se elimina durante la revisión, dejar una línea de nota: `~~US-08~~ — eliminada; contenido absorbido por US-07a`.
- No fusionar dos historias para evitar una dependencia. La dependencia se documenta, no se oculta.
- Historias de sistema ("Como sistema...") no son válidas: son comportamientos del sistema; van como criterios de aceptación de otra historia o como reglas de negocio en el BRD.
- Restricciones no funcionales específicas de una historia (tiempo de respuesta, límite de tamaño) van en sus criterios de aceptación. Las restricciones de sistema van en el BRD.

---

### 02b — Valoración INVEST

En perfil Ligero, esta valoración va al final del mismo documento de User Stories en vez de en un archivo separado; los criterios y umbrales son los mismos.

**Escala:** 🟢 cumple · 🟡 cumple con matices · 🔴 no cumple

**Umbrales:**
- Cualquier criterio 🔴 bloquea el baseline. La historia requiere corrección antes de avanzar.
- Tres o más criterios 🟡 en una historia indican un problema de diseño, aunque ninguno sea 🔴.
- I🟡 por "no tiene valor sin otra historia" no es aceptable sin justificación explícita: o se fusiona con razón documentada o se separa y se documenta la dependencia.
- Para los 🟡 que se aceptan: documentar el motivo y la mitigación. No aceptar un 🟡 en silencio.

**Señal de alarma:** si la revisión completa no produce ningún 🟡 ni 🔴, la revisión no ha sido suficientemente crítica. Releer cada historia con la pregunta: "¿qué podría salir mal al implementar esto?". Si tras la segunda lectura todos los criterios siguen en 🟢, Claude debe argumentar explícitamente por qué cada criterio está en verde antes de cerrar la revisión. Una revisión sin ningún 🟡 requiere justificación, no simplemente el resultado.

---

### 03 — BRD / PRD

En perfil Ligero, este documento se fusiona con 04 y 05 en `03_requisitos.md` (ver "Perfiles de proyecto"); la estructura de contenido es la misma, añadiendo la tabla de requisitos con columna MoSCoW y método de verificación.

**Estructura mínima:**
1. Contexto y referencia
2. Visión general del proceso *(opcional — dos párrafos en prosa; sin pasos enumerados; orienta al lector de negocio; no es la fuente de autoridad sobre flujos)*
3. Actores y permisos (una matriz por dominio funcional, no una mega-matriz)
4. Reglas de negocio (RN-xx)
5. Requisitos funcionales en formato EARS (RF-xx)
6. Requisitos no funcionales (RNF-xx)
7. Casos límite y excepciones

**Estándares de calidad:**
- IDs de reglas de negocio y requisitos secuenciales. Si se elimina uno: `~~RN-05~~ *(eliminada — ver nota en RN-12)*`.
- Las reglas de negocio que son el resultado de una decisión deben referenciar `decisions.md` con la fecha de la entrada correspondiente. Si la razón de una regla no es obvia, la referencia a `decisions.md` es obligatoria.
- El BRD no tiene sección de flujos detallados. La "Visión general del proceso" es prosa contextual, nunca pasos numerados con decisiones.
- Ninguna columna vacía con "—". Si una columna no aporta valor, se elimina del documento.
- Antes de baselinar: verificar que todos los supuestos del Project Brief siguen siendo válidos. Si alguno cambió durante el análisis: actualizarlo en el Project Brief y registrar el cambio en `decisions.md`.

**Formato EARS** para requisitos funcionales:

| Patrón | Estructura |
|--------|------------|
| Ubiquo | *"El sistema debe..."* |
| Dirigido por evento | *"Cuando X, el sistema debe..."* |
| Dependiente de estado | *"Mientras X, el sistema debe..."* |
| Opcional | *"Donde X, el sistema debe..."* |
| No deseado | *"Si X, entonces el sistema debe..."* |

---

### 04 — Matriz de requisitos (RTM)

En perfil Ligero, esta matriz vive dentro de `03_requisitos.md` (ver "Perfiles de proyecto").

Este documento es una **Matriz de Trazabilidad de Requisitos** real: permite seguir un requisito desde su origen (US) hasta su verificación (caso de prueba).

**Columnas obligatorias:**

| ID | Descripción | Categoría | US de origen | RN relacionada | Prioridad MoSCoW | Método de verificación | Caso de prueba |
|----|-------------|-----------|--------------|----------------|------------------|------------------------|----------------|

**Reglas:**
- **Método de verificación:** obligatorio para todos los requisitos, tanto RF como RNF. Para RF: referencia al test o escenario ("E2E: test_grupo_crear"). Para RNF: herramienta y criterio cuantitativo ("axe DevTools — 0 violaciones nivel AA").
- Los RF sin US de origen (comportamientos del sistema detectados durante el análisis) se marcan con "—" en esa columna. Al pie del documento, añadir una nota explicando que estos requisitos son derivados del análisis, no de una historia de usuario.
- Si un requisito cambia durante la implementación, actualizar la fila completa; no dejar datos obsoletos.

---

### 05 — MoSCoW

En perfil Ligero, esta distribución vive como columna dentro de `03_requisitos.md` (ver "Perfiles de proyecto"); las señales de alarma siguen aplicando igual.

El MoSCoW **no es un documento que repite todos los requisitos**. Es un documento de una página con:
1. Tabla de distribución (Must / Should / Could / Won't por categoría).
2. Defensa de las decisiones de priorización más importantes: por qué X es Must y no Should, por qué Y es Won't.
3. Lista de ítems Should/Could con su alternativa manual (qué haría el usuario si esta función no existiera).
4. Lista de ítems Won't Have con nota de si podría reconsiderarse en una versión futura.

La prioridad de cada requisito vive como **columna en la RTM (04)**. El documento MoSCoW referencia la RTM.

**Señales de alarma:**
- Si más del 80% de los requisitos son Must Have: el alcance no está ajustado al MVP. Partir del flujo mínimo que resuelve el problema; clasificar todo lo que esté fuera como Should/Could; argumentar de vuelta a Must solo lo que no tiene alternativa viable.
- Si no hay ningún Won't Have: el analista no ha demostrado la capacidad de decir que no. Won't Have no es "lo que no dio tiempo" — es "lo que se evaluó y se descartó explícitamente para esta versión". Toda especificación profesional tiene al menos dos o tres.

**Cierre del loop tras el MoSCoW:**
- Antes de avanzar al modelo de datos y al mapa de flujos: verificar que ningún artefacto downstream incluye ítems Won't o Could.
- El modelo de datos solo modela entidades y atributos de Must + Should.
- El mapa de flujos no incluye flujos de ítems Won't o Could.

---

### 06 — Modelo de datos conceptual

En perfil Ligero, se omite como archivo si el dominio tiene una sola entidad simple (ver "Perfiles de proyecto").

**Estándares de calidad:**
- Solo se modelan entidades y atributos de requisitos Must + Should. Los ítems Won't/Could no aparecen en el modelo.
- Excepción: entidades de infraestructura transversal (autenticación, configuración global) que son necesarias independientemente del nivel de prioridad de las features. Etiquetarlas como "infraestructura" en la tabla resumen.
- Diagrama de transiciones de estado para cada entidad que tenga un ciclo de vida propio.
- Tabla de relaciones con cardinalidad y nota sobre las restricciones de negocio relevantes.
- Tabla resumen de entidades con prioridad MoSCoW.
- Los atributos deben incluir sus restricciones de validación cuando no sean obvias (único, no nulo, rango, formato).

---

### 07 — Mapa de flujos

En perfil Ligero, se omite como archivo si todos los flujos son lineales (ver "Perfiles de proyecto"); en ese caso los pasos van como criterios de aceptación de las user stories.

Este documento es la **única fuente de autoridad sobre flujos**. Si el BRD tiene una sección de flujos, este documento la supera.

**Estructura por flujo:**

```
## Flujo N — [Nombre]

**Actores:** [lista]
**Disparador:** [qué acción o evento inicia este flujo]
**Precondición:** [estado del sistema antes de que empiece]
**Postcondición:** [estado del sistema cuando termina con éxito]
```

**Leyenda de anotaciones:**
- `[D]` — punto de decisión con ramas → Sí / → No
- `[E]` — camino de error o bloqueo: el flujo **no alcanza la postcondición** → termina en `FIN ✗`
- `[A]` — flujo alternativo: alcanza la **misma postcondición** por un camino diferente → termina en `FIN ✓`
- `[S]` — paso de funcionalidad Should
- `FIN ✓` — postcondición alcanzada
- `FIN ✗` — flujo terminado sin completarse

La distinción entre [E] y [A] importa en el diseño de la interfaz: un [E] necesita un mensaje de error; un [A] necesita una ruta de navegación alternativa.

**Reglas:**
- Referencias a RF y RN en cada paso relevante.
- Los ítems Could/Won't no aparecen en los flujos.
- Cuando un flujo desencadena otro, referenciarlo explícitamente: `*(Ver Flujo N.)*`.

---

### 08 — Especificación de interfaz

En perfil Ligero, se omite si el proyecto no tiene UI o tiene una sola vista trivial (ver "Perfiles de proyecto").

**Estructura:**
- Mapa de navegación al inicio.
- Por cada vista: elementos (tabla), estados, acciones.
- Cada vista referencia los RF que satisface.

**Estados que deben cubrirse sistemáticamente en cada vista:**

| Estado | Descripción |
|--------|-------------|
| Estado vacío | Qué ve el usuario cuando la lista o sección no tiene datos |
| Estado de carga | Indicador visual mientras se espera una respuesta asíncrona |
| Estado de error | Qué ocurre cuando la operación falla |
| Estado normal | Vista con datos |

**Si existe RNF de interfaz responsive:** para cada vista, indicar qué elementos se adaptan, colapsan o reorganizan en pantalla pequeña.

**Acciones destructivas o irreversibles:** cualquier acción que no se puede deshacer (eliminar, resetear, confirmar) debe tener especificado el diálogo de confirmación: qué dice, qué opciones ofrece.

---

### 09 — Plan de verificación

El documento que cierra el ciclo entre requisitos y pruebas.

**Estructura:**

| ID requisito | Tipo RF/RNF | Descripción | Tipo de prueba | Herramienta / Entorno | Criterio de éxito | Resultado |
|---|---|---|---|---|---|---|

**Tipos de prueba:**
- **Inspección** — revisión manual del código, documento o configuración.
- **Demostración** — ejecutar el caso en el sistema y mostrar el resultado esperado.
- **Prueba** — test automatizado (unitario, integración o E2E).
- **Análisis** — evaluación con herramienta especializada (axe, Lighthouse, W3C Validator).

**Reglas:**
- Cubre todos los Must Have (RF y RNF). Los Should Have aparecen con una nota de que son opcionales en esta versión.
- Para RNF: criterio cuantitativo obligatorio. No "cumple WCAG" sino "0 violaciones nivel AA en axe DevTools en las vistas V-01 a V-10".
- La columna "Resultado" se rellena durante la verificación, no antes. Un resultado en blanco es un requisito no verificado; documentar el motivo si se omite intencionalmente.

---

## Revisión spec-vs-implementación

Al cerrar el código, antes de dar el proyecto por terminado:

1. Recopilar todos los gaps registrados en `decisions.md` como "Gap detectado" durante el desarrollo.
2. Recorrer todos los Must Have de la RTM y verificar que cada uno está implementado y verificado.
3. Para los gaps nuevos detectados en este paso: clasificar, documentar en `decisions.md` y actualizar el plan de verificación.

---

## README de portfolio

Último paso del proyecto. Una página que orienta al evaluador externo:

- Qué problema resuelve el proyecto y por qué vale la pena leer la documentación.
- Lista de documentos en orden de lectura recomendado con una línea de descripción cada uno.
- Dos o tres decisiones clave que demuestran el razonamiento analítico detrás del proyecto.
- Estado del proyecto: qué se entregó, qué quedó diferido y por qué.
- **Resultados frente a los criterios de éxito del Brief.** Tabla obligatoria que retoma literalmente cada criterio SMART definido en `01_brief_proyecto.md` y lo confronta con el resultado real:

  | Criterio de éxito (Brief) | Resultado obtenido | ¿Cumplido? |
  |---|---|---|

  Si un criterio no se cumplió, explicar por qué y qué se necesitaría para cumplirlo — no se omite la fila ni se reformula el criterio a posteriori para que "encaje" con el resultado.

---

## GitHub y portfolio público

### Commits como artefacto de portfolio

El historial de commits es visible en GitHub y debe contar la historia del proyecto: primero la documentación por fases, luego la implementación. Un evaluador que mira el historial debe poder ver que el proceso SDD se siguió de verdad.

**Convención de mensajes de commit:**

```
docs: [fase] — [descripción breve]     ←── commits de documentación
feat: [descripción breve]              ←── funcionalidad nueva
fix: [descripción breve]               ←── corrección de error
test: [descripción breve]              ←── tests
chore: [descripción breve]             ←── tareas de mantenimiento (deps, config)
```

Ejemplos:
- `docs: project brief — baseline`
- `docs: user stories — revisión INVEST completada`
- `docs: brd/prd — baseline`
- `feat: autenticación — login y primer acceso`

**Reglas:**
- Un commit por fase de documentación al llegar a baseline, no uno por cada edición.
- Los mensajes describen el resultado ("baseline", "revisión completada"), no la acción ("edito", "añado", "cambio").
- No mezclar commits de documentación con commits de código en el mismo push.

### README.md como landing page de GitHub

El README.md es lo primero que ve cualquier persona que llega al repositorio — se renderiza directamente en la página principal de GitHub. Debe estar pensado para ese formato, no como un documento de texto plano.

**Estructura recomendada para el README final:**

```markdown
# [Nombre del proyecto]

[Una línea que describe qué hace y para quién.]

## El problema

[2-3 frases: qué ocurría antes, por qué era un problema, qué lo resuelve.]

## Documentación

| Documento | Descripción |
|-----------|-------------|
| [Project Brief](docs/01_brief_proyecto.md) | Problema, alcance y objetivos |
| [User Stories](docs/02_historias_usuario.md) | Historias de usuario con criterios de aceptación |
| ... | ... |

## Resultados frente a los criterios de éxito

[Tabla — ver sección "README de portfolio" arriba.]

## Stack tecnológico *(si aplica)*

[Lista de tecnologías principales.]

## Estado del proyecto

[Qué está entregado, qué quedó diferido y por qué.]
```

**Reglas:**
- Si el proyecto tiene interfaz visual, incluir al menos una captura de pantalla (`![descripción](ruta/imagen.png)`).
- Si el proyecto está desplegado, incluir el enlace.
- Los enlaces a documentos deben usar rutas relativas para que funcionen tanto en GitHub como en local.
- El README provisional (durante el desarrollo) puede ser solo el título y una línea. No dejarlo vacío: GitHub muestra el README vacío como señal de abandono.

### Licencia

Un repositorio público sin `LICENSE` es técnicamente "todos los derechos reservados", lo que impide que otros usen o referencien el trabajo legalmente.

- Para proyectos donde el código es el entregable principal: **MIT**.
- Para proyectos donde la documentación es el entregable principal: **CC BY 4.0** (Creative Commons Atribución).
- Para proyectos mixtos: MIT para el código, CC BY 4.0 para la documentación — especificarlo en el propio LICENSE o en el README.

---

## Log de decisiones (`decisions.md`)

Captura **todas** las decisiones relevantes del proyecto, no solo las de análisis:

- El perfil de proyecto elegido (Completo/Ligero) y su razón — primera entrada del proyecto.
- Decisiones de análisis: alcance, roles, reglas de negocio, mecanismos.
- Decisiones de diseño con consecuencias funcionales: arquitectura de autenticación, modelo de sesión, integraciones, stack tecnológico.
- Gaps detectados durante la implementación o en la revisión final. El título de estas entradas debe comenzar con **"Gap detectado:"** para identificarlos fácilmente en el log.

Los gaps se documentan **cuando se detectan**, no solo al final del proyecto.

**Formato de entrada:**

```
### [FECHA] — [TÍTULO BREVE]

- **Estado:** activa
- **Decisión:** qué se decidió
- **Contexto:** por qué era necesario decidir esto
- **Alternativas descartadas:** qué se consideró y por qué no se eligió
- **Consecuencias:** qué implica hacia adelante
```

**Reversión de una decisión:** crear una entrada nueva que referencia la original y marcar la original como supersedida:

```
### [FECHA ORIGINAL] — [TÍTULO]
- **Estado:** ~~activa~~ supersedida — ver [FECHA NUEVA]
...

### [FECHA NUEVA] — Revisión de [TÍTULO ORIGINAL]
- **Estado:** activa
- **Decisión:** la decisión de [FECHA ORIGINAL] queda revertida. [Nueva decisión].
- **Contexto:** [qué cambió para que la decisión anterior dejara de ser válida]
...
```

---

## Backlog (`BACKLOG.md`)

Captura toda idea, duda sin resolver, propuesta de funcionalidad o corrección futura que el usuario mencione en conversación y que no se vaya a abordar en el momento — no debe quedar solo en el historial de la conversación.

- Se registra en el mismo turno en que se menciona, con el formato ya establecido en `BACKLOG.md` (Origen / Descripción / Relacionado con / Preguntas abiertas).
- No hace falta que el usuario diga explícitamente "esto va al backlog": si algo se plantea como idea futura, duda que no se resuelve ahora, o posible corrección que se pospone, se anota igual.
- Si la duda se resuelve en la misma conversación (aunque se haya mencionado como posible ítem de backlog), se documenta igualmente como resuelta — ver ejemplos ya existentes en `BACKLOG.md` — en vez de omitirse.
- Un ítem de backlog nunca entra directamente en una fase formal (User Stories, BRD, RTM...) sin pasar antes por una decisión explícita del usuario, según indica la cabecera de `BACKLOG.md`.

---

## Seguridad

### Credenciales y secretos

- Nunca incrustar credenciales, tokens ni secretos en URLs, código o archivos de configuración versionados.
- Cualquier operación que implique autenticación o credenciales debe ejecutarla el usuario directamente en su terminal.
- Antes de ejecutar cualquier comando con implicaciones de seguridad: explicar qué va a hacer y esperar confirmación explícita.

### Variables de entorno

- Todas las variables de entorno que el proyecto necesita están documentadas en `.env.example` con valores de ejemplo o descripciones (nunca valores reales).
- `.env.example` sí se versiona. `.env`, `.env.local` y cualquier archivo con valores reales nunca se versionan.

### Repositorio público

Si el repositorio es público (caso habitual en un portfolio), todo lo que se sube es visible permanentemente. Implicaciones adicionales:

- Los datos de seed y de ejemplo deben ser **completamente ficticios**: nombres inventados, emails del tipo `alumno1@ejemplo.com`, sin datos reales de personas aunque sean de prueba.
- Los documentos de análisis no deben contener datos reales de usuarios, instituciones o sistemas de terceros aunque estén anonimizados parcialmente.
- Antes de cada push, revisar mentalmente si hay algo en los archivos nuevos o modificados que no debería ser público.

### Antes del primer commit

Verificar que `.gitignore` excluye:
- `.env`, `.env.local`, `.env.*.local`
- `*.pem`, `*.key`, `*.p12`
- Directorios de credenciales o secretos del entorno de desarrollo

Si el proyecto añade nuevas dependencias: revisar vulnerabilidades conocidas antes de hacer commit (`npm audit` o equivalente).

---

## Normas de trabajo

- El usuario piensa y decide. Claude orienta, propone opciones y advierte consecuencias, pero no decide.
- Ante cualquier decisión que afecte al alcance, a los requisitos, a la arquitectura o a la experiencia de usuario: presentar opciones con pros/contras y esperar que el usuario elija.
- Claude redacta propuestas completas; el usuario revisa, corrige y aprueba.
- Antes de actualizar `decisions.md`, el usuario debe haber argumentado la decisión en la conversación.
- Si el usuario quiere saltarse una fase: señalarlo y redirigir.
- Si el usuario quiere marcar un documento como baseline sin haber pasado por revisión: señalarlo.
- Si algo en la spec no tiene sentido o parece incorrecto: decirlo con argumentos antes de ejecutar.

---

## Revisión de User Stories

Flujo: redacción → valoración INVEST → corrección → baseline.

Cuando el usuario presente las user stories, cargar `docs/02b_historias_usuario_invest.md` (o la sección inline equivalente en perfil Ligero) y rellenar la valoración antes de cualquier otro paso.

---

## Estructura de documentación (perfil Completo)

```
[nombre-proyecto]/
├── CLAUDE.md
├── README.md               ←── texto provisional hasta el cierre; se completa como README de portfolio
├── decisions.md
└── docs/
    ├── 00_glosario.md      ←── vivo desde el inicio
    ├── 01_brief_proyecto.md ←── incluye diagrama de contexto
    ├── 02_historias_usuario.md
    ├── 02b_historias_usuario_invest.md
    ├── 03_brd_prd.md
    ├── 04_matriz_requisitos.md
    ├── 05_moscow.md
    ├── 06_modelo_datos.md
    ├── 07_mapa_flujos.md
    ├── 08_especificacion_interfaz.md
    └── 09_plan_verificacion.md
```

## Estructura de documentación (perfil Ligero)

```
[nombre-proyecto]/
├── CLAUDE.md
├── README.md
├── decisions.md
└── docs/
    ├── 00_glosario.md
    ├── 01_brief_proyecto.md
    ├── 02_historias_usuario.md      ←── incluye valoración INVEST inline
    ├── 03_requisitos.md             ←── BRD/PRD + RTM + MoSCoW fusionados
    ├── 06_modelo_datos.md           ←── solo si aplica (ver "Perfiles de proyecto")
    ├── 07_mapa_flujos.md            ←── solo si aplica
    ├── 08_especificacion_interfaz.md ←── solo si aplica
    └── 09_plan_verificacion.md
```
