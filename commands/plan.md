# Plan Mode

The `plan` agent creates implementation plans before writing code. It has access to all tools except `edit` and `write` (only allowed for plan files in `.opencode/plans/`).

## Access

- `Tab` or `Shift+Tab` to cycle agents (switch to `plan`)
- `<leader>a` or `/agents` to select `plan`

## How It Works

1. Switch to the `plan` agent
2. Describe the task or feature
3. The agent researches, explores code, and writes a plan to `.opencode/plans/<timestamp>-<slug>.md`
4. Switch back to the `build` agent to execute the plan

## Plan Files

| Location                         | Description            |
| -------------------------------- | ---------------------- |
| `<project>/.opencode/plans/`     | Git repos (gitignored) |
| `~/.local/share/opencode/plans/` | Non-git projects       |

Plan files persist on disk and survive across sessions. When switching from `plan` to `build`, a reminder with the plan file path is injected.

## Configuration

```json
{
  "agent": {
    "plan": {
      "model": "provider/model-name",
      "steps": 50
    }
  }
}
```

> Plan mode requires the `OPENCODE_EXPERIMENTAL_PLAN_MODE` flag.

## See Also

- [../config/agents.md](../config/agents.md) -- Agent roles (build vs plan)
- [../config/storage.md](../config/storage.md) -- Persistence overview
