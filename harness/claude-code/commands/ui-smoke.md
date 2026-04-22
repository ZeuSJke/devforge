---
name: ui-smoke
description: Run devforge:ui-verification on-demand against the current project. Useful for quick checks outside the workflow flow (e.g. after a hotfix).
---

# /ui-smoke

Task: run the `devforge:ui-verification` checklist against the current project's dev server and critical UI paths.

## Steps

1. Load project config (`.devforge/project.md` or project `CLAUDE.md` section). If missing → print guidance and stop.
2. Invoke the `devforge:ui-verification` skill — follow its checklist exactly.
3. Report pass/fail per critical path. Final line: `ui-smoke: <N> passed, <M> failed`.

## Exit behavior

- All paths green → print the summary.
- Any path red → print which path and why (console/network/navigation). Do NOT attempt to fix; surface to the user.
