# test-driven-development

> Use when implementing any feature or bugfix, before writing implementation code.

Enforces the RED-GREEN-REFACTOR cycle.

## Iron Law

No production code without a failing test first.

## Cycle

1. **RED** -- Write a failing test that describes the desired behavior
2. **GREEN** -- Write the minimal implementation to make the test pass
3. **REFACTOR** -- Clean up while keeping tests green

## Mandatory Verification

At each step, run the test suite and verify the expected result (fail or pass) before proceeding.

## If You Wrote Code Before Test

Delete it entirely. Start over with the test.

## Good Tests

- Minimal -- test one thing
- Clear -- shows intent
- Independent -- no shared state

## Bug Fix Pattern

1. Write a test that reproduces the bug (RED)
2. Fix the bug (GREEN)
3. Refactor if needed

## Anti-Patterns

- Tests-after pass immediately (prove nothing)
- Manual testing is ad-hoc and unrepeatable
- Sunk cost fallacy ("but I already wrote it")

## Bundled Files

- `testing-anti-patterns.md` -- Common testing anti-patterns reference
