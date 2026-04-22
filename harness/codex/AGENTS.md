# devforge — unified dev workflow (Codex edition)

Before any task, follow this flow. This file is your always-loaded protocol; deeper docs are in `references/` and should be read on demand via the shell `cat` primitive.

## Flow

1. `bd create -t epic "Goal"`
2. Brainstorm before code — see `references/workflow.md` "brainstorm" section.
3. Write a plan of 2-5 min subtasks — see `references/workflow.md` "plan".
4. `bd create` per subtask + `bd dep add`.
5. Isolate non-trivial work in a git worktree.
6. Per subtask: RED → VERIFY RED → GREEN → VERIFY GREEN → REFACTOR → commit → `bd close`. See `references/tdd.md`.
7. Verify: run the proving test command and quote the output. If UI changed, also run the Playwright gate from `references/ui-verification.md`.
8. Request code review.
9. Finish branch → `bd close <epic-id>`.

## Hard rules

- No production code without a failing test first.
- No "done" without quoting the proving command's output.
- No work without a beads task.
- Before using any external library: read `references/fresh-docs.md`, then query Context7 (`context7.resolve-library-id` → `context7.query-docs`).
- UI code changed → follow `references/ui-verification.md` before closing the epic.

## When to read which reference

| Situation | Read |
|--|--|
| Starting any task | `references/workflow.md` |
| About to write a test or implementation | `references/tdd.md` |
| Bug or unexpected behavior | `references/debugging.md` |
| About to import a library | `references/fresh-docs.md` |
| UI code changed, step 7 | `references/ui-verification.md` |
| Unsure of tool name in Codex | `../tools-map.md` |

Do not inline these into this file. Codex loads AGENTS.md every turn — keep it small.

## Mental model

beads = WHAT · superpowers (methodology) = HOW · context7 = WITH WHAT · playwright = PROOF.

## Project config

If this project has a `.devforge/project.md`, read it — it declares test/build/dev-server commands and critical UI paths. Without it:
- Ask the user for the test command before claiming done.
- `ui-verification` is skipped honestly (never silently succeed).

## Anti-patterns

- "Just code it" / skipping the plan.
- Writing code before a failing test.
- "Should work" / "probably fixed" — qualifiers without run output.
- Running Playwright mid-branch on half-done integration.
- Pulling library APIs from memory instead of Context7.
