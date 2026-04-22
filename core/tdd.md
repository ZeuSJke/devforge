# TDD — Red, Green, Refactor

Rigid discipline. No exceptions for production code.

## The cycle

```
RED → VERIFY RED → GREEN → VERIFY GREEN → REFACTOR → COMMIT
```

1. **RED**: write ONE failing test for the desired behavior.
2. **VERIFY RED**: run it. It must fail for the **right** reason (not import error, not typo).
3. **GREEN**: write the minimal code that makes it pass. No speculative generalization.
4. **VERIFY GREEN**: all tests pass. Quote the command and its exit code.
5. **REFACTOR**: clean up with tests still green.
6. **COMMIT**: after each green. Small commits.

## Level of test per step

| Step | Test scope | Tool examples |
|--|--|--|
| 6 (IMPLEMENT, subtask) | unit, component | Jest, Vitest, React Testing Library, Vue Test Utils |
| 7 (VERIFY, branch) | integration, e2e smoke | superpowers:verification-before-completion + devforge:ui-verification |

Component test for a UI change ≠ Playwright e2e. Component tests are part of the TDD loop; Playwright is the **final** branch-level gate (see `ui-verification.md`).

## What TDD is NOT

- Not "write tests after". Tests drive the code, not document it.
- Not "run the full suite once at the end". Each cycle is tight: one RED, one GREEN.
- Not "write 10 tests then implement". One failing test at a time.
