# Usar skills de Claude en Cursor

**Sí, es posible.** Cursor implementa el estándar [Agent Skills](https://agentskills.io) y carga skills desde rutas de Claude además de las propias.

## Rutas compatibles

Cursor descubre `SKILL.md` en:

| Ruta proyecto | Ruta usuario |
|---------------|--------------|
| `.cursor/skills/` | `~/.cursor/skills/` |
| `.agents/skills/` | `~/.agents/skills/` |
| **`.claude/skills/`** | **`~/.claude/skills/`** |
| `.codex/skills/` | `~/.codex/skills/` |

Documentación oficial: [cursor.com/docs/context/skills](https://cursor.com/docs/context/skills)

## Escenario A — Skill personal de Claude Code

Ya tienes un skill en `~/.claude/skills/graphify/SKILL.md`:

1. Abre cualquier proyecto en **Cursor**
2. El agente lo descubre al iniciar (misma ruta usuario)
3. Invócalo con `/graphify` o deja que el agente lo aplique por la `description`

**No hace falta copiar ni convertir** si el formato es estándar.

## Escenario B — Skill en un repo (compartir equipo)

1. Crea la carpeta en el repo:

```bash
mkdir -p .claude/skills/mi-flujo
```

2. Añade `SKILL.md`:

```markdown
---
name: mi-flujo
description: Despliega el entorno de staging. Usar cuando el usuario mencione deploy, staging o release.
---

# Mi flujo

1. Ejecutar `scripts/deploy.sh staging`
2. Verificar health check en `/api/health`
```

3. Commit y push — Cursor y Claude Code en ese repo ven el mismo skill.

**Alternativa equivalente**: usar `.cursor/skills/` o `.agents/skills/` (preferido si el repo es solo Cursor).

## Escenario C — Copiar skill de un plugin Claude

Los plugins de Claude viven en rutas como:

`~/.claude/plugins/.../skills/.../SKILL.md`

Para usarlos en Cursor:

```bash
# Ejemplo: copiar a skills de usuario de Cursor
cp -R ~/.claude/plugins/.../skills/mi-skill ~/.cursor/skills/mi-skill
```

O enlazar:

```bash
ln -s ~/.claude/skills/mi-skill ~/.cursor/skills/mi-skill
```

## Compatibilidad de formato

| Elemento Claude | Cursor |
|-----------------|--------|
| `name` + `description` en frontmatter | ✅ Requerido |
| Cuerpo markdown | ✅ |
| `scripts/` | ✅ |
| `references/`, `assets/` | ✅ |
| `paths` / `globs` para alcance | ✅ (`paths` preferido) |
| `disable-model-invocation` | ✅ |
| Comandos `/` en el cuerpo | ✅ (invocación manual) |

## Diferencias a tener en cuenta

1. **Hooks de Claude** (`.claude/settings.json`) no son iguales a **Hooks de Cursor** (`.cursor/hooks.json`) — configurar por separado
2. **`CLAUDE.md`** no se lee automáticamente como regla en Cursor; usa `AGENTS.md` o `.cursor/rules/`
3. Skills **built-in** de Cursor (`~/.cursor/skills-cursor/`) no son skills de Claude

## Verificar que Cursor ve el skill

1. `Customize` en la barra lateral → **Skills**
2. Debe aparecer junto a las reglas en "Agent Decides"
3. Prueba `/nombre-del-skill` en el chat

## Instalar desde GitHub

`Customize → Rules → Add Rule → Remote Rule (Github)` con un repo que contenga skills en cualquiera de las rutas estándar.

## Resumen

```
Skill de Claude (SKILL.md)
        │
        ├── ~/.claude/skills/     ──► Cursor lo carga automáticamente
        ├── .claude/skills/       ──► En el repo, ambos IDEs
        ├── ~/.cursor/skills/     ──► Copia o symlink
        └── .cursor/skills/       ──► Idiomático en repos Cursor-first
```
