# executing-plans

> Use when you have a written implementation plan to execute in a separate session with review checkpoints.

Loads and executes a plan task by task.

## Process

1. **Load** the plan file
2. **Review** critically before starting
3. **Execute** each task: mark in_progress, follow steps, run verifications, mark completed
4. **Finish** with `finishing-a-development-branch`

## Rules

- Stop and ask for help on blockers -- never guess
- `subagent-driven-development` is preferred when subagents are available
- Each task must pass its verification before moving on

## Required Skills

- `using-git-worktrees`
- `writing-plans`
- `finishing-a-development-branch`
