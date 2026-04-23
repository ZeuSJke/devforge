---
name: workflow-doctor
description: Diagnose devforge environment — check beads, superpowers, guard markers in ~/.claude/CLAUDE.md, and SessionStart/PreCompact hooks. Print actionable report.
---

# /workflow-doctor

Task: verify that devforge is correctly installed and that its peer plugins are available. Print a green/red report with explicit remediation.

## Checks

1. **Guard markers** in `~/.claude/CLAUDE.md`:
   - Exist and non-empty → green.
   - Missing → red, suggest `/install-workflow`.

2. **SessionStart / PreCompact hooks** in `~/.claude/settings.json`:
   - Contain `bd prime` → green.
   - Missing → red, suggest `/install-workflow`.

3. **beads CLI**:
   - `bd --version` exits 0 → green.
   - Not found → red, suggest `/plugin install beads`.

4. **superpowers plugin**:
   - Core skills listed (brainstorming, writing-plans, test-driven-development, verification-before-completion) → green.
   - Missing → red, suggest `/plugin install superpowers`. devforge is an orchestrator and refuses to paraphrase methodology.

5. **Project-level opt-in**:
   - `./.devforge/project.md` exists → green (devforge is active in this project).
   - Missing → yellow (devforge is installed globally but idle here; create `.devforge/project.md` to enable).

## Output format

```
devforge doctor report
----------------------
[ok]   guard markers present in ~/.claude/CLAUDE.md
[ok]   hooks installed in ~/.claude/settings.json
[ok]   bd 1.x.x
[ok]   superpowers skills available
[warn] .devforge/project.md not found in cwd — devforge idle in this project
```

End with a single-line summary: `status: ok | ok-with-warnings | broken`.
