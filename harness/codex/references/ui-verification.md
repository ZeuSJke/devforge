# UI verification — Playwright gate

## Scope

Runs **only** at step 7 of the workflow, **only** when UI code changed, **only** after every subtask in the epic is closed.

Playwright is **not** part of the TDD loop in step 6. Component-level behavior is covered by unit/component tests (React Testing Library, Vue Test Utils, etc.) with mocks. Running Playwright mid-branch — before the dev server can be started with a coherent integration — produces false reds and proves nothing.

## When it triggers

Any of:
- files under `**/components/**`, `**/pages/**`, `**/app/**`, `**/views/**`
- files matching `*.tsx`, `*.jsx`, `*.vue`, `*.svelte`, `*.astro`
- style files touched: `*.css`, `*.scss`, `tailwind.config.*`
- routing config touched

## Checklist

1. **Build**: run the build command from the project config. Must succeed.
2. **Dev server**: start it in background from the project config. Wait until the URL responds.
3. **For each critical UI path** declared in the project config:
   - `browser_navigate` to the path.
   - `browser_snapshot` — capture DOM structure.
   - Perform **one** happy-path interaction that exercises the feature (click, form fill, etc.).
   - `browser_console_messages` — assert no messages with severity `error`.
   - `browser_network_requests` — assert no 4xx/5xx on critical endpoints.
4. **Shut down** the dev server.

If any step fails, the feature is not done. Return to step 6.

## Tool names

Claude Code: `mcp__plugin_playwright_playwright__browser_navigate`, `..._snapshot`, `..._click`, `..._console_messages`, `..._network_requests`.
Codex: the same MCP tools, prefixed per `~/.codex/config.toml` entry name.

See `tools-map.md` for the full mapping.

## Project config dependency

`ui-verification` needs two pieces of information from the project:
- `dev server`: command + URL
- `critical UI paths`: list of routes to smoke-test

Both live in the project-level config file (see harness-specific INSTALL docs for the exact location and format). If the config is missing, this gate is **skipped honestly** with the message `ui-verification skipped: no project config found`. It is not marked as passing.

## Fallback if Playwright MCP unavailable

Print `ui-verification skipped: playwright MCP not available; install the @playwright plugin or add it to ~/.codex/config.toml`. Do not claim the UI is verified. Do not silently succeed.
