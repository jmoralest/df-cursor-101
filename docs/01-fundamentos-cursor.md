# Fundamentos de Cursor

## ¿Qué es Cursor?

Cursor es un IDE basado en VS Code con un **agente de IA** integrado que puede leer tu código, ejecutar comandos, editar archivos y conectarse a herramientas externas (MCP).

## Modos de interacción

| Modo | Uso | Cuándo elegirlo |
|------|-----|-----------------|
| **Agent** | Edita archivos, ejecuta terminal, usa MCP | Implementar, refactorizar, depurar |
| **Ask** | Solo lectura | Explorar código, preguntas, diseño |
| **Plan** | Planificación colaborativa | Tareas grandes con decisiones de arquitectura |
| **Tab** | Autocompletado inline | Escribir código línea a línea |

## Contexto que recibe el agente

El agente no "recuerda" entre sesiones por sí solo. El contexto viene de:

1. Tu mensaje actual y el historial del chat
2. Archivos abiertos o @-mencionados
3. **Rules** (siempre o por glob/descripción)
4. **Skills** (cuando son relevantes o invocados con `/`)
5. **MCP** (herramientas bajo demanda)
6. **Memories** y User Rules (preferencias persistentes)

## Jerarquía de instrucciones

Cuando hay conflicto, en general:

**Team Rules → Project Rules → User Rules → mensaje del chat**

Las reglas de equipo (planes Team/Enterprise) tienen prioridad sobre las del proyecto.

## Conceptos clave

```
┌─────────────────────────────────────────────────────────┐
│                    Tu conversación                       │
├─────────────┬──────────────┬────────────┬───────────────┤
│   Rules     │    Skills    │    MCP     │   Memories    │
│ (estático,  │ (dinámico,   │ (externo,  │ (aprendido,   │
│  siempre o  │  bajo        │  bajo      │  entre        │
│  por archivo)│  demanda)   │  demanda)  │  sesiones)    │
└─────────────┴──────────────┴────────────┴───────────────┘
```

## Desarrollo fundamental con agentes

1. **Contexto explícito**: @archivos, @carpetas, @docs
2. **Tareas acotadas**: un objetivo claro por conversación
3. **Verificación**: pide tests, `git diff`, o revisión manual
4. **Plan antes de código grande**: modo Plan o "diseña primero"
5. **Harness en repo**: `AGENTS.md`, reglas y skills versionados

## Siguiente lectura

- [02-reglas.md](02-reglas.md)
- [03-skills.md](03-skills.md)
- [09-tips-trucos.md](09-tips-trucos.md)
