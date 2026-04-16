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

## Creating a Custom Agent

Custom agents appear in the `Tab` cycle (if `primary` or `all`) and in the `@` autocomplete menu (if `subagent` or `all`).

### Option 1 — Markdown file

Create a `.md` file with YAML frontmatter in one of these directories:

| Location                              | Scope   |
| ------------------------------------- | ------- |
| `<project>/.opencode/agent/*.md`      | Project |
| `~/.config/opencode/agent/*.md`       | Global  |

The filename (without `.md`) becomes the agent name used in `Tab` and `@`.

```markdown
---
description: Use this agent when you need to review code for security vulnerabilities.
mode: subagent
model: anthropic/claude-opus-4-5
variant: high
steps: 30
color: "#FF5733"
permission:
  bash: deny
  edit: deny
  write: deny
---
You are an expert security auditor. Review the provided code for vulnerabilities
(OWASP Top 10, injection, auth issues, etc.). Report findings with severity and
suggested fixes. Do not modify files.
```

| Frontmatter key | Type                             | Description                                               |
| --------------- | -------------------------------- | --------------------------------------------------------- |
| `description`   | string                           | Shown in `@` autocomplete — describe when to use it      |
| `mode`          | `primary` \| `subagent` \| `all` | Role of the agent                                         |
| `model`         | string                           | Model to use (`provider/model` format)                    |
| `variant`       | string                           | Reasoning effort: `high`, `max`, `minimal`                |
| `steps`         | number                           | Max agentic iterations                                    |
| `temperature`   | number                           | Sampling temperature                                      |
| `top_p`         | number                           | Top-p sampling                                            |
| `color`         | string                           | Display color (hex `#RRGGBB` or theme: `primary`, `info`, `success`, `warning`, `error`) |
| `hidden`        | boolean                          | Hide from `@` autocomplete (subagents only)               |
| `permission`    | object                           | Tool permissions (`allow` / `deny` / `ask` per tool)      |

### Option 2 — `opencode agent create`

Generates a `.md` file interactively (or non-interactively). See [`opencode agent`](../commands/cli.md#opencode-agent).

```bash
opencode agent create --description "Reviews code for security issues" --mode subagent
```

---

## Configuration

Configure or override agents in `opencode.json` under the `agent` key. This works for both built-in agents and custom agents defined as `.md` files.

### Override a built-in agent

```json
{
  "agent": {
    "build": {
      "model": "anthropic/claude-opus-4-5",
      "variant": "high",
      "steps": 100,
      "permission": {
        "bash": "ask"
      }
    },
    "plan": {
      "model": "openai/o3"
    }
  }
}
```

### Define a new agent inline

```json
{
  "agent": {
    "security-reviewer": {
      "description": "Reviews code for security vulnerabilities",
      "mode": "subagent",
      "model": "anthropic/claude-opus-4-5",
      "steps": 30,
      "color": "#FF5733",
      "permission": {
        "bash": "deny",
        "edit": "deny",
        "write": "deny"
      },
      "prompt": "You are an expert security auditor. Review the provided code for vulnerabilities and report findings with severity and suggested fixes."
    }
  }
}
```

### Set a custom default agent

```json
{
  "default_agent": "security-reviewer"
}
```

| Key                        | Type                             | Description                                          |
| -------------------------- | -------------------------------- | ---------------------------------------------------- |
| `agent.<name>.model`       | string                           | Model to use (`provider/model` format)               |
| `agent.<name>.variant`     | string                           | Model variant (reasoning effort: high, max, minimal) |
| `agent.<name>.temperature` | number                           | Temperature for generation                           |
| `agent.<name>.top_p`       | number                           | Top-p sampling                                       |
| `agent.<name>.steps`       | number                           | Max agentic iterations                               |
| `agent.<name>.prompt`      | string                           | Custom system prompt (appended or replaces default)  |
| `agent.<name>.description` | string                           | Description shown in `@` autocomplete                |
| `agent.<name>.mode`        | `primary` \| `subagent` \| `all` | Agent role                                           |
| `agent.<name>.color`       | string                           | Display color (hex or theme color name)              |
| `agent.<name>.hidden`      | boolean                          | Hide from `@` autocomplete (subagents only)          |
| `agent.<name>.permission`  | object                           | Tool permission configuration                        |
| `agent.<name>.disable`     | boolean                          | Disable this agent                                   |

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
