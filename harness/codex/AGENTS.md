# devforge — unified dev workflow (Codex edition)

**Opt-in per project.** This flow applies only in projects that contain a `.devforge/project.md` file at their root. In any other project, ignore this file and follow that project's own methodology.

This file is a dispatch table, not a methodology. Each step invokes the corresponding superpowers skill — superpowers owns the discipline. devforge adds only three things: beads bookkeeping, a Context7 gate, and a Playwright UI gate.

## Flow

| # | Step | Invoke |
|--|--|--|
| 1 | Create epic | `bd create -t epic "<goal>"` |
| 2 | Brainstorm | superpowers brainstorming skill |
| 3 | Plan | superpowers writing-plans skill |
| 4 | Decompose | `bd create` per subtask + `bd dep add` |
| 5 | Isolate | superpowers using-git-worktrees skill |
| 6 | Implement (per subtask) | superpowers test-driven-development skill. Before any external library use: read `references/fresh-docs.md`, then call Context7 MCP. `bd update <id> --claim` → TDD cycle → `bd close`. |
| 6b | On failure | superpowers systematic-debugging skill |
| 6c | Parallel independent work | superpowers dispatching-parallel-agents / subagent-driven-development |
| 7 | Verify | superpowers verification-before-completion skill. If UI changed: follow `references/ui-verification.md`. |
| 8 | Review | superpowers requesting-code-review / receiving-code-review |
| 9 | Finish | superpowers finishing-a-development-branch; then `bd close <epic-id>` |

## Hard rules (enforced by the invoked superpowers skills; listed here only so the sequence is unambiguous)

- A beads task exists for every piece of work — this is devforge bookkeeping, the rest is superpowers.
- Context7 is queried before external-library code — see `references/fresh-docs.md`.
- Playwright gate runs on step 7 if UI changed — see `references/ui-verification.md`.

## When to read which reference

| Situation | Read |
|--|--|
| Starting any task | `references/workflow.md` |
| About to import a library | `references/fresh-docs.md` |
| UI code changed, step 7 | `references/ui-verification.md` |
| Unsure of tool name in Codex | `../../tools-map.md` |

Do not inline these. Codex loads AGENTS.md every turn — keep it small.

## Project config

If the project has `.devforge/project.md`, read it — it declares build/test/dev-server commands and critical UI paths. Without it: ask the user for the test command before claiming done; `ui-verification` is skipped honestly (never silently succeed).

## If superpowers is not installed

This plugin is an orchestrator, not a methodology. Install superpowers. devforge does not paraphrase its skills.
