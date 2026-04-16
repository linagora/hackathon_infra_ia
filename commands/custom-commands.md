# Custom Commands

User-defined or project-defined commands, accessible via the command palette (`Ctrl+P`).

## Locations

| Type    | Directory                       |
| ------- | ------------------------------- |
| User    | `~/.config/opencode/commands/`  |
| Project | `<project>/.opencode/commands/` |

## Format

Custom commands are `.md` files whose content is sent as a prompt. The command name is derived from the file path relative to the commands directory.

Subdirectories create namespaced command names. For example:
- `commands/git/commit.md` becomes `git/commit`
- `commands/review.md` becomes `review`

## Invocation

Custom commands are accessible in two ways:

- **Command palette** — `Ctrl+P`, then type the command name
- **Direct** — type `/command-name` directly in the prompt input (e.g. `/review`, `/git/commit`)

## Frontmatter Options

Command files can optionally include YAML frontmatter to set metadata:

```markdown
---
description: Fix a GitHub issue by number
agent: build
model: anthropic/claude-opus-4-5
subtask: false
---
Fix issue #$1 following the project conventions.
Make sure all tests pass.
```

| Key           | Type    | Description                                                    |
| ------------- | ------- | -------------------------------------------------------------- |
| `description` | string  | Shown in the command palette picker                            |
| `agent`       | string  | Agent to use for this command (must be a primary agent)        |
| `model`       | string  | Model override for this command (`provider/model` format)      |
| `subtask`     | boolean | Run the command as a subtask (spawned subagent)                |

## Named Arguments

Files support placeholders in the format `$ARGUMENTS`, `$1`, `$2`, etc. At execution time, arguments are parsed from the user's input.

### Example

```markdown
<!-- file: fix-issue.md -->
Fix issue #$1 following the project conventions.
Make sure all tests pass.
```

## Programmatic Registration

Plugins can register slash commands dynamically via the [`config` hook](../config/hooks.md#config):

```typescript
"config": async (config) => {
  config.command ??= {}
  config.command["my-command"] = {
    description: "Run my custom workflow",
    template: "Load and follow the my-command skill.",
  }
}
```

| Field         | Type    | Description                                              |
| ------------- | ------- | -------------------------------------------------------- |
| `description` | string  | Shown in the command palette picker                      |
| `template`    | string  | Prompt template sent when the command is invoked         |
| `agent`       | string  | Agent to use for this command                            |
| `model`       | string  | Model override (`provider/model` format)                 |
| `subtask`     | boolean | Run as a subtask                                         |

## See Also

- [command-palette.md](command-palette.md) -- Access custom commands via `Ctrl+P`
- [../config/hooks.md](../config/hooks.md) -- `config` hook for dynamic registration
