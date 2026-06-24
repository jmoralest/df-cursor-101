# Skills

Los **skills** empaquetan conocimiento y flujos de trabajo que el agente carga **bajo demanda**, ahorrando contexto frente a reglas siempre activas.

## Estándar abierto

Agent Skills es un estándar portable: [agentskills.io](https://agentskills.io). Cursor, Claude y otros agentes compatibles usan el mismo formato `SKILL.md`.

## Estructura

```
mi-skill/
├── SKILL.md           # obligatorio
├── scripts/           # opcional: ejecutables
├── references/        # opcional: docs adicionales
└── assets/            # opcional: plantillas, datos
```

## Formato SKILL.md

```markdown
---
name: mi-skill
description: Qué hace y CUÁNDO usarlo (tercera persona, términos clave).
paths: "**/*.tsx"       # opcional: limitar a archivos
disable-model-invocation: true   # opcional: solo con /mi-skill
---

# Mi Skill

## Instrucciones
1. Paso uno
2. Paso dos
```

### Campos importantes

| Campo | Requerido | Notas |
|-------|-----------|-------|
| `name` | Sí | minúsculas, guiones; coincide con carpeta |
| `description` | Sí | Crítico para que el agente lo descubra |
| `paths` | No | Glob(s); reemplaza `globs` legacy |
| `disable-model-invocation` | No | `true` = solo invocación explícita (`/`) |

## Cómo invocar

- **Automático**: el agente lee la descripción y carga el skill si encaja
- **Manual**: `/nombre-skill` en el chat del agente
- **@-mención**: `@crear-readme` (según versión de Cursor)

## Rules vs Skills vs Hooks

| | Rules | Skills | Hooks |
|--|-------|--------|-------|
| Carga | Estática / por archivo | Dinámica | Evento del sistema |
| Modelo puede ignorar | Sí | Sí | **No** (determinista) |
| Ideal para | Estilo, convenciones | Flujos multi-paso | Lint, formato, guardas |

## Autoría efectiva

1. Descripción específica con palabras disparadoras
2. SKILL.md &lt; 500 líneas; detalle en `references/`
3. Scripts reutilizables en `scripts/` para operaciones frágiles
4. Un skill = un dominio (`deploy-app`, no `utils`)

## Ejemplo en este repo

Ver `.cursor/skills/crear-readme/SKILL.md`.

## Instalar desde GitHub

`Customize → Rules → Add Rule → Remote Rule (Github)` con URL del repo que contiene skills.
