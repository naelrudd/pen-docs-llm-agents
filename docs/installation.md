# Installation

pen.dev is available as a desktop app, an IDE extension, and a command-line tool.

## Desktop app

Download the desktop app from the [pen.dev downloads page](https://www.pen.dev/downloads).

### macOS

1. Download the macOS package for your Mac
2. Open the downloaded DMG
3. Move pen.dev to Applications
4. Launch pen.dev

### Windows

1. Download the Windows installer
2. Run the installer
3. Launch pen.dev

### Linux

Download the AppImage or tarball for your system from the [pen.dev downloads page](https://www.pen.dev/downloads).

For an x64 AppImage:

```bash
chmod +x Pen-linux-x86_64.AppImage
./Pen-linux-x86_64.AppImage
```

For an ARM64 AppImage:

```bash
chmod +x Pen-linux-arm64.AppImage
./Pen-linux-arm64.AppImage
```

For an x64 tarball:

```bash
tar -xzf Pen-linux-x64.tar.gz
```

For an ARM64 tarball:

```bash
tar -xzf Pen-linux-arm64.tar.gz
```

### Check the version or update

Open the **pen.dev** menu on macOS, or **Help** on Windows or Linux. Select **About Pen** to check the installed version or **Check for Updates...** to look for an update.

## IDE extension

### VS Code and Cursor

1. Open the Extensions view in VS Code or Cursor
2. Search for **pen.dev**
3. Select **Install**
4. Create or open a file whose name ends in `.pen`

## CLI

The CLI requires Node.js 22.19 or later.

```bash
npm install -g @pen.dev/cli
```

Confirm the command is available:

```bash
pen version
```

To update, run `npm install -g @pen.dev/cli` again.

See the [CLI reference](../reference/pen-cli.md) before running an agent or editing a document headlessly.

## Connect an AI client with MCP

MCP lets an external AI client work with the `.pen` document open in the desktop app or IDE extension. pen.dev sets up supported MCP integrations when it starts.

Open **Settings (⚙️) → MCP** to see the integrations supported by your installed version. Desktop integrations include Claude Code CLI, Codex CLI, Gemini CLI, Antigravity 2.0, OpenCode CLI, Kiro CLI and Claude Desktop. The [ChatGPT desktop app and Codex IDE extension](https://learn.chatgpt.com/docs/extend/mcp) use the same MCP configuration as Codex CLI.

### Claude Code

Install Claude Code using the [official installation instructions](https://code.claude.com/docs/en/installation). If you use npm, run:

```bash
npm install -g @anthropic-ai/claude-code
```

Then:

1. Open a `.pen` document in pen.dev
2. Enable **Claude Code CLI** in **Settings (⚙️) → MCP**
3. Start or restart Claude Code
4. Use `/mcp` in Claude Code to confirm that `pencil` is connected

### ChatGPT and Codex

pen.dev configures the Codex MCP entry used by ChatGPT desktop, Codex CLI and the Codex IDE extension on the same computer.

1. Open a `.pen` document in pen.dev
2. Enable **Codex CLI** in **Settings (⚙️) → MCP**
3. Start or restart ChatGPT or Codex
4. Confirm that `pencil` appears in the client's MCP server list

To try your connected agent, follow [Create your first design](create-your-first-design.md).

## Troubleshooting

### A `.pen` file opens as text in the IDE

1. Confirm the pen.dev extension is installed and enabled
2. Run **pen.dev: Toggle Design Mode** from the Command Palette

### The `pen` command is not found

1. Confirm Node.js is version 22.19 or later
2. Re-run `npm install -g @pen.dev/cli`
3. Run `pen version`

For other symptoms, use [Troubleshooting](troubleshooting.md).