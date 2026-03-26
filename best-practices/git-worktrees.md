# Git Worktrees

Git worktrees let you work on multiple branches simultaneously without stashing or switching. OpenCode can create and manage worktrees for you.

## Why Use Worktrees

- **Isolation**: feature work doesn't affect your main branch
- **Parallel work**: run tests on one branch while developing on another
- **Safe experimentation**: discard the worktree if the approach fails
- **Clean baseline**: each worktree starts with a fresh project setup

## Quick Start

Ask the agent:

```text
"Create a worktree for the new auth feature"
"Set up an isolated workspace to refactor the database layer"
```

The agent will:
1. Create a new branch and worktree
2. Run project setup (`npm install`, `cargo build`, etc.)
3. Verify tests pass in the clean worktree
4. Report the worktree location

## Setup

### 1. Create a worktree directory

Add a `.worktrees/` or `worktrees/` directory at the project root and gitignore it:

```bash
mkdir .worktrees
echo ".worktrees/" >> .gitignore
```

### 2. Document it in AGENTS.md (optional)

```markdown
## Worktrees

Use `.worktrees/` for isolated feature work.
```

The agent checks these locations in order:
1. Existing `.worktrees/` or `worktrees/` directory
2. `AGENTS.md` for project conventions
3. Asks the user

## Manual Usage

### Create

```bash
git worktree add .worktrees/my-feature -b feat/my-feature
cd .worktrees/my-feature
npm install   # or cargo build, pip install, etc.
```

### List

```bash
git worktree list
```

### Remove

```bash
git worktree remove .worktrees/my-feature
```

Or let the agent clean up when finishing a branch.

## Workflow with OpenCode

### 1. Plan in main, implement in worktree

```text
"Switch to plan agent and design the auth feature"
```

The plan agent writes to `.opencode/plans/`. Then:

```text
"Create a worktree and implement the auth plan"
```

### 2. Review and merge

```text
"Run tests in the worktree"
"Show me the diff against main"
"Merge the worktree branch into main"
```

### 3. Finishing options

When implementation is complete, the agent presents 4 options:
- **Merge locally** -- merge into the base branch and clean up
- **Push and create PR** -- push the branch and open a pull request
- **Keep as-is** -- leave the worktree for later
- **Discard** -- remove the worktree and branch (requires confirmation)

## Tips

- Always gitignore the worktree directory (`.worktrees/`)
- Run project setup in the worktree before starting work
- Verify tests pass in a clean worktree before merging
- Use meaningful branch names (`feat/auth`, `fix/memory-leak`)
- One feature per worktree -- keep the scope focused

## See Also

- [../skills/superpowers/using-git-worktrees.md](../skills/superpowers/using-git-worktrees.md) -- Superpowers skill for automated worktree creation
- [../skills/superpowers/finishing-a-development-branch.md](../skills/superpowers/finishing-a-development-branch.md) -- Branch integration workflow
- [../commands/plan.md](../commands/plan.md) -- Plan mode
