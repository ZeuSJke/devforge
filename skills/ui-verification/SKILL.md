---
name: ui-verification
description: >
  Playwright smoke-check of critical UI paths. TRIGGER: step 7 of the devforge flow,
  when UI code changed (components, pages, styles, routes) and you are about to claim
  done. NOT for use during subtasks — mid-branch Playwright runs on half-done
  integration produce false reds. Subtask-level UI belongs in unit / component tests
  inside superpowers:test-driven-development.
---

# devforge:ui-verification

devforge gate: superpowers `verification-before-completion` covers "quote the proving command". It does NOT know about browsers. This skill is the browser-level proof.

## When to run

- Step 7 of the flow.
- UI code changed in this branch.
- All subtasks of the epic are closed and the branch builds.

## Triggers (UI code changed)

- `**/components/**`, `**/pages/**`, `**/app/**`, `**/views/**`
- `*.tsx`, `*.jsx`, `*.vue`, `*.svelte`, `*.astro`
- `*.css`, `*.scss`, `tailwind.config.*`
- Routing config

## Checklist

1. Build with the project's build command → exit 0.
2. Start dev server in background; poll URL until it responds.
3. For each critical UI path in the project config, use the Playwright MCP (any prefix — `mcp__plugin_playwright_playwright__…`, `mcp__playwright__…`, etc. — Playwright injects its own tool descriptions):
   - `browser_navigate` → path
   - `browser_snapshot`
   - One happy-path interaction (`browser_click` / `browser_type`)
   - `browser_console_messages` — no `error`-level entries
   - `browser_network_requests` — no 4xx/5xx on critical endpoints
4. Stop the dev server.

Any red step → feature not done → return to step 6.

## Project config required

Read `.devforge/project.md` or the project's `CLAUDE.md` / `AGENTS.md` section. It must declare: dev-server command + URL, and the list of critical UI paths. Missing config → print `ui-verification skipped: no project config` and refuse to mark the epic done.

## Fallback if Playwright MCP is missing

Print `ui-verification skipped: playwright MCP not installed`. Never claim UI verified.
