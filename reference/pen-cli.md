# pen.dev CLI

The pen.dev CLI is a standalone command-line tool for creating and editing `.pen` design files from the terminal. It runs the same editor engine as the desktop app and IDE extension, fully headless and without a GUI.

Use it to run the AI agent with a prompt, call MCP tools directly in an interactive shell, or export to PNG/JPEG/WEBP/PDF.

## Installation

```bash
npm install -g @pen.dev/cli
```

Verify the installation:

```bash
pen version
```

Requires Node.js 22.19 or later.

## Authentication

The CLI requires authentication for agent operations, exports, interactive mode, and workspace access. There are two methods.

### Interactive Login

```bash
pen login
```

This starts an interactive session where you choose your login method (email + password or email + OTP code). On success the session token is stored in `~/.pencil/session-cli.json`.

### CLI Key (for CI/CD)

Set the `PEN_CLI_KEY` environment variable. CLI keys are scoped to an organization and can be created in the Developer Keys section of your organization settings on the pen.dev web app.

```bash
PEN_CLI_KEY=pencil_cli_... pen --out design.pen --prompt "Create a form"
```

The CLI key always takes precedence over a stored session token.

### Checking Status

```bash
pen status
```

Displays the current authentication method, verifies the session with the backend, and shows account details.

## Quick Start

```bash
# Log in first
pen login

# Create a new design from scratch
pen --out design.pen --prompt "Create a login page with email and password fields"

# Modify an existing design
pen --in existing.pen --out modified.pen --prompt "Add a blue submit button"

# Export a design to PNG
pen --in design.pen --export design.png

# Start an interactive shell
pen interactive -o design.pen

# List available models
pen --list-models
```

## Commands

### pen login

Log in interactively via email + password or email + OTP.

### pen status

Check authentication status and display account details.

### pen codex-login

Sign in to OpenAI Codex with a ChatGPT Plus/Pro account (browser or device-code flow). Required before using `--agent codex` or a `gpt-*` model without an API key.

### pen codex-logout

Remove the stored OpenAI Codex credential.

### pen version

Print the installed CLI version.

### pen interactive

Start an interactive tool shell. See [Interactive Mode](#interactive-mode) below.

## Agent Mode

Run the AI agent with a prompt to create or modify designs.

| Option | Description |
|--------|-------------|
| `--in`, `-i <path>` | Input `.pen` file (optional; starts with an empty canvas if omitted) |
| `--out`, `-o <path>` | Output `.pen` file path (required unless `--export` is used) |
| `--prompt`, `-p <text>` | Prompt for the AI agent |
| `--prompt-file`, `-f <path>` | Attach an image or text file to the prompt (repeatable) |
| `--agent <type>` | Agent to use when `--model` is omitted: `claude`, `codex`, or `gemini` |
| `--model`, `-m <id>` | Model to use; the agent is inferred from the model ID |
| `--effort <level>` | Reasoning effort |
| `--usage <path>` | Write final token usage and cost to JSON |
| `--custom`, `-c` | Use custom Claude model configuration |
| `--list-models` | List available models and exit |
| `--tasks`, `-t <path>` | JSON tasks file for batch operations |
| `--repo`, `-C <path>` | Local folder to use as the agent's working directory |
| `--workspace <slug>` | Select the current cloud workspace |
| `--list-workspaces` | List available organizations and workspaces, then exit |
| `--export`, `-e <path>` | Export an image of the final result |
| `--export-scale <n>` | Export scale factor (default: 1) |
| `--export-type <type>` | Export format: `png`, `jpeg`, `webp`, or `pdf` (default: `png`) |
| `--preview-output <path>` | Set the preview PNG path |
| `--enable-preview` | Save a preview image after each design change |
| `--verbose-mcp` | Include full MCP tool error details in responses |
| `--verbose`, `-v` | Stream model thinking and full tool call input and output |
| `--max-failed-calls <n>` | Abort after this many failed tool calls |
| `--help`, `-h` | Show the help message |

### Examples

Create a new design:

```bash
pen --out login.pen --prompt "Create a modern login page with:
- Email input field
- Password input field
- Sign In button
- Forgot password link
- Social login options (Google, GitHub)"
```

Modify an existing design:

```bash
pen --in dashboard.pen --out dashboard-v2.pen --prompt "Add a sidebar navigation with:
- Dashboard link (active)
- Users link
- Settings link
- Logout button at bottom"
```

Export to image:

```bash
pen --in design.pen --export hero.png --export-scale 2
```

### Available Models

List the models available for each agent:

```bash
pen --list-models --agent claude
pen --list-models --agent codex
pen --list-models --agent gemini
```

Pass one of the returned model IDs to `--model`.

## Interactive Mode

The interactive shell lets you call MCP tools directly on `.pen` files. It is useful for scripting, debugging, and agentic workflows that need fine-grained control over design operations.

| Option | Description |
|--------|-------------|
| `--app`, `-a <name>` | Connect to a running pen.dev app (for example, `desktop`) |
| `--in`, `-i <path>` | Input `.pen` file (optional; uses an empty canvas if omitted) |
| `--out`, `-o <path>` | Output `.pen` file (required in headless mode) |
| `--preview-output <path>` | Set the preview PNG path |
| `--enable-preview` | Save a preview image after each design change |
| `--help`, `-h` | Show the help message |

### App Mode

Connects to a running pen.dev app. Changes are applied live.

```bash
pen interactive -a desktop -i my-design.pen
```

### Headless Mode

Spins up a local editor without a GUI. Use `save()` to write to the output file.

```bash
# New empty canvas
pen interactive -o output.pen

# Edit an existing file
pen interactive -i input.pen -o output.pen
```

### Shell Commands

| Command | Description |
|---------|-------------|
| `tool_name({ key: value })` | Call an MCP tool with arguments |
| `tool_name()` | Call an MCP tool with no arguments |
| `save()` | Save the document to disk |
| `exit()` | Exit the shell |

### Example Session

```bash
pen > read_skill()
pen > read_skill({ path: "pen-schema.md" })
pen > read_skill({ path: "execute.md" })
pen > get_app_state()
pen > get_style()
pen > execute({ input: 'hero=Insert(document,{type:"frame",name:"Hero",x:0,y:0,width:1440,height:900})' })
pen > execute({ input: 'TakeScreenshot([hero])' })
pen > save()
pen > exit()
```

Run `pen interactive --help` for the full tool reference with parameter types and descriptions.

### Supported MCP Tools

#### Design Operations

| Tool | Description |
|------|-------------|
| `execute` | Read, modify, render, and export the current document |
| `get_app_state` | Get document metadata and selection context |

#### Visual Operations

Screenshots are taken with the `execute` `TakeScreenshot()` operation. Files are exported with the `execute` `Export()` operation, which supports PNG/JPEG/WEBP/PDF images and HTML output.

#### Style & Guidelines

| Tool | Description |
|------|-------------|
| `read_skill` | Load instructions for the `.pen` schema, `execute`, and common workflows |
| `get_style` | List or load a visual style |

The `browser` tool is also available when the MCP server targets the desktop app. `spawn_agents` is available only when the server starts with `-enable_spawn_agents`.

## CI/CD Usage

For a Claude run in CI/CD, set a CLI key and Anthropic API key.

```bash
export PEN_CLI_KEY=pencil_cli_...
export ANTHROPIC_API_KEY=sk-ant-...

pen --out onboarding.pen --prompt "Create a 3-step onboarding flow"
```

## Environment Variables

| Variable | Description |
|----------|-------------|
| `PEN_CLI_KEY` | CLI API key for CI/CD (takes precedence over stored session) |
| `PEN_AGENT_API_KEY` | API key for the selected agent provider |
| `ANTHROPIC_API_KEY` | Anthropic API key for Claude |
| `PEN_API_BASE` | Backend API base URL (default: `https://api.pen.dev`) |
| `DEBUG` | Enable debug logging |

## Token Storage

| File | Purpose |
|------|---------|
| `~/.pencil/session-cli.json` | Session token from `pen login` |

The CLI uses a separate session file from the desktop app so the backend can distinguish which client is in use.