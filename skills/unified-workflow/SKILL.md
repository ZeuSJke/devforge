---
name: unified-workflow
description: >
  Dispatch table for the devforge flow. Opt-in per project: activates only when a
  `.devforge/project.md` file exists in the current project root. TRIGGER: at session
  start or before any task in a devforge-enabled project. Maps each step to the
  superpowers skill that owns its discipline, plus beads bookkeeping and the
  Playwright UI-verification gate. Does NOT restate superpowers, beads, or Context7.
---

# devforge:unified-workflow

This skill is a dispatcher, not a methodology. Each step invokes a skill from another plugin; that plugin owns the discipline. devforge only contributes the sequencing and one gate superpowers does not cover (the Playwright UI check on step 7).

**Opt-in per project.** Before applying the flow, check that `.devforge/project.md` exists at the project root. If it does not: this flow does not apply — follow the project's own methodology instead.

## Flow

| # | Step | Invoke |
|--|--|--|
| 1 | Create epic | `bd create -t epic "<goal>"` |
| 2 | Brainstorm | `superpowers:brainstorming` |
| 3 | Plan | `superpowers:writing-plans` |
| 4 | Decompose | `bd create` per subtask + `bd dep add` (see `beads:beads` for dependency semantics) |
| 5 | Isolate | `superpowers:using-git-worktrees` |
| 6 | Implement (per subtask) | `superpowers:test-driven-development`. `bd update <id> --claim` → cycle → `bd close`. External library usage is handled by Context7 MCP's own injected instructions when Context7 is installed. |
| 6b | On failure | `superpowers:systematic-debugging` |
| 6c | Independent parallel work | `superpowers:dispatching-parallel-agents` / `subagent-driven-development` |
| 7 | Verify | `superpowers:verification-before-completion`. If UI code changed → also `devforge:ui-verification`. |
| 8 | Review | `superpowers:requesting-code-review`; on reply `superpowers:receiving-code-review` |
| 9 | Finish | `superpowers:finishing-a-development-branch`; then `bd close <epic-id>` |

## What this plugin owns (and only this)

- The 9-step sequence above.
- The `bd prime` SessionStart / PreCompact hooks (project-gated).
- `devforge:ui-verification` — Playwright smoke gate on step 7 for UI changes.

Everything else — TDD, debugging, planning, review, worktrees, parallel agents, verification-before-completion, beads dependency semantics, library-docs lookup — lives in the respective plugins. Do not paraphrase them.

## Graceful degradation

| Missing | Behavior |
|--|--|
| beads | TodoWrite fallback; suggest `/plugin install beads`. |
| superpowers | devforge refuses to paraphrase the methodology. Install superpowers. |
| Context7 MCP | Library docs rely on Context7's own injected instructions. If missing: flagged by `/workflow-doctor`; Claude falls back to WebFetch / training data. |
| Playwright MCP | `devforge:ui-verification` reports `skipped: playwright MCP unavailable`. Never silently passes. |
