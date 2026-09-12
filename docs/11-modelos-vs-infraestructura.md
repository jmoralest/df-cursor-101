# Aprender a trabajar con modelos vs. construir infraestructura alrededor del modelo

> "El conocimiento que estás construyendo no pertenece al modelo."

Este documento recoge una discusión de fondo sobre **dónde conviene invertir el tiempo** cuando se aprende a trabajar con IA: ¿en dominar el modelo en sí (cómo razona, cómo especificarle un problema, cómo evaluar su respuesta) o en construir el andamiaje que lo rodea (skills, tools, harnesses, loops de validación)?

No son opciones excluyentes, pero sí tienen un **orden recomendado**. Este repo prioriza explícitamente la primera.

---

## La distinción de fondo

| | Aprender a trabajar con modelos | Construir infraestructura alrededor del modelo |
|--|----------------------------------|--------------------------------------------------|
| **Qué es** | Criterio para especificar problemas, dar contexto y evaluar resultados | Skills, tools, harnesses, loops de validación, orquestación |
| **Dónde vive el conocimiento** | En quien diseña el prompt/contexto | En código, configuración y automatizaciones |
| **Vida útil** | Transferible entre modelos y versiones | Atada a una versión concreta del modelo |
| **Cuándo rinde** | Desde el día uno | Cuando un problema concreto ya demostró que lo necesita |

La idea central: **el conocimiento que estás construyendo no pertenece al modelo**. No vive en los pesos de Claude, GPT o Gemini — vive en tu capacidad de especificar el problema, en tus documentos de contexto, en tu criterio para evaluar si una respuesta es buena. Esa capacidad **sí es tuya**, sobrevive el reemplazo de cualquier modelo y es lo único que realmente acumulas.

Esto conecta directo con la **capa de contexto** descrita en [arquitectura-capas.md](arquitectura-capas.md): rules, skills, memory y docs no son "trucos para el modelo actual", son la forma en que codificas tu propio conocimiento del problema para que cualquier modelo pueda usarlo.

---

## El riesgo de la infraestructura prematura

Un harness es fundamental cuando necesitas convertir:

> "un modelo que sabe hacer algo"

en

> "un sistema que hace algo de manera repetible, verificable y autónoma".

Por ejemplo, cuando ya tienes claro el problema, un harness con este flujo tiene muchísimo sentido:

```
Prompt
   │
   ▼
Modelo
   │
   ▼
Tool
   │
   ▼
Resultado
   │
   ▼
Validación
   │
   ▼
Modelo
   │
   ▼
Corrección
   │
   ▼
Resultado final
```

Pero si **todavía estás descubriendo cómo conseguir que el modelo razone correctamente sobre el problema**, construir todo ese andamiaje puede ser **premature optimization**.

### El problema de la capa intermedia

Cada skill, tool o harness que agregas es una capa entre el usuario y el modelo:

```
                    ┌── Skill A
                    │
Usuario → Harness ──┼── Skill B → Modelo
                    │
                    └── Tool C
```

El modelo evoluciona con el tiempo:

```
Modelo v1 → v2 → v3 → v4
```

Pero tu infraestructura, si no la revisas, **permanece congelada**:

```
Harness v1
Skills v1
Prompts v1
Rules v1
Loops v1
```

El riesgo concreto: terminas **limitando un modelo nuevo con decisiones que tomaste para uno anterior**. Un harness o skill mal mantenido puede entorpecer una capacidad nueva del modelo sin que te des cuenta — porque la capa intermedia sigue forzando el camino antiguo aunque el modelo ya no lo necesite.

Esto es sobreingeniería alrededor del LLM, y es exactamente el motivo por el que este repo organiza el arnés en capas explícitas (ver [arquitectura-capas.md](arquitectura-capas.md)) en vez de acumular skills y hooks sin criterio: cada capa debe poder justificarse, y revisarse cuando el modelo cambia.

---

## De "biblioteca de prompts" a "framework de contexto"

Si vas a invertir en algo estable, que no sea una colección de prompts sueltos por caso de uso:

```
❌ Biblioteca de prompts          ✅ Prompt/Context Framework

prompt_ventas.md                 framework/
prompt_sql.md                    │
prompt_powerbi.md                ├── system.md
prompt_sap.md                    ├── role.md
                                  ├── context.md
                                  ├── objective.md
                                  ├── constraints.md
                                  ├── methodology.md
                                  ├── output.md
                                  ├── validation.md
                                  │
                                  ├── templates/
                                  │   ├── analysis.md
                                  │   ├── coding.md
                                  │   ├── architecture.md
                                  │   ├── troubleshooting.md
                                  │   └── decision.md
                                  │
                                  └── variables/
                                      ├── topic
                                      ├── domain
                                      ├── objective
                                      ├── audience
                                      └── constraints
```

La diferencia no es de tamaño, es de naturaleza: una biblioteca es una colección de casos particulares; un framework es una **estructura reutilizable** que se combina con variables y contexto del problema para compilar el prompt final:

```
FRAMEWORK
     +
VARIABLES
     +
CONTEXTO DEL PROBLEMA
     │
     ▼
PROMPT COMPILADO
     │
     ▼
MODELO
```

Un framework así es casi un lenguaje declarativo para especificar problemas a un LLM:

```
ROLE = {{role}}
DOMAIN = {{domain}}
OBJECTIVE = {{objective}}
CONTEXT = {{context}}
DATA = {{data}}
CONSTRAINTS = {{constraints}}
OUTPUT = {{output}}
```

Y es el camino contrario al "prompt infinito": no se trata de escribir prompts más largos, sino mejor diseñados.

---

## El verdadero cambio de enfoque

No pienses:

> "Quiero ser bueno haciendo prompts."

Piensa:

> "Quiero aprender a especificar problemas para modelos de lenguaje."

El prompt es solamente el mecanismo de transporte. El activo real es saber definir:

- qué problema estamos resolviendo
- qué sabe el modelo
- qué no sabe
- qué contexto necesita
- qué debe considerar
- qué puede asumir
- qué no puede asumir
- cómo debe razonar
- qué resultado esperamos
- cómo sabemos que el resultado es bueno

Eso es más profundo — y más duradero — que reglas de formato tipo "usa XML", "usa Markdown", "pon la instrucción al final", "usa delimitadores". Esos trucos cambian con cada modelo nuevo; el criterio para especificar un problema, no.

---

## Dónde está la ventaja competitiva real

Competir por ser "experto genérico en prompting" es una carrera sin foso. Lo que sí es defendible es la combinación de **conocimiento de dominio + capacidad de estructurar problemas + LLM**:

```
                   MODELO
                     ▲
                     │
             Prompt Framework
                     ▲
                     │
        ┌────────────┴────────────┐
        │                         │
   Data Strategy            Integration
        │                         │
   SAP / BQ / PBI             APIs / AWS
        │                         │
        └──────────┬──────────────┘
                    │
              PROBLEMA NEGOCIO
```

El framework de contexto es la capa que traduce un problema de negocio concreto (con su dominio, sus datos, sus restricciones) en algo que el modelo puede resolver bien. Sin ese dominio, el framework queda vacío; sin el framework, el dominio no escala más allá de lo que una persona puede escribir a mano cada vez.

---

## Distribución recomendada de horas de aprendizaje

Si hay que repartir 100 horas de aprendizaje de IA:

| Área | Horas |
|------|-------|
| Diseño de prompts/contextos | 40 |
| Evaluación de resultados | 20 |
| Comprensión de modelos/LLM | 15 |
| RAG / context engineering | 10 |
| Tools / MCP | 7 |
| Agents / harness / loops | 5 |
| Skills | 3 |

No porque skills, tools y harnesses sean malos — de hecho este repo dedica varios documentos a ellos ([03-skills.md](03-skills.md), [05-mcp.md](05-mcp.md), [arquitectura-capas.md](arquitectura-capas.md)) — sino porque son **capas que se aprenden relativamente rápido cuando un problema real las exige**. Desarrollar criterio para decir *"este problema necesita este contexto, estas instrucciones, estas variables y este mecanismo de evaluación"* es una habilidad más difícil de adquirir y mucho más transferible entre modelos, proyectos y herramientas.

---

## La regla

> No construyas infraestructura para compensar una incapacidad que todavía puedes resolver mejor diseñando el contexto.

Skills, tools y harnesses siguen teniendo un lugar en este repo — pero **después**: una vez que el framework de contexto y el criterio de evaluación ya están sólidos, y un problema concreto demuestra que necesita repetibilidad, verificación o autonomía que el contexto por sí solo no puede dar.

---

## Referencias en este repo

| Tema | Archivo |
|------|---------|
| Arquitectura en capas (contexto / control / orquestación) | [arquitectura-capas.md](arquitectura-capas.md) |
| Skills | [03-skills.md](03-skills.md) |
| Memory | [04-memory.md](04-memory.md) |
| MCP | [05-mcp.md](05-mcp.md) |
| Hooks (capa de control) | [08-hooks.md](08-hooks.md) |
