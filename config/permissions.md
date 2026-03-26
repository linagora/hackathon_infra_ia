# Permissions

By default, OpenCode allows the agent to execute most tools without confirmation. The permission system lets you control which tools require approval (`ask`) or are blocked (`deny`).

## Actions

| Action  | Description                          |
| ------- | ------------------------------------ |
| `allow` | Execute without confirmation         |
| `ask`   | Ask user confirmation before running |
| `deny`  | Block the tool entirely              |

## Quick Configuration

### Ask for everything

```json
{ "permission": "ask" }
```

### Ask for dangerous tools only

```json
{
  "permission": {
    "bash": "ask",
    "edit": "ask"
  }
}
```

### Fully autonomous (no prompts)

```json
{ "permission": "allow" }
```

## Pattern-Based Rules

Permissions support wildcard patterns. Order matters -- **the last matching rule wins**.

### Allow git commands, deny rm, ask for the rest

```json
{
  "permission": {
    "bash": {
      "git *": "allow",
      "rm *": "deny",
      "*": "ask"
    }
  }
}
```

### Allow editing only Python files

```json
{
  "permission": {
    "edit": {
      "*.py": "allow",
      "*": "ask"
    }
  }
}
```

### Allow access to a specific external directory

```json
{
  "permission": {
    "external_directory": {
      "~/other-project/*": "allow",
      "*": "ask"
    }
  }
}
```

## Wildcard Matching

- `*` matches any sequence of characters (including `/` in paths)
- `?` matches exactly one character
- `git *` also matches `git` alone (trailing ` *` is optional)
- Patterns starting with `~/` are expanded to the home directory

| Pattern             | Matches                                     |
| ------------------- | ------------------------------------------- |
| `*`                 | Everything                                  |
| `git *`             | `git`, `git status`, `git checkout main`    |
| `git checkout *`    | `git checkout main`, `git checkout -b feat` |
| `rm *`              | `rm`, `rm -rf /`                            |
| `*.py`              | `foo.py`, `src/main.py`                     |
| `*.env`             | `.env`, `config/.env`                       |
| `~/other-project/*` | Any path under `~/other-project/`           |

## What Each Permission Controls

| Permission           | Pattern is                 | Example patterns                          |
| -------------------- | -------------------------- | ----------------------------------------- |
| `bash`               | The full shell command     | `git *`, `npm run *`, `rm *`              |
| `edit`               | Relative file path         | `*.py`, `src/*.ts`, `*.test.ts`           |
| `read`               | Absolute file path         | `/etc/passwd`, `*.env`                    |
| `glob`               | The glob pattern argument  | `src/**/*.ts`                             |
| `grep`               | The search pattern         | `TODO`, `password`                        |
| `task`               | The subagent type          | `explore`, `general`                      |
| `skill`              | The skill name             | `brainstorming`, `review-pr`              |
| `external_directory` | External directory glob    | `~/other-project/*`                       |
| `webfetch`           | The URL                    | `https://api.example.com/*`               |
| `websearch`          | The search query           | Simple action only (`allow`/`ask`/`deny`) |
| `codesearch`         | The search query           | Simple action only                        |
| `todowrite`          | Always `*`                 | Simple action only                        |
| `question`           | Always `*`                 | Simple action only                        |
| `lsp`                | Always `*`                 | Simple action only                        |
| `doom_loop`          | The tool name being looped | Simple action only                        |

## Per-Agent Permissions

Override permissions for a specific agent:

```json
{
  "agent": {
    "build": {
      "permission": {
        "bash": "ask",
        "edit": "ask"
      }
    },
    "explore": {
      "permission": {
        "bash": "deny"
      }
    }
  }
}
```

## Default Permissions

### Global Defaults

| Permission                | Default | Note                                 |
| ------------------------- | ------- | ------------------------------------ |
| `*` (everything)          | `allow` | Most tools run without prompting     |
| `doom_loop`               | `ask`   | Prompts when agent appears stuck     |
| `external_directory`      | `ask`   | Prompts for paths outside project    |
| `question`                | `deny`  | Agent can't ask clarifying questions |
| `read` on `*.env`         | `ask`   | Protects environment files           |
| `read` on `*.env.example` | `allow` | Except example env files             |

### Per-Agent Defaults

| Agent        | Key differences                                                            |
| ------------ | -------------------------------------------------------------------------- |
| `build`      | `question: allow` -- can ask user questions                                |
| `plan`       | `edit: deny` for all except `.opencode/plans/*.md`                         |
| `explore`    | Most tools denied. Only `read`, `grep`, `glob`, `bash`, `webfetch` allowed |
| `compaction` | All tools denied                                                           |
| `title`      | All tools denied                                                           |
| `summary`    | All tools denied                                                           |

## User Prompt Options

When the agent requests permission (`ask`), you see three options:

| Option                | Effect                                                      |
| --------------------- | ----------------------------------------------------------- |
| **Allow once**        | Runs the tool. Next identical call will ask again.          |
| **Allow for session** | Runs the tool. Approves similar future calls automatically. |
| **Deny**              | Blocks the tool. Also denies all other pending requests.    |

"Allow for session" uses smart pattern matching. For example, approving `git checkout main` auto-approves all future `git checkout *` commands.

## Evaluation Order

Rules are evaluated with "last match wins". The full precedence (highest priority last):

1. Global defaults
2. Per-agent defaults (e.g., explore denies most tools)
3. User global `permission` config
4. User per-agent `permission` config
5. Session-level approvals (from "Allow for session" clicks)

## Override via Environment Variable

```bash
OPENCODE_PERMISSION='{"bash":"ask","edit":"ask"}' opencode
```

## See Also

- [configuration.md](configuration.md) -- Full configuration reference
- [agents.md](agents.md) -- Per-agent configuration
- [glossary](../glossary.md) -- Term definitions
