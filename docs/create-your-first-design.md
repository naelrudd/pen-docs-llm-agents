# Create your first design

Create a shape, change it with an agent, and save your first `.pen` file.

Before you start, [install the desktop app](installation.md).
Sign in to pen.dev when prompted.

## Create a shape

Select **New File** on the desktop dashboard, then connect an agent:

- In the desktop app: Connect an AI provider
  in **Settings (⚙️) → Agents**, then choose a model in the agent composer
- From Claude Code or Codex: Connect the client
  and keep the document open in the desktop app

Send this request in the desktop agent composer or your connected client:

> In the current document, add a blue rectangle named First button,
> 160 pixels wide and 48 pixels tall, inside the existing frame.

The agent uses tools to change the canvas. If it asks for permission, review the
requested action before allowing it.

When the agent finishes, expand **Frame** in **Layers** and select **First
button**. In the properties panel, check that the rectangle is blue, with **W**
set to 160 and **H** set to 48.

## Change the selection

In the desktop app, the selected rectangle appears in the agent composer's
context. Send this follow-up request to the same agent:

> Change the selected rectangle's fill to #16A34A. Keep its size and position.

When the agent finishes, check that the rectangle is green. Click the rectangle
on the canvas before using **Edit → Undo** to undo the change. Use **Edit →
Redo** to restore it. An agent request can make several changes, so undo each
change you want to reverse.

## Save your file

New desktop documents save automatically as drafts. To keep this design in a
folder of your choice, select **File → Save As...** and save it as
`first-design.pen`.

After further edits, use Cmd/Ctrl + S to save changes to that file. See
[`.pen` Files](pen-files.md) for saving and recovery behavior.

## If the agent cannot start

If the desktop model picker has no model or the integrated agent reports an
authentication error, check your provider in **Settings (⚙️) → Agents**. Follow the
[provider troubleshooting steps](authentication.md), then retry the request.

If your external client cannot connect, follow the
[MCP troubleshooting steps](ai-integration.md).

See the [pen.dev Interface](pencil-interface.md) for more ways to
select and edit your design.