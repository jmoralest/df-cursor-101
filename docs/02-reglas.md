# Reglas (Rules)

Las **reglas** son instrucciones persistentes que moldean el comportamiento del agente en cada conversación (o cuando coinciden archivos/patrones).

## Ubicación

```
.cursor/rules/
├── 00-cursor-basics.mdc    # alwaysApply: true
└── markdown-docs.mdc       # globs: docs/**/*.md
```

También existe **`AGENTS.md`** en la raíz (o subcarpetas): markdown plano, portable entre herramientas (Cursor, Claude Code, etc.).

## Formato `.mdc`

```markdown
---
description: Qué hace esta regla (aparece en el selector)
globs: **/*.ts          # opcional: archivos que activan la regla
alwaysApply: false      # true = siempre en contexto
---

# Título

Contenido de la regla...
```

## Cuatro modos de activación

| Configuración | Comportamiento |
|---------------|----------------|
| `alwaysApply: true` | Siempre incluida |
| `globs: **/*.py` | Se adjunta al trabajar con esos archivos |
| Solo `description` | El agente la incluye cuando la considera relevante |
| Invocación manual | `@nombre-regla` en el chat |

## Rules vs AGENTS.md

| | `.cursor/rules/*.mdc` | `AGENTS.md` |
|--|----------------------|-------------|
| Frontmatter YAML | Sí | No |
| Globs por extensión | Sí | No (alcance por carpeta) |
| Portable a otras IDEs | Parcial | Sí |
| Control fino de activación | Sí | Limitado |

## Buenas prácticas

- **&lt; 50 líneas** por regla; una preocupación por archivo
- Ejemplos concretos ✅/❌
- No duplicar lo que ya está en skills (flujos largos → skill)
- Versionar en git para el equipo

## Crear una regla paso a paso

1. `Cursor Settings → Rules → New Rule` o crea `.cursor/rules/mi-regla.mdc`
2. Define `description` clara
3. Elige: `alwaysApply`, `globs`, o solo descripción inteligente
4. Escribe instrucciones accionables
5. Prueba abriendo un archivo que coincida con el glob

## Migración a Skills

Las reglas "Apply Intelligently" (solo `description`, sin globs ni alwaysApply) pueden migrarse a skills con `/migrate-to-skills`.

Ver [03-skills.md](03-skills.md) y [07-claude-skills-en-cursor.md](07-claude-skills-en-cursor.md).
