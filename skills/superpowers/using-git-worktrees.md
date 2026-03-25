# using-git-worktrees

> Use when starting feature work that needs isolation from current workspace or before executing implementation plans.

Creates isolated git worktrees with smart directory selection and safety verification.

## Directory Selection Priority

1. Check existing `.worktrees` or `worktrees` directory
2. Check `AGENTS.md` for project conventions
3. Ask the user

## Safety Verification

The worktree directory **must** be gitignored before creating project-local worktrees. Verify this before proceeding.

## Creation Steps

1. Detect project name
2. Create worktree with new branch (`git worktree add`)
3. Auto-detect and run project setup:
   - `npm install` / `yarn` / `pnpm install`
   - `cargo build`
   - `pip install` / `poetry install`
   - `go mod download`
4. Verify clean test baseline
5. Report location to user
