# verification-before-completion

> Use when about to claim work is complete, fixed, or passing, before committing or creating PRs.

Evidence before assertions, always.

## Iron Law

No completion claims without fresh verification evidence.

## The Gate

1. **IDENTIFY** the verification command
2. **RUN** it fresh (not from memory or cache)
3. **READ** the full output
4. **VERIFY** it confirms the claim
5. **ONLY THEN** make the claim

## Examples

| Claim             | Required Evidence        |
| ----------------- | ------------------------ |
| "Tests pass"      | Test runner output       |
| "Build succeeds"  | Build exit code 0        |
| "Agent completed" | VCS diff showing changes |
| "Bug is fixed"    | Failing test now passes  |

## Red Flags

- Using "should", "probably", "seems to"
- Expressing satisfaction before running verification
- Claiming success based on past runs
