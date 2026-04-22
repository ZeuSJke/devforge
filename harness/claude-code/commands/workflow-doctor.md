---
name: workflow-doctor
description: Diagnose devforge environment — check beads, superpowers, context7 MCP, playwright MCP, project config, and guard markers in ~/.claude/CLAUDE.md. Print actionable report.
---

# /workflow-doctor

Task: verify that devforge is correctly installed and that its peer tools are available. Print a green/red report with explicit remediation.

## Checks

1. **Guard markers** in `~/.claude/CLAUDE.md`:
   - Exist and non-empty → green.
   - Missing → red, suggest `/install-workflow`.

2. **SessionStart / PreCompact hooks** in `~/.claude/settings.json`:
   - Contain `bd prime` and the `[devforge]` echo reminder → green.
   - Missing → red, suggest `/install-workflow`.

3. **beads CLI**:
   - `bd --version` exits 0 → green.
   - Not found → red, suggest `/plugin install beads`.

4. **superpowers plugin**:
   - Skill `superpowers:unified-workflow`-adjacent skills listed (brainstorming, writing-plans, TDD, verification-before-completion) → green.
   - Missing → red, suggest `/plugin install superpowers`.

5. **Context7 MCP**:
   - Tool `mcp__plugin_context7_context7__resolve-library-id` available → green.
   - Missing → red, suggest installing Context7 MCP.

6. **Playwright MCP**:
   - Tool `mcp__plugin_playwright_playwright__browser_navigate` available → green.
   - Missing → yellow (ui-verification will skip honestly), suggest Playwright MCP install.

7. **Project-level config**:
   - `./.devforge/project.md` exists OR project `CLAUDE.md` has a `## devforge project config` section → green.
   - Missing → yellow (ui-verification will skip, verification-before-completion will ask for commands).

## Output format

```
devforge doctor report
----------------------
[ok]   guard markers present in ~/.claude/CLAUDE.md
[ok]   hooks installed in ~/.claude/settings.json
[ok]   bd 1.x.x
[ok]   superpowers skills available
[ok]   context7 MCP
[warn] playwright MCP not found → install to enable ui-verification
[warn] no project config found → ui-verification will skip
```

End with a single-line summary: `status: ok | ok-with-warnings | broken`.
