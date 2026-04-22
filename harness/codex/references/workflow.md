# devforge workflow — dispatch table

This plugin does NOT restate the superpowers methodology. Every step below **invokes** the corresponding superpowers skill. The skill owns the discipline. devforge only adds three things superpowers doesn't cover: beads bookkeeping, Context7 gating, and a Playwright UI-proof gate.

## What devforge adds, and only this

| Add-on | Skill | Covers |
|--|--|--|
| beads | — (CLI calls + `bd prime` hooks) | WHAT: task creation, claim, close, dependency edges |
| Context7 | `devforge:fresh-docs` | WITH WHAT: current library docs before any external-library code |
| Playwright | `devforge:ui-verification` | PROOF: browser-level smoke check of critical UI paths at step 7 |

Anything else — TDD, debugging, planning, review, worktrees, parallel agents, verification — lives inside superpowers. Invoke the skill. Do not paraphrase it.

## The flow

| # | Step | Invoke | devforge bookkeeping |
|--|--|--|--|
| 1 | Create epic | — | `bd create -t epic "<goal>"` |
| 2 | Brainstorm | `superpowers:brainstorming` | — |
| 3 | Plan | `superpowers:writing-plans` | — |
| 4 | Decompose | — | `bd create` per subtask + `bd dep add` (parent-child / blocks) |
| 5 | Isolate | `superpowers:using-git-worktrees` | — |
| 6 | Implement (per subtask) | `superpowers:test-driven-development` | `bd update <id> --claim` → TDD cycle → commit → `bd close <id>`; invoke `devforge:fresh-docs` before any external library use |
| 6b | If anything breaks | `superpowers:systematic-debugging` | — |
| 6c | Independent parallel work | `superpowers:dispatching-parallel-agents` / `subagent-driven-development` | — |
| 7 | Verify | `superpowers:verification-before-completion` | + `devforge:ui-verification` if UI code was touched |
| 8 | Review | `superpowers:requesting-code-review` → on reply, `superpowers:receiving-code-review` | — |
| 9 | Finish branch | `superpowers:finishing-a-development-branch` | `bd close <epic-id>` |

## Beads dependency types (bookkeeping reference)

Only `blocks` affects `bd ready`:

| Type | Affects ready? | Use for |
|--|--|--|
| blocks | yes | Sequential/technical prerequisite |
| parent-child | no | Epic → subtask |
| related | no | Connected but independent |
| discovered-from | no | Side quest found during implementation |

Direction: "X needs Y" → `bd dep add X Y`. Side quests: `bd create -t bug` + `bd dep add new current --type discovered-from`.

## Graceful degradation

| Missing plugin | Fallback |
|--|--|
| beads | TodoWrite; suggest install. Flow continues. |
| superpowers | We refuse to reinterpret the methodology — install superpowers. This plugin is an orchestrator, not a replacement. |
| Context7 MCP | `devforge:fresh-docs` falls back to WebFetch with a `[may be stale]` label. |
| Playwright MCP | `devforge:ui-verification` reports `skipped: playwright MCP unavailable`. Never silently passes. |
