# devforge

Claude Code plugin that glues [superpowers](https://github.com/obra/superpowers) and [beads](https://github.com/steveyegge/beads) into a single 9-step development flow.

It does not restate either plugin's content. It owns two things:

- The **sequence** — which superpowers skill to invoke at which step, interleaved with the right `bd` commands.
- The **hooks** — `bd prime` on SessionStart and PreCompact so beads state survives session boundaries.

Library-docs lookup (Context7) and any other MCP instructions are expected to come from those MCPs themselves — devforge does not duplicate them.

## Flow

| # | Step | Invoke |
|--|--|--|
| 1 | Create epic | `bd create -t epic "<goal>"` |
| 2 | Brainstorm | `superpowers:brainstorming` |
| 3 | Plan | `superpowers:writing-plans` |
| 4 | Decompose | `bd create` per subtask + `bd dep add` |
| 5 | Isolate | `superpowers:using-git-worktrees` |
| 6 | Implement (per subtask) | `superpowers:test-driven-development` |
| 6b | On failure | `superpowers:systematic-debugging` |
| 7 | Verify | `superpowers:verification-before-completion` |
| 8 | Review | `superpowers:requesting-code-review` / `receiving-code-review` |
| 9 | Finish | `superpowers:finishing-a-development-branch`; then `bd close <epic-id>` |

## Layout

```
.claude-plugin/          plugin.json, marketplace.json (local marketplace)
CLAUDE.md                appended to ~/.claude/CLAUDE.md by /install-workflow
skills/unified-workflow/ the single dispatch-table skill
commands/                install-workflow, workflow-doctor
settings.example.json    SessionStart + PreCompact hooks (project-gated)
```

## Install (local, no public marketplace yet)

```bash
git clone https://github.com/ZeuSJke/devforge.git ~/devforge
claude plugin marketplace add ~/devforge
claude plugin install devforge@devforge-marketplace
/install-workflow      # idempotent: guards in ~/.claude/CLAUDE.md, merges settings.json
/workflow-doctor       # check beads + superpowers
```

Restart the session after `/install-workflow` for hooks to load.

## Prerequisites

- [superpowers](https://github.com/obra/superpowers) plugin — methodology is invoked from here.
- [beads](https://github.com/steveyegge/beads) plugin — provides the `bd` CLI and dependency semantics.

Context7 MCP is optional — when installed, it self-instructs Claude to pull fresh library docs. devforge does not configure or duplicate it.

## Scope — per-project opt-in

devforge installs globally but activates only in projects containing `.devforge/project.md`.

- Hooks wrap commands in `test -f .devforge/project.md && … || true` — silent no-op elsewhere.
- CLAUDE.md block and `unified-workflow` skill open with "applies only when `.devforge/project.md` exists".

Enable in a project: create `.devforge/project.md`. The file's existence is the opt-in signal; its content is free-form (a short note explaining why this project uses devforge is enough).

Disable temporarily: rename to `.devforge/project.md.off`.
Remove entirely: delete the blocks between `<!-- devforge:begin --> … <!-- devforge:end -->` in `~/.claude/CLAUDE.md` and `~/.claude/settings.json`.

## Graceful degradation

| Missing | Behavior |
|--|--|
| beads | TodoWrite fallback; flow continues. |
| superpowers | devforge refuses to paraphrase — install superpowers. |
| Context7 MCP | Claude falls back to WebFetch / training data; devforge does not interfere. |

## License

MIT.
