---
name: crear-readme
description: Genera o actualiza README.md siguiendo la plantilla y estructura de df-cursor-101. Usar cuando el usuario pida documentar un proyecto, crear un README educativo sobre Cursor, o sincronizar documentación con GitHub.
---

# Crear README

## Plantilla mínima

```markdown
# Nombre del proyecto

> Una línea que explique el propósito.

## Contenido

| Tema | Guía |
|------|------|
| ... | `docs/...` |

## Inicio rápido

\`\`\`bash
# comandos esenciales
\`\`\`

## Estructura del repositorio

\`\`\`
.
├── README.md
├── .cursor/
└── docs/
\`\`\`

## Referencias

- [Cursor Docs](https://cursor.com/docs)
```

## Pasos

1. Identificar audiencia (principiante / equipo)
2. Listar secciones obligatorias del README principal
3. Mover detalle largo a `docs/` y enlazar desde README
4. Verificar que ejemplos de `.cursor/` coinciden con lo documentado
5. Si hay repo remoto, indicar `git push` tras commit
