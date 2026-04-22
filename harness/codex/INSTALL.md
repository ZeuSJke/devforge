# devforge — Codex CLI install

Two copy-paste steps. No scripts, no remote execution — you can read exactly what lands in your config before committing to it.

## Step 1 — AGENTS.md

Pick one location:

- **Global (all projects)**: `~/.codex/instructions.md`
- **Project-only**: `AGENTS.md` in the project root

Open that file (create it if missing) and paste the block below. The guard markers make re-runs safe — you can replace just the contents between the markers next time you update.

```
<!-- devforge:begin -->

{paste the full contents of harness/codex/AGENTS.md here}

<!-- devforge:end -->
```

## Step 2 — MCP servers

Open `~/.codex/config.toml` and paste the block from `harness/codex/config.example.toml` (also between its own guard markers). It declares:

- `[mcp_servers.playwright]` — the Playwright MCP, needed for `ui-verification`.
- `[mcp_servers.context7]` — the Context7 MCP, needed for `fresh-docs`.

If you already have these MCPs under different names, keep yours — just update `tools-map.md` accordingly so `fresh-docs` / `ui-verification` reference the right prefix.

## Step 3 — beads

Beads is a CLI, not an MCP. Install per the [beads README](https://github.com/steveyegge/beads) so `bd` is on your PATH. devforge assumes `bd prime`, `bd create`, `bd ready`, `bd close` work.

## Step 4 — verify

Start a new Codex session in a project directory and ask Codex to quote the 9-step flow from the AGENTS.md. If it cannot, AGENTS.md did not load — check the path.

Then try a tiny task ("add a failing test for X") — Codex should:
- create a beads epic,
- write a plan,
- write the failing test and verify it fails,
- implement and verify green,
- close the task.

If any step is skipped, the AGENTS.md is not being followed — file an issue.

## No CLAUDE.md-style hook layer in Codex

Codex has no equivalent of Claude Code's `SessionStart` hook. The reminder lives in AGENTS.md itself, which is loaded every session. This is sufficient — the whole point of AGENTS.md is to be the always-loaded protocol.

## Uninstall

Remove the blocks between the `<!-- devforge:begin -->` / `<!-- devforge:end -->` guards in both files. That's it.
