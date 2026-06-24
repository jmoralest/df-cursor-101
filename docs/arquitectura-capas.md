# Arquitectura en capas del arnés de agentes

Este documento define las **tres capas** que conforman un arnés (harness) para trabajar con agentes de IA en Cursor y herramientas similares.

```
                    ARNÉS COMPLETO
┌─────────────────────────────────────────────────────┐
│  CAPA DE CONTEXTO                                    │
│  Rules · Skills · Memory · AGENTS.md · docs · MCP   │
├─────────────────────────────────────────────────────┤
│  CAPA DE CONTROL                                     │
│  Hooks · tests · init.sh · CHECKPOINTS.md            │
├─────────────────────────────────────────────────────┤
│  CAPA DE ORQUESTACIÓN                                │
│  Subagentes · progress/ · feature_list.json          │
└─────────────────────────────────────────────────────┘
                         │
                         ▼
                   Código de la aplicación
```

---

## Capa de contexto

### Definición

> Conjunto de reglas, skills, memorias, documentación, referencias y archivos @mencionados que configuran el **conocimiento y el comportamiento esperado** del agente en un proyecto, cargados de forma persistente o bajo demanda, **sin imponer ejecución automática ni división de trabajo**.

En una frase: todo lo que le dice al agente **qué debe saber y cómo debe comportarse**, sin ejecutar nada por sí misma.

### Qué incluye

| Pieza | Qué aporta | Dónde vive |
|-------|------------|------------|
| **Rules** | Convenciones fijas | `.cursor/rules/`, User Rules en Settings |
| **Skills** | Flujos y expertise bajo demanda | `.cursor/skills/`, `~/.cursor/skills/` |
| **Memory** | Preferencias aprendidas entre sesiones | Cursor Settings → Rules → Memories |
| **AGENTS.md / AGENT.md** | Mapa de entrada: qué leer y en qué orden | Raíz del repo |
| **docs/** | Conocimiento largo | `docs/`, `README.md` |
| **MCP** | Datos y herramientas externas | `.cursor/mcp.json` |
| **@archivos** | Código concreto en este momento | Menciones en el chat |

### Niveles: proyecto vs usuario

| Nivel | Ubicación | Alcance | En git |
|-------|-----------|---------|--------|
| **Proyecto** | `.cursor/rules/`, `.cursor/skills/` | Solo este repo | ✅ |
| **Usuario** | Settings, `~/.cursor/skills/` | Todos tus proyectos | ❌ |
| **Interno Cursor** | `~/.cursor/skills-cursor/` | Built-in de Cursor | ❌ No editar |

Cursor **mezcla** todas las capas disponibles al abrir un proyecto. La UI **Customize** muestra rules y skills de proyecto, usuario e internos en un solo sitio.

### Cómo se activan las Rules

| Modo | Configuración | ¿Quién decide? |
|------|---------------|----------------|
| Siempre | `alwaysApply: true` | Cursor (automático) |
| Por archivo | `globs: **/*.py` | Cursor (automático) |
| Inteligente | Solo `description` | El modelo (aproximado) |
| Manual | `@nombre-regla` | Tú |

Las **Memories** aceptadas se incluyen en futuros chats sin que el agente las elija cada vez.

### Cómo se activan los Skills

- El agente lee la `description` y carga el skill si encaja
- Invocación manual: `/nombre-skill` o `@nombre-skill`
- Con `disable-model-invocation: true` solo entran si tú los invocas

### Valor para el desarrollador

- No repetir stack, estilo y convenciones en cada chat
- Ahorrar contexto: skills y docs largos solo cuando aplican
- Compartir conocimiento con el equipo vía git
- Reducir alucinaciones de stack y estilo

### Parte de verdad

| Verdad | Implicación |
|--------|-------------|
| El agente puede ignorar parte del contexto | Lo crítico va en la capa de control |
| `alwaysApply` consume ventana de contexto | Pocas rules, cortas |
| Memory no está en git | No usarla para estándares de equipo |
| Más contexto no siempre es mejor | Priorizar lo esencial |

**Analogía:** briefing antes del partido — táctica y reglas del juego.

---

## Capa de control

### Definición

> Mecanismos **deterministas** que se ejecutan sin depender de que el modelo recuerde o obedezca: scripts, hooks, tests, verificaciones de entorno y criterios explícitos de calidad.

En una frase: **que pase o no pase**, aunque el agente quiera saltarse pasos.

### Qué incluye

| Pieza | Qué hace | Ejemplo |
|-------|----------|---------|
| **Hooks** | Scripts en eventos del IDE | Formatear tras `afterFileEdit` |
| **Tests** | Verificación automática del código | `pytest`, `npm test` |
| **init.sh** | Comprobar que el entorno está listo | Deps instaladas, tests verdes al inicio |
| **CHECKPOINTS.md** | Definición de «terminado correcto» | «Todo endpoint tiene test» |
| **CI/CD** | Control en remoto | GitHub Actions bloquea merge si falla |

### Ejemplo

1. El agente edita `auth.py`
2. Un **hook** formatea el archivo automáticamente
3. `pytest` falla → no se da por bueno el cambio
4. **CHECKPOINTS.md** exige test de logout → se revisa antes de cerrar

### Valor para el desarrollador

- Garantías sin confiar en la memoria del modelo
- Formato y convenciones aplicados de forma consistente
- Menos regresiones si el agente edita solo

### Parte de verdad

| Verdad | Implicación |
|--------|-------------|
| No es obligatoria desde el día uno | Tests + revisar el diff suele bastar al principio |
| Hooks lentos o mal diseñados molestan | Diseñar con cuidado, empezar simple |
| Los tests son el control más rentable | Priorizar CI antes que hooks complejos |

**Cuándo merece la pena:** equipo, repo grande, agente autónomo durante mucho tiempo, políticas no negociables (secretos, comandos peligrosos).

**Analogía:** árbitro y VAR — aplica las reglas pase lo que pase.

---

## Capa de orquestación

### Definición

> Estructura que **divide el trabajo** en tareas acotadas, roles, pasos secuenciales y artefactos de seguimiento en disco, para evitar chats infinitos donde el agente mezcla tareas y pierde el hilo.

En una frase: **una cosa cada vez, con acta escrita**.

### Qué incluye

| Pieza | Qué hace | Ejemplo |
|-------|----------|---------|
| **feature_list.json** | Alcance incremental | Una feature `in_progress` a la vez |
| **progress/** | Trazas en disco | `progress/impl_login.md`, `review_login.md` |
| **Subagentes** | Roles especializados | Leader → implementer → reviewer |
| **AGENTS.md** | Orden de lectura y restricciones | «No codees sin leer `docs/architecture.md`» |

### Ejemplo

1. `feature_list.json`: solo `login` en `in_progress`
2. Leader escribe el plan en `progress/current.md`
3. Implementer codea y documenta en `progress/impl_login.md`
4. Reviewer valida contra `CHECKPOINTS.md`
5. Si OK → `login` pasa a `done`; siguiente feature

### Valor para el desarrollador

- Sesiones largas sin caos en el chat
- Auditoría: qué decidió cada paso y cuándo
- Paralelismo con subagentes en tareas independientes

### Parte de verdad

| Verdad | Implicación |
|--------|-------------|
| Añade overhead | Overkill para un fix de cinco minutos |
| Brilla en proyectos grandes o largos | Varias features, varias sesiones |
| `progress/` solo sirve si lo consultas | El disco sustituye al chat como memoria de trabajo |
| Subagentes consumen más tokens y tiempo | Más orden, no siempre más velocidad |
| Orquestación «light» funciona | Lista en markdown + «haz solo el punto 2» |

**Cuándo merece la pena:** agente trabajando horas, muchas features, necesidad de auditar decisiones.

**Analogía:** entrenador y kanban — quién entra, en qué minuto, con acta escrita.

---

## Cómo se relacionan las tres capas

| Capa | Pregunta que responde | Naturaleza | Confías en el modelo |
|------|----------------------|------------|---------------------|
| **Contexto** | ¿Qué debe saber y cómo actuar? | Información (blanda) | Parcialmente |
| **Control** | ¿Cumple calidad y reglas duras? | Ejecución (dura) | No |
| **Orquestación** | ¿Qué hace ahora y quién lo hace? | Organización | Parcialmente |

Si algo sale mal:

- Diseño o convenciones incorrectas → revisar **contexto**
- Código roto o sin formato → revisar **control**
- Mezcla de tareas o trabajo a medias → revisar **orquestación**

---

## Qué usar según el proyecto

| Situación | Mínimo razonable |
|-----------|------------------|
| Aprender / repo educativo | Capa de contexto |
| Código propio, tareas cortas | Contexto + tests |
| Repo de equipo | Contexto en git + CI |
| Agente autónomo, muchas features | + orquestación light (`progress/`, feature list) |
| Harness completo | Las tres capas |

---

## Referencias en este repo

| Tema | Archivo |
|------|---------|
| Rules | [02-reglas.md](02-reglas.md) |
| Skills | [03-skills.md](03-skills.md) |
| Memory | [04-memory.md](04-memory.md) |
| Hooks (control) | [08-hooks.md](08-hooks.md) |
| Graphify (mapa del repo) | [graphify.md](graphify.md) |
| Harness de referencia externo | [ejemplo-harness-subagentes](https://github.com/jmoralest/ejemplo-harness-subagentes) |

---

## Resumen

- **Contexto** informa y guía — es la base del arnés.
- **Control** obliga y verifica — existe porque informar no basta.
- **Orquestación** organiza el trabajo — existe porque un chat largo se desordena solo.

Las rules, skills, memory y documentación conforman la **capa de contexto**. Hooks, tests y checkpoints son **control**. Feature lists, progress y subagentes son **orquestación**. Juntas forman el arnés completo; no todas son necesarias en todos los proyectos.
