# init

Creates or updates an `AGENTS.md` file containing build commands, code style guidelines, and other useful context information.

## Access

- Via command palette: `/init`

## Behavior

1. Analyzes the project structure (files, languages, dependencies)
2. Generates an `AGENTS.md` file at the project root
3. This file is automatically loaded as context in future sessions

## See Also

- [configuration.md](../config/configuration.md) -- `instructions` setting (files loaded as context)
- [command-palette.md](command-palette.md) -- Access via `Ctrl+P`
