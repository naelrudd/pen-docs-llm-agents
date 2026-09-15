# AI Integration

Agents can work with pen.dev in two ways:

- Use an **integrated agent** from the desktop app
- Connect an **external MCP client** to a running pen.dev desktop or IDE host

## Use an integrated agent

Open or create a `.pen` document in the desktop app. Choose a model, then
enter a request in the agent composer.

See [Authentication](authentication.md) for account and provider
setup and [AI agents and Skills](ai-agents.md) for sessions, context, skills,
and design variants in the desktop app.

## Connect an external MCP client

1. Install the pen.dev desktop app or IDE extension
2. Open the `.pen` document the agent should use
3. Enable the client in **Settings (⚙️) → MCP**
4. Start or reload the external client
5. Confirm that `pencil` appears in the client's MCP server list

See [Installation](installation.md)
for Claude Code, ChatGPT and Codex setup.

Continue with [Create your first design](create-your-first-design.md)
to try your connected agent.

## MCP tools

The public server registers these top-level tools:

| Tool | Availability |
|------|--------------|
| `get_style` | Standard |
| `read_skill` | Standard |
| `get_app_state` | Standard |
| `execute` | Standard |
| `browser` | When the MCP server targets the desktop app |
| `spawn_agents` | Only when the MCP server starts with `-enable_spawn_agents` |

Check the client's live tool list for the tools in your installed version.

## Troubleshooting

### The integrated agent has no model

Open Settings (⚙️) → Agents and check the selected provider.

### The external client cannot connect

1. Confirm pen.dev is running and a `.pen` document is open
2. Reload the client after changing its MCP configuration

### A documented tool is missing

Inspect the live tool list. `browser` and `spawn_agents` are conditional.

### An agent is using the wrong document

Open the intended `.pen` file and include its full path in your prompt.