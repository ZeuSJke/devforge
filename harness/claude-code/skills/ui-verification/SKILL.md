---
name: ui-verification
description: >
  Playwright smoke-check of critical UI paths. TRIGGER: step 7 of unified-workflow
  when UI code changed (components, pages, styles, routes) and about to claim done.
  NOT for use during subtasks — the branch must be fully green first, otherwise
  Playwright runs against half-done integration and produces false reds.
---

# devforge:ui-verification

## When this runs

- **Only** at step 7 VERIFY of the workflow.
- **Only** when this branch touched UI code (see triggers below).
- **Only** after every subtask in the epic is closed and the branch builds.

Running this skill earlier is a misuse — unit/component tests cover subtask-level behavior.

## Triggers (UI code changed)

- `**/components/**`, `**/pages/**`, `**/app/**`, `**/views/**`
- `*.tsx`, `*.jsx`, `*.vue`, `*.svelte`, `*.astro`
- `*.css`, `*.scss`, `tailwind.config.*`
- Routing config

## Checklist

1. **Build** — run the project-config build command. Must exit 0.
2. **Dev server** — start it in background per project config. Wait until the URL responds (poll).
3. **For each critical UI path** in project config:
   - `mcp__plugin_playwright_playwright__browser_navigate` → path.
   - `mcp__plugin_playwright_playwright__browser_snapshot` — DOM snapshot.
   - One happy-path interaction exercising the feature (`browser_click` / `browser_type`).
   - `mcp__plugin_playwright_playwright__browser_console_messages` — assert no `error`-level messages.
   - `mcp__plugin_playwright_playwright__browser_network_requests` — assert no 4xx/5xx.
4. **Stop** the background dev server.

Any red step → feature is not done → back to step 6.

## Project config required

This skill needs two things from the project-level config file (`.devforge/project.md` or a dedicated section in project `CLAUDE.md`):
- `dev server`: command + URL
- `critical UI paths`: list of routes + short description

If the config is missing:
- Print `ui-verification skipped: no project config (.devforge/project.md). UI change is NOT verified.`
- Do NOT mark the epic as done.
- Ask the user whether to create the project config now, or to skip explicitly with their acknowledgement.

## Fallback if Playwright MCP unavailable

Print `ui-verification skipped: playwright MCP not installed. Install @playwright plugin (Claude Code) or add [mcp_servers.playwright] (Codex).` Do NOT claim UI verified.
