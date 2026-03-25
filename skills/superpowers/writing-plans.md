# writing-plans

> Use when you have a spec or requirements for a multi-step task, before touching code.

Writes comprehensive implementation plans for engineers with zero codebase context.

## Process

1. **Scope check** -- break multi-subsystem specs into separate plans
2. **Map file structure** before defining tasks
3. **Define tasks** -- each step is 2-5 minutes of work
4. **Review** plan with plan-document-reviewer subagent (max 3 iterations)
5. **Hand off** to execution (subagent-driven or inline)

## Plan Document Format

Each plan has a header with metadata, then tasks with:
- Exact file paths
- Complete code to write
- TDD steps (write failing test, verify fail, implement, verify pass, commit)

## Task Granularity

Every task must be bite-sized (2-5 minutes). If a task takes longer, it needs to be split.

## Execution Handoff

Offers two execution modes:
- **Subagent-driven** (recommended) -- via `subagent-driven-development`
- **Inline execution** -- via `executing-plans`

## Bundled Files

- `plan-document-reviewer-prompt.md` -- Prompt for the plan review subagent
