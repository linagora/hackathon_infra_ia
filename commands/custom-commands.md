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

## Named Arguments

Files support placeholders in the format `$ARGUMENTS`, `$1`, `$2`, etc. At execution time, arguments are parsed from the user's input.

### Example

```markdown
<!-- file: fix-issue.md -->
Fix issue #$1 following the project conventions.
Make sure all tests pass.
```

## See Also

- [command-palette.md](command-palette.md) -- Access custom commands via `Ctrl+P`
