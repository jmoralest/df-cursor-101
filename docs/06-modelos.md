# Modelos e integración

## Modelos en Cursor

Cursor ofrece varios modelos (Composer, Claude, GPT, etc.). Puedes elegir modelo por chat o en Settings.

| Uso típico | Orientación |
|------------|-------------|
| Implementación rápida en repo | Composer (optimizado para código en IDE) |
| Razonamiento largo / arquitectura | Modelos "thinking" o Claude Opus |
| Tareas simples | Modelos más rápidos/económicos |

## Cambiar modelo

- Selector en la barra del chat del agente
- `Cursor Settings → Models`
- En automatizaciones: campo `model` en API/SDK

## Cursor SDK (agentes programáticos)

Para ejecutar agentes **fuera del IDE** (CI, scripts, bots):

- **TypeScript**: `@cursor/sdk` — [docs](https://cursor.com/docs/sdk/typescript)
- **Python**: `cursor-sdk` — [docs](https://cursor.com/docs/sdk/python)

```typescript
import { Agent } from "@cursor/sdk";

const result = await Agent.prompt("Explica este repo", {
  apiKey: process.env.CURSOR_API_KEY!,
  model: { id: "composer-2.5" },
  local: { cwd: process.cwd() },
});
```

Patrones: `Agent.prompt()` (one-shot), `Agent.create()` + `agent.send()` (multi-turn), runtime local vs cloud.

## Integración con otros entornos

| Entorno | Archivo de contexto | Skills |
|---------|---------------------|--------|
| **Cursor** | `.cursor/rules/`, `AGENTS.md` | `.cursor/skills/`, `.claude/skills/` |
| **Claude Code** | `CLAUDE.md`, `AGENTS.md` | `.claude/skills/` |
| **Codex** | — | `.codex/skills/` |
| **Estándar abierto** | — | `.agents/skills/` |

## Subagentes (Task tool)

El agente principal puede lanzar subagentes especializados (`explore`, `shell`, `bugbot`, etc.) para tareas paralelas o aisladas.

## No confundir

- **Modelo** = qué LLM responde
- **Skill** = instrucciones cargadas en el contexto del agente
- **MCP** = herramientas externas
- **Rule** = convenciones persistentes

Ver [07-claude-skills-en-cursor.md](07-claude-skills-en-cursor.md) para reutilizar skills entre Claude y Cursor.
