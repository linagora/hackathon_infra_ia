# Todos

The `todowrite` tool lets the agent manage a task list during a session.

## How It Works

- The agent creates and updates todos via the `todowrite` tool
- Todos are displayed in the TUI sidebar
- Each todo has a status (`pending`, `in_progress`, `completed`) and priority

## Scope

Todos are **session-scoped** -- they do not persist or carry over between sessions. Each session has its own independent task list.

## Storage

Stored in the SQLite database (`todo` table), keyed by `session_id` and `position`.

## See Also

- [../config/storage.md](../config/storage.md) -- Persistence overview
- [../config/agents.md](../config/agents.md) -- `todowrite` tool available to build agent
