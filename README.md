# review-mode-plugin

Plugin for the [Review Mode](https://marketplace.visualstudio.com/items?itemName=aurelio-amerio.review-mode) VS Code extension. Adds AI agent skills and commands for annotation-based document review workflows.

Works with **Claude Code**, **Cursor**, and **Codex**.

## Prerequisites

Install the [Review Mode](https://marketplace.visualstudio.com/items?itemName=aurelio-amerio.review-mode) VS Code extension. It automatically sets up the required MCP server.

## Installation

### Claude Code

Run these commands inside Claude Code:

```
/plugin marketplace add https://github.com/aurelio-amerio/review-mode-plugin
/plugin install review-mode@review-mode-plugin
```

Skills are invoked as `/review-mode:review-mode`. Commands as `/review-mode:review-mode` and `/review-mode:update-plan`.

### Cursor

Clone the repository into Cursor's local plugins directory:

```bash
git clone --depth 1 https://github.com/aurelio-amerio/review-mode-plugin ~/.cursor/plugins/local/review-mode-plugin
```

### Codex

Add the plugin from the GitHub marketplace:

```bash
codex plugin marketplace add aurelio-amerio/review-mode-plugin
```

Once added, browse and install the plugin from the Codex plugin directory.

## Commands

| Command | Description |
|---------|-------------|
| `/review-mode` | Open a file in Review Mode for annotation-based feedback |
| `/update-plan` | Fetch annotations, implement changes, resolve comments, and re-open in Review Mode (for Markdown plan files) |
| `/implement-review` | Fetch annotations, implement changes, and resolve comments (for any file type: code, config, docs) |

## Skill

The `review-mode` skill is auto-loaded and teaches the agent how to interact with Review Mode: opening files, reading annotations, implementing feedback, and managing annotation lifecycle (resolve, clarify, reject).
