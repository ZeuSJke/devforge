# fresh-docs — Context7 gate

Superpowers does not cover library-docs lookup. This is devforge's responsibility.

## Rule

Before writing, modifying, or reviewing code that imports or calls into an external library, framework, SDK, CLI, or cloud service:

1. `mcp__…__resolve-library-id` with library name + the specific question.
2. Pick the best `/org/project` match (higher trust score, higher snippet count, exact name match; version-specific IDs when version matters).
3. `mcp__…__query-docs` with that ID and the full question (not a single keyword).
4. Write code against the returned docs.

Applies to well-known libraries too — React, Next.js, Tailwind, Django, Spring Boot. Confidence in an API from training data is the most common source of silently wrong code.

## When to skip

- Pure business logic with no external API surface.
- Refactors that don't change semantics.
- Debugging your own business logic.
- Language-level or general-programming questions.

Everything else goes through Context7.

## Fallback if MCP is unavailable

`WebFetch` the official docs URL and label the output `[warning: fetched via WebFetch, may be stale]`. Do not fill in from memory.

## Tool names

See `tools-map.md` for the harness-specific MCP prefix (Claude Code vs Codex).
