# Log de decisiones — household_os

Captura todas las decisiones relevantes del proyecto: análisis, diseño con consecuencias funcionales, y gaps detectados durante la implementación.

Formato de entrada:

```
### [FECHA] — [TÍTULO BREVE]

- **Estado:** activa
- **Decisión:** qué se decidió
- **Contexto:** por qué era necesario decidir esto
- **Alternativas descartadas:** qué se consideró y por qué no se eligió
- **Consecuencias:** qué implica hacia adelante
```

---

### 2026-09-20 — Perfil de proyecto: Completo

- **Estado:** activa
- **Decisión:** household_os sigue el perfil Completo de la plantilla SDD (`CLAUDE.md`), con las diez fases sin recortes.
- **Contexto:** al arrancar el proyecto había que elegir entre perfil Completo y Ligero (plantilla v2.0). household_os es una herramienta de organización familiar (calendarios, eventos y tareas compartidos) con varios roles de hogar, entidades con ciclo de vida propio (tareas, eventos recurrentes) y flujos con puntos de decisión (asignación, notificaciones, qué pasa si una tarea no se completa) — encaja con el criterio de perfil Completo, no con el de una utilidad acotada.
- **Alternativas descartadas:** perfil Ligero — se descartó porque habría forzado fusionar documentos (BRD+RTM+MoSCoW) y omitir modelo de datos/flujos/interfaz en un proyecto que, por su alcance real, sí los necesita.
- **Consecuencias:** se documentan las diez fases (00 a 09) por separado, según la secuencia estándar del perfil Completo.

---
