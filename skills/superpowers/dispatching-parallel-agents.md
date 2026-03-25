# dispatching-parallel-agents

> Use when facing 2+ independent tasks that can be worked on without shared state or sequential dependencies.

Runs concurrent agent tasks for faster resolution.

## When to Use

- 3+ test files failing with different root causes
- Multiple independent subsystems broken
- Parallel investigation across unrelated domains

## When NOT to Use

- Failures are related or share state
- Tasks have sequential dependencies
- Single root cause affects multiple symptoms

## Pattern

1. **Identify** independent problem domains
2. **Create** focused, self-contained task prompts
3. **Dispatch** one agent per domain in parallel
4. **Review** results and integrate

## Agent Prompt Structure

Each agent prompt must be:
- **Focused** -- one problem domain only
- **Self-contained** -- all context included
- **Specific** -- clear expected output format
