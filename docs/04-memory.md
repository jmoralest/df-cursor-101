# Memory (Memoria)

## El problema que resuelve

Los modelos no retienen contexto entre sesiones. **Memory** y otras capas de configuración persisten preferencias sin repetirlas en cada chat.

## Capas de persistencia

```
┌──────────────────────────────────────────────────┐
│ User Rules          │ Settings → Rules (global)   │
├─────────────────────┼────────────────────────────┤
│ Project Rules       │ .cursor/rules/*.mdc         │
├─────────────────────┼────────────────────────────┤
│ AGENTS.md           │ Raíz / subdirectorios       │
├─────────────────────┼────────────────────────────┤
│ Memories            │ Generadas + aprobadas por ti│
├─────────────────────┼────────────────────────────┤
│ Skills (repo)       │ .cursor/skills/ en git      │
└─────────────────────┴────────────────────────────┘
```

## Memories automáticas

Cursor puede **proponer** guardar hechos aprendidos:

- "Este proyecto usa pytest, no unittest"
- "El usuario prefiere respuestas en español"
- "Siempre usar `gh` para operaciones de GitHub"

**Tú apruebas o rechazas** cada memory. Revisa en Settings → Rules la sección de memories.

## Cuándo usar cada mecanismo

| Necesidad | Mecanismo recomendado |
|-----------|----------------------|
| Estándar de equipo obligatorio | Project Rule en git |
| Preferencia personal en todos los proyectos | User Rule |
| Flujo ocasional (deploy, PR) | Skill |
| Descubrimiento incremental del agente | Memory (con moderación) |
| Documentación para humanos y agentes | AGENTS.md / README |

## MEMORY.md en este repo

El archivo `MEMORY.md` en la raíz es parte del **harness**: referencia rápida para agentes, no es la memoria automática de Cursor.

## Anti-patrones

- Duplicar la misma instrucción en User Rule + Project Rule + Memory
- Memories contradictorias con reglas de equipo
- Confiar en memory para secretos o credenciales (usar `.env` y `.gitignore`)

## Siguiente

- [02-reglas.md](02-reglas.md) — reglas explícitas
- [09-tips-trucos.md](09-tips-trucos.md) — gestión del contexto
