---
name: fresh-docs
description: >
  Query Context7 for current library/framework/SDK docs before writing code.
  TRIGGER: about to write, modify, or review code that imports or calls into
  any external library, framework, SDK, CLI tool, or cloud service. Applies
  to well-known libraries too (React, Next.js, Tailwind, Django, etc.) — LLM
  training data is stale.
---

# devforge:fresh-docs

## The rule

Before writing code against ANY external API surface:

1. `mcp__plugin_context7_context7__resolve-library-id` with library name + question.
2. Pick best match in `/org/project` format (prefer higher trust score, higher snippet count, exact name match; version-specific IDs when version is known).
3. `mcp__plugin_context7_context7__query-docs` with that ID and the **full** question (not one word).
4. Implement against the returned docs.

## When to skip

Only these:
- Pure business logic with no external API surface.
- Refactoring (semantics unchanged).
- Debugging your own business logic.
- General programming concepts.

Everything else — including libraries you "know well" — goes through Context7.

## Fallback

If Context7 MCP is unavailable:
- `WebFetch` on the official docs URL.
- Label the answer `[warning: docs fetched via WebFetch, may be cached]`.
- Never fill in from memory.

## Why this is rigid

"I know this API" is the most common source of silently wrong code. APIs change between minor versions. Defaults change. Deprecations appear. Confidence is not a substitute for current documentation.

See `core/fresh-docs.md` for the full rationale.
