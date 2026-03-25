# requesting-code-review

> Use when completing tasks, implementing major features, or before merging to verify work meets requirements.

Dispatches a code-reviewer subagent with structured context.

## Review Request Template

Provide to the reviewer:
- What was implemented
- Plan or requirements reference
- Base and head git SHAs
- Description of changes

## Integration Points

| Workflow                      | When to Review   |
| ----------------------------- | ---------------- |
| `subagent-driven-development` | After each task  |
| `executing-plans`             | After each batch |
| Ad-hoc development            | Before merge/PR  |

## Acting on Feedback

Address issues by severity: blocking first, then simple fixes, then complex changes.

## Bundled Files

- `code-reviewer.md` -- Prompt template for the code review subagent
