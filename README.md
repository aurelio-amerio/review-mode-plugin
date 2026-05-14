# review-mode-plugin

Plugin for the [Review Mode](https://marketplace.visualstudio.com/items?itemName=aurelio-amerio.review-mode) VS Code extension. Adds AI agent skills and commands for annotation-based document review workflows.

Works with **Claude Code** and **Cursor**.

## Prerequisites

The `review-mode-mcp` server must be installed and available on your `PATH`:

```bash
npm install -g review-mode-mcp
```

## Installation

### Claude Code

```
/plugin marketplace add https://github.com/aurelio-amerio/review-mode-plugin
/plugin install review-mode@review-mode-plugin
```

Skills are invoked as `/review-mode:review-mode`. Commands as `/review-mode:review-mode` and `/review-mode:update-plan`.

### Cursor

```
/add-plugin https://github.com/aurelio-amerio/review-mode-plugin
```

## Commands

| Command | Description |
|---------|-------------|
| `/review-mode` | Open a Markdown file in Review Mode for annotation-based feedback |
| `/update-plan` | Fetch annotations, implement changes, resolve comments, re-open in Review Mode |

## Skill

The `review-mode` skill is auto-loaded and teaches the agent how to interact with Review Mode: opening files, reading annotations, implementing feedback, and managing annotation lifecycle (resolve, clarify, reject).
