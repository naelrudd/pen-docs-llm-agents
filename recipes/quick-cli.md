# Recipe: Quick CLI Start

Create and modify pen.dev designs from the command line.

## Prerequisites

- Node.js 22.19+
- pen.dev CLI installed: `npm install -g @pen.dev/cli`
- Authenticated: `pen login` or set `PEN_CLI_KEY`

## Create a new design

```bash
pen --out login.pen --prompt "Create a login page with email and password fields"
```

## Modify an existing design

```bash
pen --in dashboard.pen --out dashboard-v2.pen --prompt "Add a sidebar navigation"
```

## Export to image

```bash
pen --in design.pen --export hero.png --export-scale 2
```

## Batch operations with tasks file

Create a JSON file with multiple prompts:

```json
[
  { "out": "step1.pen", "prompt": "Create a basic layout with header and footer" },
  { "out": "step2.pen", "prompt": "Add a sidebar to the left" }
]
```

```bash
pen --tasks tasks.json --repo .
```