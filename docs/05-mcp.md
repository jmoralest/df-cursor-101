# MCP (Model Context Protocol)

MCP conecta el agente de Cursor con **herramientas y datos externos** de forma estandarizada.

## Configuración

Archivo típico: `.cursor/mcp.json` (proyecto) o configuración en Settings (usuario).

```json
{
  "mcpServers": {
    "mi-servidor": {
      "command": "npx",
      "args": ["-y", "mi-paquete-mcp"],
      "env": {
        "API_KEY": "..."
      }
    }
  }
}
```

## Qué permite

- Consultar bases de datos (BigQuery, SQL Server, etc.)
- Leer correo, Slack, logs (Sentry, Datadog)
- APIs internas de tu empresa
- Cualquier servidor MCP compatible

## Flujo

```
Usuario → Agente Cursor → CallMcpTool → Servidor MCP → Datos/Acción
```

El agente **descubre** herramientas del servidor y las invoca cuando son relevantes (tras leer el schema de cada tool).

## MCP vs Skills

| MCP | Skills |
|-----|--------|
| Protocolo + servidor externo | Archivos markdown en el repo |
| Datos en vivo | Conocimiento y procedimientos |
| Requiere credenciales/config | Versionable en git |

Son complementarios: un skill puede documentar *cómo* usar un MCP concreto.

## CLI sin MCP

El agente también ejecuta herramientas de terminal (`gh`, `docker`, `kubectl`) sin configuración MCP adicional.

## Buenas prácticas

1. Documentar en README qué MCPs requiere el proyecto
2. No commitear secretos; usar variables de entorno
3. Skills con `references/` para queries SQL o APIs repetitivas

## Referencias

- [Cursor MCP Docs](https://cursor.com/docs/context/mcp)
- [modelcontextprotocol.io](https://modelcontextprotocol.io)
