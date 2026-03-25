# systematic-debugging

> Use when encountering any bug, test failure, or unexpected behavior, before proposing fixes.

4-phase root cause analysis process.

## Phase 1: Root Cause Investigation

- Read the full error message
- Reproduce the issue
- Check recent changes (`git log`, `git diff`)
- In multi-component systems, trace the data flow
- Gather evidence before forming hypotheses

## Phase 2: Pattern Analysis

- Find working examples of similar functionality
- Compare against known-good references
- Identify what differs between working and broken

## Phase 3: Hypothesis and Testing

- Form a single hypothesis
- Test minimally -- one variable at a time
- If the hypothesis is wrong, discard it completely

## Phase 4: Implementation

1. Create a failing test that reproduces the bug
2. Implement a single, targeted fix
3. Verify the fix passes
4. If 3+ fixes fail, question the architecture -- don't keep patching

## Red Flags

- Applying fixes without understanding root cause
- Changing multiple things at once
- "It works now" without knowing why

## Bundled Files

- `root-cause-tracing.md` -- Techniques for tracing root causes
- `defense-in-depth.md` -- Layered debugging strategies
- `condition-based-waiting.md` -- Handling async/timing issues
- `find-polluter.sh` -- Script to find test pollution
