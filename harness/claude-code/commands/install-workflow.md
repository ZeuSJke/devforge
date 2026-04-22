---
name: install-workflow
description: Idempotently install devforge into ~/.claude — appends CLAUDE.md between guard markers, merges settings.json hooks, runs workflow-doctor.
---

# /install-workflow

Task: install devforge into the user's global Claude Code configuration.

## Steps — perform in order, verify each

### 1. Append CLAUDE.md between guard markers

- Read the current `~/.claude/CLAUDE.md` (create empty if missing).
- Check whether the guards `<!-- devforge:begin -->` and `<!-- devforge:end -->` already exist.
  - If yes: replace the block between them with the current content of `<plugin-root>/CLAUDE.md` (reinstall-safe).
  - If no: append a new block, wrapped in the guards.

Block format:

```
<!-- devforge:begin -->
{contents of plugin CLAUDE.md}
<!-- devforge:end -->
```

### 2. Merge settings.example.json hooks into ~/.claude/settings.json

- Read `~/.claude/settings.json` (treat missing as `{}`).
- Load `settings.example.json` from the plugin.
- For each `hooks.<event>` array in the example:
  - For each hook in that array, identify by the exact `command` string.
  - If an entry with the same command already exists in the user's file, skip it (idempotent).
  - Otherwise, append it.
- Write the merged JSON back with 2-space indent.

### 3. Run /workflow-doctor

Invoke `/workflow-doctor` and display its report.

### 4. Print summary

- Print the list of actions performed (appended/updated/skipped).
- Remind the user to restart the Claude Code session for hooks to take effect.

## Safety

- Never delete unrelated content from `~/.claude/CLAUDE.md`.
- Never overwrite unrelated hooks in `~/.claude/settings.json`.
- On any parse error of existing files, stop and report — do not guess.
