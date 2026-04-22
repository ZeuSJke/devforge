# devforge

Unified development workflow across harnesses. One methodology, two installation paths.

Combines:

- **[superpowers](https://github.com/obra/superpowers)** — TDD, brainstorming, systematic debugging, code review.
- **[beads](https://github.com/steveyegge/beads)** — task tracking across sessions.
- **[Context7](https://github.com/upstash/context7)** — current library docs on demand.
- **[Playwright MCP](https://github.com/microsoft/playwright-mcp)** — browser-level proof that the UI actually works.

Mental model:

| Plugin | Role |
|--|--|
| beads | **WHAT** — tasks & order |
| superpowers | **HOW** — methodology |
| context7 | **WITH WHAT** — current APIs |
| playwright | **PROOF** — UI works, not just compiles |

## The flow

```
1. EPIC       → bd create -t epic "Goal"
2. BRAINSTORM → superpowers:brainstorming
3. PLAN       → superpowers:writing-plans (2–5 min subtasks)
4. SUBTASKS   → bd create + bd dep add
5. ISOLATE    → git worktree for non-trivial work
6. IMPLEMENT  → TDD RED → GREEN → REFACTOR → commit → bd close
7. VERIFY     → run proving command + ui-verification if UI changed
8. REVIEW     → superpowers:requesting-code-review
9. FINISH     → close the branch → bd close <epic-id>
```

**Playwright runs only on step 7** when UI code changed. Not during subtasks — a half-done branch cannot be browser-tested meaningfully.

## Layout

```
core/                         platform-agnostic source of truth
  workflow.md (dispatch table) · fresh-docs.md (Context7 gate) · ui-verification.md (Playwright gate)

harness/
  claude-code/                Claude Code plugin + installer
    .claude-plugin/ · CLAUDE.md · skills/ · commands/ · settings.example.json
  codex/                      Codex CLI — copy-paste install, no scripts
    AGENTS.md · references/ · config.example.toml · INSTALL.md

tools-map.md                  Claude Code ↔ Codex tool name mapping
```

## Install — Claude Code

```bash
claude plugin marketplace add github:ZeuSJke/devforge
claude plugin install devforge@devforge-marketplace
/install-workflow      # idempotent: guards in ~/.claude/CLAUDE.md + merges settings.json
/workflow-doctor       # check everything is wired up
```

Restart the session. Hooks run `bd prime` + a one-line reminder each SessionStart and before PreCompact.

## Install — Codex CLI

Two copy-paste steps, fully documented in [`harness/codex/INSTALL.md`](harness/codex/INSTALL.md):

1. Paste `harness/codex/AGENTS.md` into `~/.codex/instructions.md` (or a project `AGENTS.md`) between `<!-- devforge:begin --> / <!-- devforge:end -->` guards.
2. Paste `harness/codex/config.example.toml` into `~/.codex/config.toml` between the same guards.

That's it — no shell installer, no remote execution. Uninstall = delete the blocks.

## Project-level config

devforge is project-agnostic but needs to know two things per repo:

- Which commands run tests / build / dev-server.
- Which UI paths are critical enough to smoke-test with Playwright.

Declare these in `./.devforge/project.md` at the project root, or as a `## devforge project config` section in the project's `CLAUDE.md` / `AGENTS.md`. See [`core/ui-verification.md`](core/ui-verification.md) for the required shape. Without this file:

- `verification-before-completion` asks you for the test command before claiming done.
- `ui-verification` skips honestly with `ui-verification skipped: no project config`. It does **not** silently pass.

## Graceful degradation

| Missing | Fallback |
|--|--|
| beads | Use TodoWrite; flow still works. |
| superpowers | Plan-mode + AskUserQuestion for brainstorm. |
| Context7 MCP | WebFetch official docs, labelled `[may be stale]`. |
| Playwright MCP | `ui-verification` skipped honestly — epic cannot be closed with silent pass. |

## What we deliberately left out

- **413 specialist agent templates** (from template-bridge). Out of scope — pulls in another ecosystem and dilutes the discipline. This plugin is a methodology, not a catalog.
- **Gemini CLI harness.** Planned, not built.
- **`curl | bash` installers for Codex.** The plugin is markdown + TOML. You should see exactly what you paste into your config. A shell runner for this would be overkill and a supply-chain risk for zero benefit.

## Credits & inspiration

Architecture inspired by [template-bridge](https://github.com/maslennikov-ig/template-bridge) (three-layer protection pattern). Methodology from [superpowers](https://github.com/obra/superpowers). Task model from [beads](https://github.com/steveyegge/beads).

## License

MIT.
