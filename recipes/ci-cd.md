# Recipe: CI/CD with pen.dev CLI

Automate design creation and export in your CI/CD pipeline.

## Setup

Set environment variables:

```bash
export PEN_CLI_KEY=pencil_cli_...
export ANTHROPIC_API_KEY=sk-ant-...
```

## Create a design in CI

```bash
pen --out onboarding.pen --prompt "Create a 3-step onboarding flow with illustrations"
```

## Export from CI

```bash
pen --in design.pen --export onboarding.png --export-scale 2
pen --in design.pen --export onboarding.pdf --export-type pdf
```

## Batch tasks

Create `tasks.json`:

```json
[
  { "out": "login.pen", "prompt": "Login page with email and password" },
  { "out": "signup.pen", "prompt": "Signup page with name, email, password, and social login" },
  { "out": "forgot.pen", "prompt": "Forgot password page with email input" }
]
```

```bash
pen --tasks tasks.json --repo .
```

## Use with GitHub Actions

```yaml
name: Generate Design
on:
  workflow_dispatch:

jobs:
  generate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '22'
      - run: npm install -g @pen.dev/cli
      - run: pen --out design.pen --prompt "Create a hero section with gradient background"
        env:
          PEN_CLI_KEY: ${{ secrets.PEN_CLI_KEY }}
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
      - uses: actions/upload-artifact@v4
        with:
          name: design
          path: design.pen
```