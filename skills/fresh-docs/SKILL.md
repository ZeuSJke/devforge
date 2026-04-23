---
name: fresh-docs
description: >
  Query Context7 for current library/framework/SDK docs before writing or modifying
  code that uses them. TRIGGER: about to write, modify, or review code that imports
  or calls into an external library, framework, SDK, CLI tool, or cloud service —
  including well-known ones (React, Next.js, Tailwind, Django, etc.) where stale
  training data produces silently wrong code.
---

# devforge:fresh-docs

devforge gate: superpowers does not cover library-docs lookup. This skill does.

## Rule

1. `mcp__plugin_context7_context7__resolve-library-id` with library name + specific question.
2. Pick best `/org/project` match (trust score, snippet count, exact name match, version-specific when relevant).
3. `mcp__plugin_context7_context7__query-docs` with that ID and the full question.
4. Write the code against the returned docs.

## Skip only when

- Pure business logic with no external API surface.
- Refactors with unchanged semantics.
- Debugging your own business logic.
- Language-level or general programming questions.

## Fallback if MCP is missing

`WebFetch` the official docs URL, label the output `[warning: may be stale]`. Never fill from memory.
