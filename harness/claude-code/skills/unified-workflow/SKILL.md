---
name: unified-workflow
description: >
  Mandatory development flow combining beads (tasks), superpowers (methodology),
  context7 (fresh docs), playwright (UI proof). TRIGGER: at session start, before any
  task, or when workflow is unclear. Defines the 9-step flow:
  epic → brainstorm → plan → subtasks → isolate → TDD → verify → review → finish.
---

# devforge:unified-workflow

## The flow

```
1. EPIC       → bd create -t epic "Goal"
2. BRAINSTORM → superpowers:brainstorming
3. PLAN       → superpowers:writing-plans
4. SUBTASKS   → bd create + bd dep add (parent-child / blocks)
5. ISOLATE    → superpowers:using-git-worktrees
6. IMPLEMENT  → per subtask: TDD RED→GREEN→REFACTOR → commit → bd close
7. VERIFY     → superpowers:verification-before-completion
              + devforge:ui-verification (if UI code changed)
8. REVIEW     → superpowers:requesting-code-review
9. FINISH     → superpowers:finishing-a-development-branch
              → bd close <epic-id>
```

## Hard rules

- No production code without a failing test first (see `test-driven-development`).
- Before any external library use → `devforge:fresh-docs` (Context7).
- UI code changed → `devforge:ui-verification` is mandatory on step 7.
- No "done" without running the proving command (see `superpowers:verification-before-completion`).
- All work tracked in beads. Side quests: `bd create -t bug` + `bd dep add new current --type discovered-from`.

## Beads dependency types

Only `blocks` affects `bd ready`:

| Type | Affects ready? | Use for |
|--|--|--|
| blocks | yes | Sequential/technical prerequisite |
| parent-child | no | Epic → subtask |
| related | no | Connected but independent |
| discovered-from | no | Side quest |

Direction: "X needs Y" → `bd dep add X Y`.

## Skill priority

1. `devforge:fresh-docs` first — whenever a library is involved.
2. Process skills (`superpowers:brainstorming`, `superpowers:systematic-debugging`, `superpowers:verification-before-completion`).
3. Implementation skills (`superpowers:test-driven-development`, `superpowers:requesting-code-review`).
4. `devforge:ui-verification` — step 7, UI changes only.

## Graceful degradation

| Missing plugin | Fallback |
|--|--|
| beads | Use TodoWrite. Suggest `/plugin install beads`. |
| superpowers | Use plan mode + AskUserQuestion for brainstorming. Suggest install. |
| context7 | WebFetch official docs with `[warning: may be stale]`. |
| playwright | `ui-verification` reports `skipped: playwright MCP unavailable` — does NOT silently succeed. |

## Anti-patterns

- "Just code it" — skipping brainstorm.
- Tests written after the code.
- "Should work" / "probably fixed" / "I think it passes" — qualifiers instead of run output.
- Running Playwright mid-branch on half-done work (always false-red).
- Trusting LLM memory for library APIs instead of Context7.
- Trusting a subagent's report without your own verification run.

## Canonical references

Full methodology lives in plugin-scoped docs:
- `core/workflow.md` — the flow in full
- `core/tdd.md` — Red/Green/Refactor discipline
- `core/debugging.md` — systematic debugging protocol
- `core/fresh-docs.md` — Context7 rule (see also `devforge:fresh-docs`)
- `core/ui-verification.md` — Playwright checklist (see also `devforge:ui-verification`)
