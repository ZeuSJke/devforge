# Tool name mapping — Claude Code ↔ Codex CLI

Source of truth for cross-harness tool names. Every `core/*.md` refers here instead of hard-coding names.

## File and shell ops

| Action | Claude Code | Codex CLI |
|--|--|--|
| Read file | `Read` | `shell` → `cat` (or file-read primitive) |
| Write file | `Write` | `apply_patch` (create) |
| Edit file | `Edit` | `apply_patch` (update) |
| Shell exec | `Bash` | `shell` |
| Search content | `Grep` | `shell` → `rg` |
| Find files | `Glob` | `shell` → `fd` / `find` |

## MCP — Playwright

Assumes the Playwright MCP is installed under a name Claude Code surfaces as `plugin_playwright_playwright`. In Codex, the prefix is whatever key you chose under `[mcp_servers.<name>]` — below we assume `playwright`.

| Function | Claude Code | Codex CLI |
|--|--|--|
| Navigate | `mcp__plugin_playwright_playwright__browser_navigate` | `playwright.browser_navigate` |
| Snapshot DOM | `mcp__plugin_playwright_playwright__browser_snapshot` | `playwright.browser_snapshot` |
| Click | `mcp__plugin_playwright_playwright__browser_click` | `playwright.browser_click` |
| Type | `mcp__plugin_playwright_playwright__browser_type` | `playwright.browser_type` |
| Console | `mcp__plugin_playwright_playwright__browser_console_messages` | `playwright.browser_console_messages` |
| Network | `mcp__plugin_playwright_playwright__browser_network_requests` | `playwright.browser_network_requests` |
| Screenshot | `mcp__plugin_playwright_playwright__browser_take_screenshot` | `playwright.browser_take_screenshot` |

## MCP — Context7

Assumes `[mcp_servers.context7]` in Codex.

| Function | Claude Code | Codex CLI |
|--|--|--|
| Resolve library ID | `mcp__plugin_context7_context7__resolve-library-id` | `context7.resolve-library-id` |
| Query docs | `mcp__plugin_context7_context7__query-docs` | `context7.query-docs` |

## Beads

Beads is a CLI (`bd`), identical on both harnesses — no wrapper.

## Finding the actual MCP name

If the prefix differs on your machine:
- **Claude Code**: `/mcp` lists active MCP tools; search for `playwright` or `context7`.
- **Codex CLI**: `cat ~/.codex/config.toml` — the section name under `[mcp_servers.<name>]` is the prefix.
