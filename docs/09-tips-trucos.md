# Tips y trucos

## Contexto

- **@archivo / @carpeta** — incluye código sin buscar a ciegas
- **@docs** — documentación indexada del proyecto
- Mantén reglas **cortas**; el resto en skills
- Una tarea grande → dividir en pasos o usar Plan mode

## Productividad

| Atajo / acción | Efecto |
|----------------|--------|
| `Cmd+I` / `Ctrl+I` | Chat del agente |
| `Tab` | Autocompletado inline |
| `/` en agente | Invocar skills/comandos |
| `@` | Mencionar archivos, docs, skills |
| `Customize` sidebar | Ver rules y skills activos |

## Calidad de respuestas

1. Sé específico: archivo, error exacto, resultado esperado
2. Pide "lee X antes de editar" para harness (`AGENTS.md`)
3. Pide `git diff` o tests tras cambios
4. Usa Ask mode para explorar sin edits accidentales

## Equipo

- Reglas y skills en **git**
- `AGENTS.md` como mapa de divulgación progresiva
- Team Rules (plan Enterprise) para políticas globales
- No commitear `.env` ni API keys en `mcp.json`

## Errores frecuentes

| Problema | Solución |
|----------|----------|
| El agente ignora convenciones | `alwaysApply: true` o globs más específicos |
| Skill no aparece | Revisar `name`, `description`, ruta `SKILL.md` |
| Demasiado contexto | Migrar reglas largas a skills |
| Skill Claude no en Cursor | Verificar ruta `~/.claude/skills/` o copiar a `.cursor/skills/` |
| Comando peligroso | Hook `beforeShellExecution` |

## Harness engineering

Inspirado en repos como [ejemplo-harness-subagentes](https://github.com/jmoralest/ejemplo-harness-subagentes):

```
AGENTS.md          → mapa para el agente
feature_list.json  → alcance incremental (en proyectos de app)
progress/          → trazabilidad en disco
.cursor/rules/     → convenciones
.cursor/skills/    → flujos repetibles
```

## Recursos oficiales

- [Cursor Learn — Customizing Agents](https://cursor.com/learn/customizing-agents)
- [Agent Best Practices](https://cursor.com/blog/agent-best-practices)
- [Rules Docs](https://cursor.com/docs/rules)
- [Skills Docs](https://cursor.com/docs/context/skills)
