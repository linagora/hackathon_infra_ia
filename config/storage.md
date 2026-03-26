# Storage & Persistence

OpenCode persists data in a SQLite database and instruction files on disk.

## Database Location

| Path                                  | Description                         |
| ------------------------------------- | ----------------------------------- |
| `~/.local/share/opencode/opencode.db` | Default database (SQLite, WAL mode) |
| `OPENCODE_DB` env var                 | Custom database path                |

## What Persists Where

| Data                 | Storage                      | Scope       | Cross-session? |
| -------------------- | ---------------------------- | ----------- | -------------- |
| Sessions (metadata)  | SQLite `session` table       | Per-project | Browsable      |
| Messages + parts     | SQLite `message`/`part`      | Per-session | No             |
| Compaction summaries | SQLite (messages, `summary`) | Per-session | No             |
| Todos                | SQLite `todo` table          | Per-session | No             |
| Permissions          | SQLite `permission` table    | Per-project | Yes            |
| AGENTS.md            | Files on disk                | Project     | Yes            |
| Plan files           | `.opencode/plans/*.md`       | Project     | Yes            |
| Git diffs            | JSON files in storage/       | Per-session | No             |

## Sessions

Every message and tool call is saved to the database in real-time. There is no manual "save" -- state is written on every LLM response chunk.

### Resume a Session

- `<leader>l` or `/sessions` to browse and resume
- `opencode run -c` to continue the last session from CLI
- `opencode run --session <id>` to continue a specific session

### Fork a Session

`/fork` creates a deep copy of all messages, allowing you to branch off from any point.

### Child Sessions

Subagents (via the `task` tool) create child sessions linked via `parent_id`. Navigate with `<leader>Down` / `Up` / `Left` / `Right`.

## Compaction

When a conversation approaches the model's context window limit, OpenCode automatically compacts it.

### How It Works

1. Token usage exceeds threshold (context limit minus ~20K reserved tokens)
2. The `compaction` agent generates a structured summary:
   - **Goal** -- what the user is trying to accomplish
   - **Instructions** -- important instructions from the conversation
   - **Discoveries** -- what was learned
   - **Accomplished** -- completed, in-progress, and remaining work
   - **Relevant files** -- structured file list
3. The summary replaces old messages -- only the summary + messages after it are sent to the model
4. Old tool call outputs exceeding 40K tokens are pruned

### Configuration

```json
{
  "compaction": {
    "auto": true,
    "prune": true,
    "reserved": 20000
  }
}
```

Disable with `OPENCODE_DISABLE_AUTOCOMPACT=1` or in config:

```json
{ "compaction": { "auto": false } }
```

Manual trigger: `<leader>c` or `/compact`

## Cross-Session Memory

OpenCode has **no automatic cross-session memory**. Each session is self-contained.

The only data that carries between sessions:

1. **AGENTS.md** -- project instructions, loaded into every session. See [agents-md.md](agents-md.md).
2. **Plan files** -- written to `.opencode/plans/*.md`, persist on disk. See [plan.md](../commands/plan.md).
3. **Session list** -- you can browse and resume old sessions
4. **Permissions** -- tool permission rules persist per-project

## See Also

- [agents-md.md](agents-md.md) -- AGENTS.md guide
- [plan.md](../commands/plan.md) -- Plan mode and plan files
- [todo.md](../commands/todo.md) -- Session-scoped todos
- [compact.md](../commands/compact.md) -- Manual compaction
- [configuration.md](configuration.md) -- `compaction` config key
