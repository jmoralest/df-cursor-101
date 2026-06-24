# Graphify — uso básico

[Graphify](https://github.com/search?q=graphifyy) convierte una carpeta de código y documentación en un **grafo de conocimiento** navegable: HTML interactivo, JSON y un informe en lenguaje natural.

En Cursor puedes invocarlo con el skill `/graphify` (desde `~/.claude/skills/graphify/` o si lo tienes en `~/.cursor/skills/`). También funciona como **CLI** en terminal.

---

## Requisito previo

```bash
# Instalar (una vez)
uv tool install graphifyy
# o
pip install graphifyy
```

Comprobar:

```bash
graphify --help
```

---

## Flujo rápido en este repo

Desde la raíz del proyecto:

```bash
cd /Volumes/JMTSSD/df-cursor-101

# Primera vez: construir el grafo completo
graphify .

# Siguientes veces: solo archivos nuevos o modificados
graphify update .

# Abrir la visualización en el navegador (macOS)
open graphify-out/graph.html
```

En una sola línea:

```bash
cd /Volumes/JMTSSD/df-cursor-101 && graphify update . && open graphify-out/graph.html
```

---

## Qué genera

Tras ejecutar graphify en un directorio aparece la carpeta `graphify-out/`:

| Archivo | Contenido |
|---------|-----------|
| `graph.html` | Grafo interactivo en el navegador |
| `graph.json` | Grafo en JSON (consultas, GraphRAG) |
| `GRAPH_REPORT.md` | Informe legible del corpus |
| `.graphify_labels.json` | Nombres de comunidades (leyenda del HTML) |
| `.graphify_python` | Intérprete usado (para subcomandos) |
| `.graphify_root` | Ruta escaneada (para `update` sin argumentos) |

---

## Comandos básicos

### Construcción

```bash
graphify .                          # Pipeline completo en el directorio actual
graphify /ruta/al/proyecto          # Pipeline en otra ruta
graphify . --update                 # Incremental: solo cambios
graphify update .                   # Igual: actualizar sin reescanear todo
graphify . --mode deep              # Extracción más profunda
graphify . --cluster-only           # Reagrupar comunidades sin re-extraer
graphify . --no-viz                 # Solo JSON + informe, sin HTML
```

### Consultas (con `graphify-out/graph.json` ya generado)

```bash
graphify query "¿Cómo se organizan las rules y skills?"
graphify query "¿Qué documenta arquitectura-capas?" --budget 1500
graphify path "Rules" "Skills"      # Camino más corto entre dos conceptos
graphify explain "AGENTS.md"        # Explicación de un nodo
```

### Repos remotos

```bash
graphify https://github.com/jmoralest/df-cursor-101
graphify https://github.com/owner/repo --branch main
```

### Exportaciones opcionales

```bash
graphify . --svg                    # graphify-out/graph.svg
graphify . --graphml                # Para Gephi / yEd
graphify . --wiki                   # Wiki por comunidades
graphify . --watch                  # Reconstruir al cambiar archivos
```

---

## Uso en Cursor (skill)

Con el skill instalado, en el chat del **Agente**:

```
/graphify .
/graphify . --update
/graphify query "¿Qué conecta README con docs?"
```

Si ya existe `graphify-out/graph.json`, preguntas sobre el repo pueden responderse con `graphify query` **sin reconstruir** el grafo.

---

## Cuándo usar cada comando

| Situación | Comando |
|-----------|---------|
| Primera vez en un repo | `graphify .` |
| Cambiaste docs o código | `graphify update .` |
| Ver el mapa visual | `open graphify-out/graph.html` |
| Pregunta sobre el repo ya mapeado | `graphify query "..."` |
| Repo grande, solo una carpeta | `graphify docs/` |
| Etiquetas "Community 0" genéricas | Ver sección **Nombres de comunidades** abajo |

---

## Nombres de comunidades (no "Community 0")

Tras `graphify update .` en terminal, el HTML suele mostrar **"Community 0", "Community 1"…**. Eso es normal: el comando de terminal **solo extrae y agrupa**; no nombra las comunidades.

El **paso de nombrar** (Step 5 del skill graphify) lo hace el **agente** leyendo los nodos de cada comunidad en `GRAPH_REPORT.md` y escribiendo `graphify-out/.graphify_labels.json`. En Cursor **no hace falta API key externa**: usa el modelo del agente con el que trabajas.

### Flujo en Cursor (tu caso)

```bash
cd /Volumes/JMTSSD/df-cursor-101
graphify update .
```

Luego en el chat del **Agente**:

```
/graphify
Etiqueta las comunidades del grafo según GRAPH_REPORT.md,
escribe graphify-out/.graphify_labels.json y regenera graph.html
```

O sin el skill, pídelo en lenguaje natural: el agente lee el informe, asigna nombres (2–5 palabras por comunidad) y ejecuta:

```bash
graphify export html
open graphify-out/graph.html
```

### Qué hace el agente (Step 5)

1. Lee `graphify-out/GRAPH_REPORT.md` — sección **Communities**
2. Por cada comunidad, mira los nodos y elige un nombre (ej. "Rules y reglas", "Arquitectura en capas")
3. Escribe `graphify-out/.graphify_labels.json`
4. Ejecuta `graphify export html`

### Dos modos de graphify (no confundir)

| Modo | Quién nombra comunidades |
|------|--------------------------|
| **Terminal** `graphify update .` | Nadie — quedan placeholders |
| **Agente** `/graphify` o pedir etiquetado | El agente de Cursor (tus modelos) |
| **Terminal** `graphify label .` | LLM externo (Gemini, etc.) — solo si corres CLI sin agente |

En Cursor trabajas con el agente; **no necesitas** `GOOGLE_API_KEY` para etiquetar.

### Nombres actuales en este repo

Ya aplicados en `.graphify_labels.json`: Guía paso a paso, README e índice, Arquitectura en capas, Skills — formato, Claude en Cursor, Fundamentos Cursor, Rules y reglas, etc.

```bash
open graphify-out/graph.html
```

---

## Integración con el arnés

Graphify encaja en la **capa de contexto**: ayuda a entender arquitectura y relaciones entre archivos sin leer todo el repo en cada chat.

Ver también: [arquitectura-capas.md](arquitectura-capas.md) y [07-claude-skills-en-cursor.md](07-claude-skills-en-cursor.md).

---

## Notas

- `graphify-out/` suele añadirse a `.gitignore` en proyectos de aplicación (artefacto generado).
- En corpus muy grandes (>500 archivos) graphify puede pedir acotar a un subdirectorio.
- El skill completo vive en `~/.claude/skills/graphify/SKILL.md` con el pipeline detallado.
