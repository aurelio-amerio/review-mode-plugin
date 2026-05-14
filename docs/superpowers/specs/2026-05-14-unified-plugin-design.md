# Unified review-mode Plugin Design

**Date:** 2026-05-14  
**Status:** Approved

## Goal

Migrate the existing Cursor-only review-mode plugin into a single repository (`review-mode-plugin`) that works for both Claude Code and Cursor, distributed from GitHub via a Claude marketplace file.

## Source material

- Existing Cursor plugin: `review-mode/data/agents/cursor/review-mode/`
  - `.cursor-plugin/plugin.json` — Cursor manifest
  - `mcp.json` — MCP server config (Cursor format, with `mcpServers` wrapper)
  - `icon.png` — plugin logo
  - `rules/review-mode.mdc` — always-apply Cursor rule → becomes a command
  - `rules/update-plan.mdc` — Cursor rule → becomes a command
  - `skills/review-mode/SKILL.md` — shared skill (migrated as-is)

## Repository structure

```
review-mode-plugin/
├── .claude-plugin/
│   ├── plugin.json           # Claude Code plugin manifest
│   └── marketplace.json      # Claude Code marketplace catalog
├── .cursor-plugin/
│   └── plugin.json           # Cursor plugin manifest
├── skills/
│   └── review-mode/
│       └── SKILL.md          # Shared skill (migrated as-is)
├── commands/
│   ├── review-mode.md        # /review-mode command (from rules/review-mode.mdc)
│   └── update-plan.md        # /update-plan command (from rules/update-plan.mdc)
├── .mcp.json                 # Single MCP config (flat format, no wrapper)
├── icon.png                  # Plugin logo
├── LICENSE
└── README.md
```

## File specifications

### `.claude-plugin/plugin.json`

Standard Claude Code plugin manifest. Claude auto-discovers `skills/`, `commands/`, and `.mcp.json`.

```json
{
  "name": "review-mode",
  "description": "Review Mode — threaded annotations, revision tracking, and AI agent tools for Markdown files.",
  "version": "0.0.18",
  "author": {
    "name": "aurelio-amerio"
  },
  "repository": "https://github.com/aurelio-amerio/review-mode-plugin",
  "keywords": ["review", "annotations", "mcp", "markdown", "collaboration"]
}
```

### `.claude-plugin/marketplace.json`

Enables installation via `/plugin marketplace add https://github.com/aurelio-amerio/review-mode-plugin`. The plugin is the repo itself, so `source` is `"./"`.

```json
{
  "name": "review-mode-plugin",
  "owner": {
    "name": "aurelio-amerio"
  },
  "plugins": [
    {
      "name": "review-mode",
      "source": "./",
      "description": "Review Mode — threaded annotations, revision tracking, and AI agent tools for Markdown files."
    }
  ]
}
```

### `.cursor-plugin/plugin.json`

Cursor manifest referencing shared dirs. `mcpServers` points to the shared `.mcp.json` instead of embedding config.

```json
{
  "name": "review-mode",
  "displayName": "Review Mode",
  "description": "Review Mode — threaded annotations, revision tracking, and AI agent tools for Markdown files.",
  "version": "0.0.18",
  "author": {
    "name": "aurelio-amerio"
  },
  "repository": "https://github.com/aurelio-amerio/review-mode-plugin",
  "keywords": ["review", "annotations", "mcp", "markdown", "collaboration"],
  "logo": "./icon.png",
  "skills": "./skills/",
  "commands": "./commands/",
  "mcpServers": "./.mcp.json"
}
```

### `.mcp.json`

Single MCP config file used by both platforms. Claude auto-discovers it; Cursor reads it via the `mcpServers` reference in `.cursor-plugin/plugin.json`. Flat format (no `mcpServers` wrapper).

```json
{
  "review-mode": {
    "command": "review-mode-mcp",
    "args": [],
    "type": "stdio"
  }
}
```

### `skills/review-mode/SKILL.md`

Migrated from `cursor/review-mode/skills/review-mode/SKILL.md` with no content changes.

### `commands/review-mode.md`

Migrated from `rules/review-mode.mdc`. Rename to `.md` (Cursor supports `.md`, `.mdc`, `.markdown`). Replace existing frontmatter with:

```yaml
---
name: review-mode
description: Open a Markdown file in Review Mode for annotation-based feedback.
---
```

Content (workflow steps) unchanged.

### `commands/update-plan.md`

Migrated from `rules/update-plan.mdc`. Same frontmatter migration:

```yaml
---
name: update-plan
description: Fetch Review Mode annotations, implement changes, resolve comments, and re-open in Review Mode.
---
```

Content (workflow steps) unchanged.

## Installation

**Claude Code:**
```
/plugin marketplace add https://github.com/aurelio-amerio/review-mode-plugin
/plugin install review-mode@review-mode-plugin
```

Skills are invoked as `/review-mode:review-mode`. Commands as `/review-mode:review-mode` and `/review-mode:update-plan`.

**Cursor:**
```
/add-plugin https://github.com/aurelio-amerio/review-mode-plugin
```

## What is NOT in scope

- Changes to the `review-mode` VS Code extension
- Changes to the `review-mode-mcp` server
- Removing `data/agents/cursor/` from the main `review-mode` repo (separate cleanup task)
- Other agent formats (antigravity, cline) — they remain in `review-mode/data/agents/`
