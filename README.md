# pen-docs-llm-agents

An unofficial, machine-readable mirror of the official [pen.dev documentation](https://docs.pen.dev/) — all pages converted to plain Markdown so AI coding assistants, LLM agents, RAG pipelines, and vector stores can consume them without scraping or HTML parsing.

pen.dev is a vector design tool that integrates directly into your development environment with an IDE extension, desktop app, and CLI. It uses `.pen` files (JSON-based, Git-friendly) and supports AI agents for vibe-designing, MCP integration, code-on-canvas script nodes, component systems with slots and themes, and two-way design-to-code workflows.

## Why this exists

- pen.dev ships docs for humans but not a plain cloneable corpus for agents.
- Agents often need to read many doc pages during integration work; a local, structured Markdown mirror is faster and more reliable than live-fetching pages.
- Deterministic snapshot: pin a commit and know exactly what docs your agent is reading.

## How to use

Point your agent at this repo (or a subfolder). Suggested flow:

1. **Discover** — read [`llms.txt`](llms.txt) or [`INDEX.md`](INDEX.md) for the full page catalog.
2. **Read** — load any page directly, e.g. `docs/installation.md`, `reference/pen-cli.md`, `recipes/quick-cli.md`.
3. **Embed/index** — feed `docs/`, `reference/`, `recipes/` into your vector store or context window.

### Quick index

| Section | Contents |
|---------|----------|
| [`docs/`](docs/) | Step-by-step guides — installation, authentication, AI integration, canvas interface, drawing, components, variables, slots, design libraries, themes, import/export, keyboard shortcuts, troubleshooting |
| [`reference/`](reference/) | Technical reference — `.pen` format spec (with full TypeScript schema), CLI commands and options, design-to-code workflows |
| [`recipes/`](recipes/) | Copy-paste recipes — quick CLI start, build a design system, script nodes, CI/CD automation |
| [`llms.txt`](llms.txt) | Full page index with original URLs |

## RAG Starter Kit

Local semantic search over the docs:

```bash
pip install chromadb
python rag.py build                    # index Markdown pages into ./rag_chroma
python rag.py query "your question"    # retrieve top-k relevant chunks
python rag.py info                     # corpus stats
```

Re-running `build` is idempotent and incremental.

## Updating

Re-mirror from the official docs:

```bash
# Fetch all pages from https://docs.pen.dev/ and convert to Markdown
# Each page URL follows the pattern: https://docs.pen.dev/{section}/{page}
```

## License & attribution

- This repository is **not affiliated with, endorsed by, or sponsored by pen.dev**.
- All documentation content is © pen.dev and belongs to its respective owners.
- It mirrors the official [pen.dev Documentation](https://docs.pen.dev/).
- This repo is provided "as is" for reference/educational purposes. Refer to [docs.pen.dev](https://docs.pen.dev/) for authoritative, always-current documentation.