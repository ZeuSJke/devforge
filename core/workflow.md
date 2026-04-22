# devforge workflow — 9 steps

Единая методология разработки. Повторяется на каждой задаче без исключений.

## Mental model

| Плагин | Роль |
|--|--|
| beads | **WHAT** — что делать и в каком порядке |
| superpowers | **HOW** — как работать (brainstorm, plan, TDD, debug, review) |
| context7 | **WITH WHAT** — против каких свежих API пишем код |
| playwright | **PROOF** — материальное доказательство, что UI работает перед merge |

## The flow

```
1. EPIC       → bd create -t epic "Goal"
2. BRAINSTORM → superpowers:brainstorming
3. PLAN       → superpowers:writing-plans (2-5 min subtasks)
4. SUBTASKS   → bd create + bd dep add (parent-child / blocks)
5. ISOLATE    → superpowers:using-git-worktrees (non-trivial work)
6. IMPLEMENT  → per subtask: RED → GREEN → REFACTOR → commit → bd close
7. VERIFY     → superpowers:verification-before-completion
              + devforge:ui-verification (IF UI code changed)
8. REVIEW     → superpowers:requesting-code-review
9. FINISH     → superpowers:finishing-a-development-branch
              → bd close <epic-id>
```

## Cross-cutting rules

- **No production code without a failing test first.** Ever. See `tdd.md`.
- **No completion claims without running the verification command.** Evidence before assertions.
- **No work without a beads task.** Side quests → `bd create -t bug` + `bd dep add new current --type discovered-from`.
- **Before writing any code against an external library, query Context7.** See `fresh-docs.md`.
- **If UI code changed, `ui-verification` is mandatory before closing the epic.** See `ui-verification.md`.

## Skill invocation priority

1. **Context7 first** — fresh docs for any library involved.
2. **Process skills** — brainstorming, debugging, verification.
3. **Implementation skills** — TDD, code-review.
4. **UI gate** — `ui-verification` on step 7 if frontend touched.

## Anti-patterns (never)

- Skip brainstorming → "just code it".
- Write implementation before a failing test.
- Claim "fixed" / "done" without running and quoting the proving command.
- Work without a beads task.
- Run Playwright on a half-done branch (raises false red; Playwright belongs on step 7, not during subtasks).
- Qualifier language: "should work", "probably fixed", "I think this passes".
- Trust a subagent's report without running verification yourself.
- Pull implementation details from memory for a library — memory is stale, Context7 is current.

## Beads dependency types

Only `blocks` affects `bd ready`:

| Type | Affects ready? | Use when |
|--|--|--|
| blocks | yes | Sequential work, technical prerequisite |
| parent-child | no | Epic → subtask hierarchy |
| related | no | Connected but independent |
| discovered-from | no | Side quest found during implementation |

Direction: "X needs Y" → `bd dep add X Y`.
