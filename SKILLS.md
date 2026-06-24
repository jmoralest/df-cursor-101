# Skills — Índice de este proyecto

## Skills incluidos en el repo

| Skill | Ruta | Cuándo usarlo |
|-------|------|---------------|
| `crear-readme` | `.cursor/skills/crear-readme/SKILL.md` | Generar o actualizar README siguiendo la plantilla del proyecto |

## Dónde Cursor busca skills

| Ubicación | Alcance |
|-----------|---------|
| `.cursor/skills/` | Proyecto (versionable en git) |
| `.agents/skills/` | Proyecto (estándar abierto) |
| `~/.cursor/skills/` | Usuario (todos los proyectos) |
| `~/.agents/skills/` | Usuario (estándar abierto) |
| `.claude/skills/` | Proyecto (**compatibilidad Claude**) |
| `~/.claude/skills/` | Usuario (**compatibilidad Claude**) |

Cursor recorre subdirectorios y detecta cualquier `SKILL.md`.

## Skills built-in de Cursor

Los skills en `~/.cursor/skills-cursor/` son internos (create-skill, create-rule, sdk, etc.). **No los copies ni edites.**

## Guías

- [docs/03-skills.md](docs/03-skills.md) — Autoría y formato
- [docs/07-claude-skills-en-cursor.md](docs/07-claude-skills-en-cursor.md) — Usar skills de Claude en Cursor
- [docs/10-paso-a-paso.md](docs/10-paso-a-paso.md) — Tutorial completo
