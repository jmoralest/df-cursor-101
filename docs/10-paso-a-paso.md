# Guía paso a paso

Tutorial completo para dominar Cursor desde cero hasta un repo productivo con reglas, skills y GitHub.

---

## Fase 1 — Primeros pasos (15 min)

### 1.1 Instalar y abrir un proyecto

1. Descarga [Cursor](https://cursor.com)
2. Abre una carpeta de proyecto (`File → Open Folder`)
3. Inicia sesión si quieres modelos cloud

### 1.2 Primer chat con el agente

1. `Cmd+I` (Mac) o `Ctrl+I` (Windows/Linux)
2. Modo **Agent** para que pueda editar archivos
3. Prueba: *"Resume la estructura de este repositorio"*

### 1.3 Mencionar contexto

```
@README.md ¿Qué secciones faltan según docs/?
```

---

## Fase 2 — Reglas (30 min)

### 2.1 Crear tu primera regla

1. `Cursor Settings → Rules → New Rule`
2. O crea manualmente:

```bash
mkdir -p .cursor/rules
```

`.cursor/rules/typescript.mdc`:

```markdown
---
description: Convenciones TypeScript del proyecto
globs: **/*.{ts,tsx}
alwaysApply: false
---

# TypeScript

- Preferir `interface` para objetos públicos
- Manejar errores con tipos explícitos, no `catch (e) {}` vacío
```

### 2.2 AGENTS.md portable

Crea `AGENTS.md` en la raíz:

```markdown
# Agents

Lee `docs/architecture.md` antes de cambiar código en `src/`.
Los tests se ejecutan con `npm test`.
```

### 2.3 Verificar

Abre un `.ts` y pide al agente refactorizar algo — debe respetar la regla.

---

## Fase 3 — Skills (45 min)

### 3.1 Crear un skill de proyecto

```bash
mkdir -p .cursor/skills/release/SKILL.md
```

Contenido mínimo:

```markdown
---
name: release
description: Publica una nueva versión siguiendo semver y changelog. Usar cuando el usuario pida release, versionar o publicar.
---

# Release

1. Actualizar `CHANGELOG.md`
2. Bump versión en `package.json`
3. `git tag vX.Y.Z && git push --tags`
```

### 3.2 Invocar

En el agente: `/release` o describe la tarea y deja que el agente lo detecte.

### 3.3 Skill solo manual (estilo comando)

Añade al frontmatter:

```yaml
disable-model-invocation: true
```

---

## Fase 4 — Skill de Claude en Cursor (20 min)

### 4.1 Usar skill existente de Claude

Si ya tienes `~/.claude/skills/mi-skill/SKILL.md`:

1. Abre Cursor en cualquier repo
2. Ve a `Customize → Skills` — debe listarse
3. Ejecuta `/mi-skill`

### 4.2 Compartir en un repo

```bash
mkdir -p .claude/skills/deploy
# Crear SKILL.md (ver docs/07-claude-skills-en-cursor.md)
git add .claude/skills/
git commit -m "Add deploy skill"
```

Cursor y Claude Code comparten el skill al clonar el repo.

---

## Fase 5 — Memory y preferencias (10 min)

1. Trabaja con el agente varias sesiones
2. Cuando proponga una memory, **acepta solo lo útil**
3. Revisa `Settings → Rules` periódicamente
4. Para equipo: convierte memories estables en **project rules** en git

---

## Fase 6 — MCP (opcional, 30 min)

1. Identifica una fuente externa (DB, API, Slack)
2. Configura servidor en `.cursor/mcp.json` o Settings
3. Documenta en README qué variables de entorno necesita
4. Prueba: *"Consulta la tabla X usando MCP"*

---

## Fase 7 — Publicar en GitHub (15 min)

```bash
cd tu-proyecto
git init
git add .
git commit -m "Initial commit: Cursor harness and docs"

gh repo create jmoralest/mi-repo --public --source=. --push
```

O remoto manual:

```bash
git remote add origin git@github.com:jmoralest/mi-repo.git
git branch -M main
git push -u origin main
```

---

## Fase 8 — Checklist de repo maduro

```
[ ] README.md con mapa de docs
[ ] AGENTS.md para agentes
[ ] .cursor/rules/ con reglas acotadas
[ ] .cursor/skills/ para flujos repetibles
[ ] .gitignore (sin secretos)
[ ] docs/ para guías largas
[ ] (opcional) .cursor/mcp.json documentado
[ ] (opcional) .cursor/hooks.json para formato/lint
```

---

## Siguiente nivel

- [09-tips-trucos.md](09-tips-trucos.md)
- [Cursor SDK](https://cursor.com/docs/sdk) para CI/CD
- Repo de referencia: [ejemplo-harness-subagentes](https://github.com/jmoralest/ejemplo-harness-subagentes)
