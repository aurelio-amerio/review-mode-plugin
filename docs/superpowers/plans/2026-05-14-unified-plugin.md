# Unified review-mode Plugin Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Migrate the existing Cursor-only review-mode plugin into the `review-mode-plugin` repo so it works for both Claude Code and Cursor, distributable from GitHub.

**Architecture:** Single repo with shared `skills/` and `commands/` directories. Both platforms read the same files. Claude uses `.claude-plugin/plugin.json` (auto-discovers content) plus a `marketplace.json` for GitHub-based installation. Cursor uses `.cursor-plugin/plugin.json` with explicit dir references and `"mcpServers": "./.mcp.json"`. One `.mcp.json` serves both.

**Tech Stack:** JSON (manifests), Markdown (skills/commands), YAML frontmatter

**Spec:** `docs/superpowers/specs/2026-05-14-unified-plugin-design.md`

**Source files (read-only reference):**
- `../review-mode/data/agents/cursor/review-mode/` — existing Cursor plugin

---

## File Map

| File | Action | Purpose |
|------|--------|---------|
| `.claude-plugin/plugin.json` | Create | Claude Code plugin manifest |
| `.claude-plugin/marketplace.json` | Create | Claude marketplace catalog for GitHub install |
| `.cursor-plugin/plugin.json` | Create | Cursor plugin manifest with dir refs + MCP pointer |
| `skills/review-mode/SKILL.md` | Create | Shared skill (migrated from cursor plugin) |
| `commands/review-mode.md` | Create | /review-mode command (migrated from rules/review-mode.mdc) |
| `commands/update-plan.md` | Create | /update-plan command (migrated from rules/update-plan.mdc) |
| `.mcp.json` | Create | Single MCP config, flat format (no wrapper) |
| `icon.png` | Create | Plugin logo (copy from cursor plugin) |
| `README.md` | Modify | Update with installation instructions |

---

## Task 1: Claude Code plugin manifest

**Files:**
- Create: `.claude-plugin/plugin.json`

- [ ] **Step 1: Create the `.claude-plugin` directory and `plugin.json`**

  ```bash
  mkdir -p /home/aure/github/review-mode-plugin/.claude-plugin
  ```

  Create `.claude-plugin/plugin.json`:

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

- [ ] **Step 2: Verify the file is valid JSON**

  ```bash
  cd /home/aure/github/review-mode-plugin && cat .claude-plugin/plugin.json | python3 -m json.tool
  ```

  Expected: pretty-printed JSON with no errors.

- [ ] **Step 3: Commit**

  ```bash
  cd /home/aure/github/review-mode-plugin
  git add .claude-plugin/plugin.json
  git commit -m "feat: add Claude Code plugin manifest"
  ```

---

## Task 2: Claude marketplace file

**Files:**
- Create: `.claude-plugin/marketplace.json`

- [ ] **Step 1: Create `marketplace.json`**

  Create `.claude-plugin/marketplace.json`:

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

- [ ] **Step 2: Verify valid JSON**

  ```bash
  cd /home/aure/github/review-mode-plugin && cat .claude-plugin/marketplace.json | python3 -m json.tool
  ```

  Expected: pretty-printed JSON with no errors.

- [ ] **Step 3: Commit**

  ```bash
  cd /home/aure/github/review-mode-plugin
  git add .claude-plugin/marketplace.json
  git commit -m "feat: add Claude marketplace file"
  ```

---

## Task 3: Cursor plugin manifest

**Files:**
- Create: `.cursor-plugin/plugin.json`

- [ ] **Step 1: Create the `.cursor-plugin` directory and `plugin.json`**

  ```bash
  mkdir -p /home/aure/github/review-mode-plugin/.cursor-plugin
  ```

  Create `.cursor-plugin/plugin.json`:

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

- [ ] **Step 2: Verify valid JSON**

  ```bash
  cd /home/aure/github/review-mode-plugin && cat .cursor-plugin/plugin.json | python3 -m json.tool
  ```

  Expected: pretty-printed JSON with no errors.

- [ ] **Step 3: Commit**

  ```bash
  cd /home/aure/github/review-mode-plugin
  git add .cursor-plugin/plugin.json
  git commit -m "feat: add Cursor plugin manifest"
  ```

---

## Task 4: MCP configuration

**Files:**
- Create: `.mcp.json`

- [ ] **Step 1: Create `.mcp.json`**

  The existing `cursor/mcp.json` uses a `mcpServers` wrapper. The new `.mcp.json` uses the flat Claude format (no wrapper). Cursor reads it via the `mcpServers` reference in `.cursor-plugin/plugin.json`.

  Create `.mcp.json`:

  ```json
  {
    "review-mode": {
      "command": "review-mode-mcp",
      "args": [],
      "type": "stdio"
    }
  }
  ```

- [ ] **Step 2: Verify valid JSON**

  ```bash
  cd /home/aure/github/review-mode-plugin && cat .mcp.json | python3 -m json.tool
  ```

  Expected: pretty-printed JSON with no errors.

- [ ] **Step 3: Commit**

  ```bash
  cd /home/aure/github/review-mode-plugin
  git add .mcp.json
  git commit -m "feat: add MCP server configuration"
  ```

---

## Task 5: Shared skill

**Files:**
- Create: `skills/review-mode/SKILL.md`

Source: `../review-mode/data/agents/cursor/review-mode/skills/review-mode/SKILL.md`

- [ ] **Step 1: Create `skills/` directory and copy SKILL.md**

  ```bash
  mkdir -p /home/aure/github/review-mode-plugin/skills/review-mode
  cp /home/aure/github/review-mode/data/agents/cursor/review-mode/skills/review-mode/SKILL.md \
     /home/aure/github/review-mode-plugin/skills/review-mode/SKILL.md
  ```

- [ ] **Step 2: Verify the file copied correctly and frontmatter is intact**

  ```bash
  head -10 /home/aure/github/review-mode-plugin/skills/review-mode/SKILL.md
  ```

  Expected output (first 8 lines):
  ```
  ---
  name: review-mode
  description: >
    Use when the user asks to open a Markdown file in Review Mode inside VS Code,
    or when iterating on a plan/document based on Review Mode annotations.
    Review Mode renders the file in a read-only WebView panel with annotation
    support. This does NOT open a web browser.
  ---
  ```

- [ ] **Step 3: Commit**

  ```bash
  cd /home/aure/github/review-mode-plugin
  git add skills/review-mode/SKILL.md
  git commit -m "feat: add shared review-mode skill"
  ```

---

## Task 6: Commands — review-mode

**Files:**
- Create: `commands/review-mode.md`

Source: `../review-mode/data/agents/cursor/review-mode/rules/review-mode.mdc`

- [ ] **Step 1: Create `commands/` directory**

  ```bash
  mkdir -p /home/aure/github/review-mode-plugin/commands
  ```

- [ ] **Step 2: Copy source file and rename**

  ```bash
  cp /home/aure/github/review-mode/data/agents/cursor/review-mode/rules/review-mode.mdc \
     /home/aure/github/review-mode-plugin/commands/review-mode.md
  ```

- [ ] **Step 3: Replace frontmatter**

  The source file has:
  ```yaml
  ---
  name: review-mode
  description: Open a Markdown file in Review Mode for annotation-based feedback.
  alwaysApply: true
  ---
  ```

  Replace with (remove `alwaysApply`, description stays):
  ```yaml
  ---
  name: review-mode
  description: Open a Markdown file in Review Mode for annotation-based feedback.
  ---
  ```

  Edit `commands/review-mode.md` to remove the `alwaysApply: true` line. Body content (everything after the closing `---`) is unchanged.

- [ ] **Step 4: Verify frontmatter has no `alwaysApply` and body is intact**

  ```bash
  head -6 /home/aure/github/review-mode-plugin/commands/review-mode.md
  ```

  Expected:
  ```
  ---
  name: review-mode
  description: Open a Markdown file in Review Mode for annotation-based feedback.
  ---

  # /review-mode
  ```

- [ ] **Step 5: Commit**

  ```bash
  cd /home/aure/github/review-mode-plugin
  git add commands/review-mode.md
  git commit -m "feat: add review-mode command"
  ```

---

## Task 7: Commands — update-plan

**Files:**
- Create: `commands/update-plan.md`

Source: `../review-mode/data/agents/cursor/review-mode/rules/update-plan.mdc`

- [ ] **Step 1: Copy source file and rename**

  ```bash
  cp /home/aure/github/review-mode/data/agents/cursor/review-mode/rules/update-plan.mdc \
     /home/aure/github/review-mode-plugin/commands/update-plan.md
  ```

- [ ] **Step 2: Replace frontmatter**

  The source file has:
  ```yaml
  ---
  name: udpate-plan
  description: Fetch Review Mode annotations, implement changes, resolve comments, and re-open in Review Mode, /update-plan
  alwaysApply: false
  ---
  ```

  Replace with (fix typo in name, clean up description, remove `alwaysApply`):
  ```yaml
  ---
  name: update-plan
  description: Fetch Review Mode annotations, implement changes, resolve comments, and re-open in Review Mode.
  ---
  ```

  Edit `commands/update-plan.md` to use this new frontmatter. Body content is unchanged.

- [ ] **Step 3: Verify frontmatter is correct**

  ```bash
  head -6 /home/aure/github/review-mode-plugin/commands/update-plan.md
  ```

  Expected:
  ```
  ---
  name: update-plan
  description: Fetch Review Mode annotations, implement changes, resolve comments, and re-open in Review Mode.
  ---

  # /update-plan
  ```

- [ ] **Step 4: Commit**

  ```bash
  cd /home/aure/github/review-mode-plugin
  git add commands/update-plan.md
  git commit -m "feat: add update-plan command"
  ```

---

## Task 8: Plugin icon

**Files:**
- Create: `icon.png`

- [ ] **Step 1: Copy icon from existing Cursor plugin**

  ```bash
  cp /home/aure/github/review-mode/data/agents/cursor/review-mode/icon.png \
     /home/aure/github/review-mode-plugin/icon.png
  ```

- [ ] **Step 2: Verify the file exists and is non-empty**

  ```bash
  ls -lh /home/aure/github/review-mode-plugin/icon.png
  ```

  Expected: file listed with non-zero size.

- [ ] **Step 3: Commit**

  ```bash
  cd /home/aure/github/review-mode-plugin
  git add icon.png
  git commit -m "feat: add plugin icon"
  ```

---

## Task 9: Update README

**Files:**
- Modify: `README.md`

- [ ] **Step 1: Rewrite README with installation and usage instructions**

  Replace the contents of `README.md` with:

  ````markdown
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
  ````

- [ ] **Step 2: Commit**

  ```bash
  cd /home/aure/github/review-mode-plugin
  git add README.md
  git commit -m "docs: update README with installation and usage instructions"
  ```

---

## Task 10: Smoke-test the plugin structure

- [ ] **Step 1: Verify the full directory tree looks correct**

  ```bash
  find /home/aure/github/review-mode-plugin -not -path '*/.git/*' -not -path '*/docs/*' | sort
  ```

  Expected output:
  ```
  /home/aure/github/review-mode-plugin
  /home/aure/github/review-mode-plugin/.claude-plugin
  /home/aure/github/review-mode-plugin/.claude-plugin/marketplace.json
  /home/aure/github/review-mode-plugin/.claude-plugin/plugin.json
  /home/aure/github/review-mode-plugin/.cursor-plugin
  /home/aure/github/review-mode-plugin/.cursor-plugin/plugin.json
  /home/aure/github/review-mode-plugin/.mcp.json
  /home/aure/github/review-mode-plugin/LICENSE
  /home/aure/github/review-mode-plugin/README.md
  /home/aure/github/review-mode-plugin/commands
  /home/aure/github/review-mode-plugin/commands/review-mode.md
  /home/aure/github/review-mode-plugin/commands/update-plan.md
  /home/aure/github/review-mode-plugin/icon.png
  /home/aure/github/review-mode-plugin/skills
  /home/aure/github/review-mode-plugin/skills/review-mode
  /home/aure/github/review-mode-plugin/skills/review-mode/SKILL.md
  ```

- [ ] **Step 2: Validate all JSON files**

  ```bash
  cd /home/aure/github/review-mode-plugin
  for f in .claude-plugin/plugin.json .claude-plugin/marketplace.json .cursor-plugin/plugin.json .mcp.json; do
    echo -n "$f: "
    python3 -m json.tool "$f" > /dev/null && echo "OK" || echo "INVALID"
  done
  ```

  Expected: all four print `OK`.

- [ ] **Step 3: Verify all Markdown command files have correct frontmatter (no `alwaysApply`, correct `name`)**

  ```bash
  head -5 /home/aure/github/review-mode-plugin/commands/review-mode.md
  echo "---"
  head -5 /home/aure/github/review-mode-plugin/commands/update-plan.md
  ```

  Expected for `review-mode.md`:
  ```
  ---
  name: review-mode
  description: Open a Markdown file in Review Mode for annotation-based feedback.
  ---
  ```

  Expected for `update-plan.md`:
  ```
  ---
  name: update-plan
  description: Fetch Review Mode annotations, implement changes, resolve comments, and re-open in Review Mode.
  ---
  ```
