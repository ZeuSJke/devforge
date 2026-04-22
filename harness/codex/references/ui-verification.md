# ui-verification — Playwright gate

Superpowers `verification-before-completion` covers the general "quote the proving command" rule. It does NOT know about browsers. This skill is devforge's browser-level proof for user-facing changes.

## Scope

Runs **only** at step 7 of the workflow, **only** when UI code changed, **only** after every subtask in the epic is closed and the branch builds.

Running Playwright during subtasks (mid-branch) is misuse — integration is half-done, the dev server cannot be started coherently, and the result is false-red. Subtask-level UI behavior belongs in unit / component tests (part of `superpowers:test-driven-development`).

## Triggers (UI code changed)

- `**/components/**`, `**/pages/**`, `**/app/**`, `**/views/**`
- `*.tsx`, `*.jsx`, `*.vue`, `*.svelte`, `*.astro`
- `*.css`, `*.scss`, `tailwind.config.*`
- Routing config

## Checklist

1. **Build** — run the project's build command (from project config). Must exit 0.
2. **Dev server** — start in background (project config). Poll the URL until it responds.
3. **Per critical UI path** in project config:
   - `browser_navigate` → path
   - `browser_snapshot` — DOM snapshot
   - One happy-path interaction exercising the feature (`browser_click` / `browser_type`)
   - `browser_console_messages` — assert no `error`-level messages
   - `browser_network_requests` — assert no 4xx/5xx on critical endpoints
4. **Stop** the dev server.

Any red step → the feature is not done → back to step 6.

## Project config dependency

This skill needs two declarations from the project-level config (`.devforge/project.md` or a section in the project's `CLAUDE.md` / `AGENTS.md`):

- `dev server`: command + URL
- `critical UI paths`: list of routes + short description

Without them: print `ui-verification skipped: no project config` and refuse to mark the epic done. Ask the user to either create the config or acknowledge the skip explicitly.

## Fallback if Playwright MCP is unavailable

Print `ui-verification skipped: playwright MCP not installed`. Never claim UI verified. Never silently pass.

## Tool names

See `tools-map.md` for the harness-specific MCP prefix.
