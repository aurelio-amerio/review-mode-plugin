# review-mode-plugin

Plugin for the [Review Mode](https://marketplace.visualstudio.com/items?itemName=aurelio-amerio.review-mode) VS Code extension. Adds AI agent skills and commands for annotation-based document review workflows.

Works with **Claude Code**, **GitHub Copilot**, **Cursor**, and **Codex**.

For full documentation, see the [Review Mode GitHub page](https://github.com/aurelio-amerio/review-mode).

## Prerequisites

1. Install the [Review Mode](https://marketplace.visualstudio.com/items?itemName=aurelio-amerio.review-mode) VS Code extension.
2. Install [`uv`](https://docs.astral.sh/uv/getting-started/installation/) if you don't have it already.
3. Install the MCP server:
   ```bash
   uv tool install review-mode-mcp
   ```

## Installation
This plugin is not currently published on the official marketplace, as such it needs to be installed as a local plugin.

### Claude Code

```
/plugin marketplace add https://github.com/aurelio-amerio/review-mode-plugin
/plugin install review-mode@review-mode-plugin
```

### GitHub Copilot

Open the Command Palette and run **Chat: Install Plugin From Source**, then enter:

```
https://github.com/aurelio-amerio/review-mode-plugin
```

### Cursor

```bash
git clone --depth 1 https://github.com/aurelio-amerio/review-mode-plugin ~/.cursor/plugins/local/review-mode-plugin
```

### Codex

```bash
codex plugin marketplace add aurelio-amerio/review-mode-plugin
```
