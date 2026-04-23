# devforge

Claude Code plugin that orchestrates [superpowers](https://github.com/obra/superpowers) skills as a 9-step development flow and adds two things superpowers does not cover:

- **beads** bookkeeping (`bd` CLI) — task creation, claim, close, dependencies.
- **Playwright** gate (`devforge:ui-verification`) — browser-level smoke check on step 7 when UI changed.

Library-docs lookup (Context7) is not in this list on purpose: when the Context7 MCP server is installed, it injects its own usage instructions into the session. devforge relies on that and does not duplicate it.

devforge does not restate superpowers methodology, beads dependency semantics, or Context7 rules. It only sequences their skills and adds the Playwright gate.

## Flow

| # | Step | Invoke |
|--|--|--|
| 1 | Create epic | `bd create -t epic "<goal>"` |
| 2 | Brainstorm | `superpowers:brainstorming` |
| 3 | Plan | `superpowers:writing-plans` |
| 4 | Decompose | `bd create` per subtask + `bd dep add` |
| 5 | Isolate | `superpowers:using-git-worktrees` |
| 6 | Implement (per subtask) | `superpowers:test-driven-development`; Context7's own injection handles library docs |
| 6b | On failure | `superpowers:systematic-debugging` |
| 7 | Verify | `superpowers:verification-before-completion` + `devforge:ui-verification` if UI changed |
| 8 | Review | `superpowers:requesting-code-review` / `receiving-code-review` |
| 9 | Finish | `superpowers:finishing-a-development-branch`; then `bd close <epic-id>` |

## Layout

```
.claude-plugin/     plugin.json, marketplace.json
CLAUDE.md           appended to ~/.claude/CLAUDE.md by /install-workflow
skills/             unified-workflow, ui-verification
commands/           install-workflow, workflow-doctor, ui-smoke
settings.example.json   SessionStart + PreCompact hooks (project-gated)
```

## Install

```bash
claude plugin marketplace add ZeuSJke/devforge
claude plugin install devforge@devforge-marketplace
/install-workflow      # idempotent: guards in ~/.claude/CLAUDE.md, merges settings.json
/workflow-doctor       # check beads, superpowers, Context7 MCP, Playwright MCP
```

Restart the session after `/install-workflow` for hooks to load.

## Prerequisites

- [superpowers](https://github.com/obra/superpowers) plugin — methodology is invoked from here.
- [beads](https://github.com/steveyegge/beads) plugin — provides the `bd` CLI and dependency semantics.
- Context7 MCP server — library-docs lookup. Context7 injects its own usage instructions when installed; devforge relies on that and does not duplicate it.
- Playwright MCP server — required for `devforge:ui-verification`.

`/workflow-doctor` reports missing pieces with remediation steps.

## Scope — per-project opt-in

devforge installs globally but activates only in projects containing `.devforge/project.md`.

- Hooks wrap commands in `test -f .devforge/project.md && … || true` — silent no-op elsewhere.
- CLAUDE.md block and `unified-workflow` skill open with "applies only when `.devforge/project.md` exists".

Enable in a project: create `.devforge/project.md`.
Disable temporarily: rename to `.devforge/project.md.off`.
Remove entirely: delete the blocks between `<!-- devforge:begin --> … <!-- devforge:end -->` in `~/.claude/CLAUDE.md` and `~/.claude/settings.json`.

## Project config format

`.devforge/project.md`:

```md
## Test commands
- unit: pnpm test
- build: pnpm build
- lint: pnpm lint

## Dev server
- command: pnpm dev
- url: http://localhost:3000

## Critical UI paths
- /login     — login happy path
- /checkout  — one-item checkout
- /dashboard — widgets render
```

Without this file: `verification-before-completion` asks for the test command; `ui-verification` skips honestly (never silently passes).

## Graceful degradation

| Missing | Behavior |
|--|--|
| beads | TodoWrite fallback; flow continues. |
| superpowers | devforge refuses to paraphrase — install superpowers. |
| Context7 MCP | Flagged by `/workflow-doctor`; Claude falls back to WebFetch / training data. |
| Playwright MCP | `ui-verification` reports `skipped: playwright MCP unavailable`. |

## License

MIT.
