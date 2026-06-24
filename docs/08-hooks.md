# Hooks

Los **hooks** ejecutan lógica **determinista** en eventos del agente. El modelo no puede "olvidar" ejecutarlos (a diferencia de skills).

## Ubicación

- Proyecto: `.cursor/hooks.json` + `.cursor/hooks/*`
- Usuario: `~/.cursor/hooks.json` + `~/.cursor/hooks/*`

## Ejemplo mínimo

`.cursor/hooks.json`:

```json
{
  "version": 1,
  "hooks": {
    "afterFileEdit": [
      {
        "command": ".cursor/hooks/format.sh"
      }
    ]
  }
}
```

## Eventos comunes

| Evento | Uso |
|--------|-----|
| `beforeShellExecution` | Bloquear o aprobar comandos |
| `afterFileEdit` | Formatear tras editar |
| `preToolUse` | Interceptar herramientas |
| `beforeSubmitPrompt` | Validar prompts (secretos, política) |
| `subagentStop` | Encadenar subagentes |

## Hooks vs Skills vs Rules

| | Determinista | Usa tokens del modelo |
|--|--------------|----------------------|
| Hook | ✅ | ❌ |
| Skill | ❌ | ✅ |
| Rule | ❌ (sugerencia) | ✅ |

**Regla práctica**: lint/format/guardas → hook; flujo complejo opcional → skill; estilo → rule.

## Referencia

- [Cursor Hooks Docs](https://cursor.com/docs/agent/hooks)
- Skill built-in: `create-hook` en Cursor
