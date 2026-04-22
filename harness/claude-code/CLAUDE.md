# devforge — unified dev workflow

Before starting ANY task, invoke `devforge:unified-workflow`.

## Flow (do not skip steps)

1. `bd create -t epic "Goal"`
2. `superpowers:brainstorming` — design before code
3. `superpowers:writing-plans` — 2-5 min subtasks
4. `bd create` per subtask + `bd dep add`
5. `superpowers:using-git-worktrees` — for non-trivial work
6. Per subtask: TDD RED → GREEN → REFACTOR → commit → `bd close`
7. `superpowers:verification-before-completion` + `devforge:ui-verification` (if UI changed)
8. `superpowers:requesting-code-review`
9. `superpowers:finishing-a-development-branch` → `bd close <epic-id>`

## Hard rules

- No production code without a failing test first.
- No "done" claims without the proving command's output.
- No work without a beads task.
- Before any library use: `devforge:fresh-docs` (Context7).
- UI code changed → `devforge:ui-verification` (Playwright) is mandatory at step 7.

## Mental model

- **beads** = WHAT (tasks & order)
- **superpowers** = HOW (methodology)
- **context7** = WITH WHAT (current APIs)
- **playwright** = PROOF (UI actually works in a browser)

Full methodology: see skill files.
