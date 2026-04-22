# Fresh docs — always query Context7 first

LLM training data is stale. Libraries change APIs. Writing code from memory is the #1 source of silently wrong implementations.

## The rule

Before writing ANY code that uses an external library, framework, SDK, CLI, or cloud service:

1. `resolve-library-id` with the library name + the specific question.
2. `query-docs` with the chosen library ID (`/org/project` format) and the full question.
3. Write code against the returned docs.

Applies equally to well-known libraries (React, Next.js, Tailwind, Django, Spring Boot) and obscure ones. **Especially** to well-known libraries, because that is where the false sense of confidence lives.

## When to skip

Only these cases:
- Pure business logic with no external API surface.
- Refactoring code you are not changing semantically.
- Debugging your own business logic.
- General programming concepts unrelated to a specific library.

## Fallback if Context7 MCP is unavailable

Use `WebFetch` on the official documentation URL. Add an explicit note: `[warning: docs fetched via WebFetch, may be cached/stale]`. Do not rely on memory.

## Tool names

See `tools-map.md` for the per-harness MCP tool names (Claude Code vs Codex).
