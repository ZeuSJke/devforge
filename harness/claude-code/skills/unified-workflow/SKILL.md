---
name: unified-workflow
description: >
  Dispatch table for the devforge flow. TRIGGER: at session start, before any task, or
  whenever the next step is unclear. Maps each step to the superpowers skill that owns
  the discipline for that step, plus beads bookkeeping and devforge's two add-on gates
  (fresh-docs, ui-verification). Does NOT restate superpowers methodology.
---

# devforge:unified-workflow

This skill is a dispatcher, not a methodology. Each step invokes a superpowers skill; superpowers owns the discipline. devforge only contributes beads bookkeeping and two gates (fresh-docs, ui-verification) that cover ground superpowers does not.

## Flow

| # | Step | Invoke |
|--|--|--|
| 1 | Create epic | `bd create -t epic "<goal>"` |
| 2 | Brainstorm | `superpowers:brainstorming` |
| 3 | Plan | `superpowers:writing-plans` |
| 4 | Decompose | `bd create` per subtask + `bd dep add` (parent-child / blocks) |
| 5 | Isolate | `superpowers:using-git-worktrees` |
| 6 | Implement (per subtask) | `superpowers:test-driven-development`. Before any external library use → `devforge:fresh-docs`. `bd update <id> --claim` → cycle → `bd close`. |
| 6b | On failure | `superpowers:systematic-debugging` |
| 6c | Independent parallel work | `superpowers:dispatching-parallel-agents` / `subagent-driven-development` |
| 7 | Verify | `superpowers:verification-before-completion`. If UI code changed → also `devforge:ui-verification`. |
| 8 | Review | `superpowers:requesting-code-review`; on reply `superpowers:receiving-code-review` |
| 9 | Finish | `superpowers:finishing-a-development-branch`; then `bd close <epic-id>` |

## What this plugin owns (everything else lives in superpowers)

- beads CLI calls and the `bd prime` SessionStart / PreCompact hooks
- `devforge:fresh-docs` — Context7 gate before external library use
- `devforge:ui-verification` — Playwright smoke gate on step 7

## Beads dependencies

Only `blocks` affects `bd ready`. Use `parent-child` for epic→subtask, `related` for independent, `discovered-from` for side quests (`bd create -t bug` + `bd dep add new current --type discovered-from`).

## Graceful degradation

| Missing | Fallback |
|--|--|
| beads | TodoWrite; suggest `/plugin install beads`. Flow continues. |
| superpowers | This plugin refuses to paraphrase the methodology. Install superpowers. |
| Context7 MCP | `devforge:fresh-docs` falls back to WebFetch with `[may be stale]`. |
| Playwright MCP | `devforge:ui-verification` reports skip honestly; never silently passes. |
