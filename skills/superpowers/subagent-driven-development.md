# subagent-driven-development

> Use when executing implementation plans with independent tasks in the current session.

Dispatches a fresh subagent per task with two-stage review.

## Process (per task)

1. **Dispatch** implementer subagent with focused prompt
2. **Handle** questions (NEEDS_CONTEXT, BLOCKED)
3. **Review stage 1** -- spec compliance
4. **Review stage 2** -- code quality
5. **Mark** task complete

## Implementer Statuses

| Status               | Action                        |
| -------------------- | ----------------------------- |
| `DONE`               | Proceed to review             |
| `DONE_WITH_CONCERNS` | Review concerns, then proceed |
| `NEEDS_CONTEXT`      | Provide context, re-dispatch  |
| `BLOCKED`            | Escalate to user              |

## Model Selection

| Complexity   | Model Tier |
| ------------ | ---------- |
| Mechanical   | Cheap      |
| Integration  | Standard   |
| Architecture | Capable    |

## Advantages

- Fresh context per task (no accumulated confusion)
- Parallel execution when tasks are independent
- Two-stage review catches issues early

## Bundled Files

- `implementer-prompt.md` -- Template for implementer subagent
- `spec-reviewer-prompt.md` -- Template for spec review
- `code-quality-reviewer-prompt.md` -- Template for code quality review
