# Additional Slash Commands

Less frequently used commands, available via the command palette (`Ctrl+P`) or by typing directly.

## Session Management

| Command     | Aliases | Description               |
| ----------- | ------- | ------------------------- |
| `/share`    |         | Share session             |
| `/unshare`  |         | Unshare session           |
| `/rename`   |         | Rename session            |
| `/timeline` |         | Jump to message           |
| `/fork`     |         | Fork from message         |
| `/copy`     |         | Copy session transcript   |
| `/export`   |         | Export session transcript |

## Display

| Command       | Aliases              | Description          |
| ------------- | -------------------- | -------------------- |
| `/thinking`   | `/toggle-thinking`   | Show/hide thinking   |
| `/timestamps` | `/toggle-timestamps` | Show/hide timestamps |
| `/status`     |                      | View status          |

## Integration

| Command    | Description        |
| ---------- | ------------------ |
| `/mcps`    | Toggle MCP servers |
| `/connect` | Connect a provider |

## Project

| Command   | Description                            |
| --------- | -------------------------------------- |
| `/init`   | Create/update AGENTS.md                |
| `/review` | Review changes (commit, branch, or PR) |

### `/review` — Argument Modes

`/review` accepts an optional argument that controls what is reviewed:

| Argument          | What is reviewed                                       |
| ----------------- | ------------------------------------------------------ |
| _(none)_          | Uncommitted changes (staged + unstaged)                |
| `<commit-hash>`   | A specific commit by its SHA                           |
| `<branch>`        | All commits on `<branch>` not yet merged into `main`   |
| `<PR-URL>`        | A GitHub pull request by URL                           |
| `<PR-number>`     | A GitHub pull request by number (requires `gh` CLI)    |

Examples:

```
/review
/review abc1234
/review feature/my-branch
/review https://github.com/org/repo/pull/42
/review 42
```

### `/share` and `/unshare`

`/share` publishes the current session as a public URL. Behavior depends on the `share` config:

| Config value  | `/share` behavior                         |
| ------------- | ----------------------------------------- |
| `"manual"`    | Publishes on demand                       |
| `"auto"`      | Already published — returns existing URL  |
| `"disabled"`  | Returns an error                          |

`/unshare` removes the public link for the current session.

See [configuration.md](../config/configuration.md#share) for share configuration.

## Experimental

| Command       | Description       |
| ------------- | ----------------- |
| `/workspaces` | Manage workspaces |

> Requires the `OPENCODE_EXPERIMENTAL_WORKSPACES` flag.

## See Also

- [command-palette.md](command-palette.md) -- Main commands
