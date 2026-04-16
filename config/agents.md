# Agents

OpenCode has 7 agents. The user interacts with **build** (default) and **plan**; the others are internal or subagents.

## Selection & Invocation

### Switching the active agent

`Tab` and `Shift+Tab` cycle through available primary agents (mode `primary` or `all`). The active agent handles all messages in the session until changed.

| Shortcut    | Action                     |
| ----------- | -------------------------- |
| `Tab`       | Next primary agent         |
| `Shift+Tab` | Previous primary agent     |
| `<leader>a` | Open agent list            |

### Invoking a subagent with `@`

Type `@` in the prompt to open a selection menu for agents and files. Selecting an agent invokes it on the current message: the main agent delegates the task to the subagent via the `task` tool.

```
@explore List all TypeScript files in src/
@general Research the best approach for caching
```

The agent name can also be typed directly in the text (`@explore ...`) without going through the menu.

---

## Agent Roles

| Agent        | Mode     | Description                                                    |
| ------------ | -------- | -------------------------------------------------------------- |
| `build`      | primary  | Default agent. Executes tools based on configured permissions. |
| `plan`       | primary  | Plan mode. Disallows edit tools (except plan files).           |
| `general`    | subagent | General-purpose agent for research and multi-step tasks.       |
| `explore`    | subagent | Fast agent for codebase exploration (read-only).               |
| `compaction` | hidden   | Summarizes conversations for context compaction.               |
| `title`      | hidden   | Generates short session titles.                                |
| `summary`    | hidden   | Generates PR-style summaries.                                  |

## Configuration

Configure agents in `opencode.json` under the `agent` key:

```json
{
  "agent": {
    "build": {
      "model": "provider/model-name",
      "variant": "high",
      "steps": 100
    }
  }
}
```

| Key                        | Type    | Description                                          |
| -------------------------- | ------- | ---------------------------------------------------- |
| `agent.<name>.model`       | string  | Model to use (`provider/model` format)               |
| `agent.<name>.variant`     | string  | Model variant (reasoning effort: high, max, minimal) |
| `agent.<name>.temperature` | number  | Temperature for generation                           |
| `agent.<name>.top_p`       | number  | Top-p sampling                                       |
| `agent.<name>.steps`       | number  | Max agentic iterations                               |
| `agent.<name>.prompt`      | string  | Custom system prompt                                 |
| `agent.<name>.disable`     | boolean | Disable this agent                                   |
| `agent.<name>.description` | string  | Custom description                                   |
| `agent.<name>.permission`  | object  | Tool permission configuration                        |
| `agent.<name>.color`       | string  | Display color (hex or theme color name)              |
| `agent.<name>.hidden`      | boolean | Hide from agent list                                 |

## Build Agent Tools

| Tool            | Description                         |
| --------------- | ----------------------------------- |
| **bash**        | Execute shell commands              |
| **read**        | Read file contents                  |
| **edit**        | Edit files with search/replace      |
| **write**       | Write/create files                  |
| **apply_patch** | Apply patches (used for GPT models) |
| **glob**        | Find files by pattern               |
| **grep**        | Search file contents                |
| **task**        | Spawn a subagent                    |
| **webfetch**    | Fetch a URL                         |
| **websearch**   | Search the web                      |
| **codesearch**  | Search code across repositories     |
| **todowrite**   | Manage task lists                   |
| **skill**       | Load and use skills                 |
| **question**    | Ask user a question                 |

## Plan Agent Tools

Same as build, except `edit` and `write` are denied (only allowed for plan files in `.opencode/plans/`).

## Explore Agent Tools (Read-Only)

| Tool           | Description                     |
| -------------- | ------------------------------- |
| **read**       | Read file contents              |
| **grep**       | Search file contents            |
| **glob**       | Find files by pattern           |
| **bash**       | Execute shell commands          |
| **webfetch**   | Fetch a URL                     |
| **websearch**  | Search the web                  |
| **codesearch** | Search code across repositories |

## See Also

- [configuration.md](configuration.md) -- Full configuration reference
- [providers.md](providers.md) -- Provider setup
