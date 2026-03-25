# finishing-a-development-branch

> Use when implementation is complete, all tests pass, and you need to decide how to integrate the work.

Guides completion by presenting structured options.

## Process

1. **Verify** all tests pass
2. **Determine** base branch
3. **Present** exactly 4 options:
   - Merge locally
   - Push and create PR
   - Keep branch as-is
   - Discard work
4. **Execute** chosen option with specific git commands
5. **Cleanup** worktree

## Rules

- Discard requires explicit user confirmation
- Worktree cleanup depends on the chosen option
- Always verify tests before presenting options
