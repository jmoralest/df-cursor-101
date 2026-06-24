# Memory — Referencia rápida

La memoria en Cursor complementa reglas y skills: aprende preferencias **entre sesiones** sin que tengas que repetirlas.

## Tipos

| Tipo | Dónde vive | Quién la gestiona |
|------|------------|-------------------|
| **User Rules** | Cursor Settings → Rules | Tú (manual) |
| **Project Rules** | `.cursor/rules/*.mdc` | Equipo (git) |
| **Memories** | Generadas por el agente | Cursor (automático, con tu aprobación) |
| **AGENTS.md / CLAUDE.md** | Raíz o subcarpetas | Tú (git) |

## Cómo se crean las Memories

1. Trabajas con el agente en un proyecto
2. El agente detecta una preferencia recurrente (estilo, stack, convención)
3. Te propone guardarla como memory
4. Tú aceptas o rechazas

Las memories **no sustituyen** reglas de equipo versionadas. Úsalas para preferencias personales o descubrimientos locales.

## Buenas prácticas

- **Reglas en git** para lo que el equipo debe cumplir siempre
- **Skills** para flujos que solo a veces necesitas
- **Memories** para matices personales que el agente infiere con el tiempo
- Revisa periódicamente Settings → Rules → Memories y elimina las obsoletas

## Más detalle

Ver [docs/04-memory.md](docs/04-memory.md)
