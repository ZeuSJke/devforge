# Systematic debugging

Before any fix, invoke this protocol. Shortcuts produce fake fixes.

## Five steps

1. **Reproduce** — minimal command/input that triggers the bug. If you can't reproduce, you can't claim a fix.
2. **Root-cause investigation** — read the actual error, the actual logs, the actual recent diff. Not what you expect to be there.
3. **Pattern analysis** — find a working comparable in the codebase. What does it do differently?
4. **Single-variable hypothesis** — change one thing. Run the reproduction. Did it change? If many variables changed at once, you're guessing.
5. **Fix via failing test** — write a test that fails because of the bug, then fix. The test protects against regression.

## Stopping rule

After **3 failed hypotheses**, stop patching. Question the architecture. The bug may be a symptom of a design issue, not a local defect.

## What not to do

- Change multiple things hoping one fixes it.
- Claim "fixed" after one run without the failing test.
- Silence the error (try/except, nullish coalescing, default values) instead of understanding it.
- Trust LLM memory for the behavior of a library — check Context7 / the source.
