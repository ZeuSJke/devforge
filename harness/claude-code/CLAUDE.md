# devforge — unified dev workflow

Before starting ANY task, invoke `devforge:unified-workflow`. It is a dispatch table: each step of the flow points to the superpowers skill that owns the discipline for that step. devforge does NOT restate superpowers — it sequences its skills and adds beads / Context7 / Playwright where superpowers has no coverage.

## Flow (do not skip; each cell is an invocation)

| # | Step | Invoke |
|--|--|--|
| 1 | Create epic | `bd create -t epic "<goal>"` |
| 2 | Brainstorm | `superpowers:brainstorming` |
| 3 | Plan | `superpowers:writing-plans` |
| 4 | Decompose | `bd create` per subtask + `bd dep add` |
| 5 | Isolate | `superpowers:using-git-worktrees` |
| 6 | Implement (per subtask) | `superpowers:test-driven-development`; `devforge:fresh-docs` before any external library use |
| 6b | On failure | `superpowers:systematic-debugging` |
| 7 | Verify | `superpowers:verification-before-completion` + `devforge:ui-verification` if UI changed |
| 8 | Review | `superpowers:requesting-code-review` / `receiving-code-review` |
| 9 | Finish | `superpowers:finishing-a-development-branch`; then `bd close <epic-id>` |

## What devforge itself owns (everything else is superpowers)

- **beads bookkeeping** — `bd create / claim / close / dep add`, and the `bd prime` hooks.
- **Context7 gate** (`devforge:fresh-docs`) — invoke before any external library use.
- **Playwright gate** (`devforge:ui-verification`) — invoke on step 7 when UI code changed.

If superpowers is not installed, install it — this plugin is an orchestrator, not a replacement.
