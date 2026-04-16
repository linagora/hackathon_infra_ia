# Environment Variables

Environment variables used by OpenCode.

## Configuration

| Variable                          | Description                                   |
| --------------------------------- | --------------------------------------------- |
| `OPENCODE_CONFIG`                 | Path to a custom config file                  |
| `OPENCODE_CONFIG_CONTENT`         | Inline config (JSON string, highest priority) |
| `OPENCODE_CONFIG_DIR`             | Extra directory to load config from           |
| `OPENCODE_TUI_CONFIG`             | Path to custom TUI config file                |
| `OPENCODE_DISABLE_PROJECT_CONFIG` | Skip loading project-level config             |
| `OPENCODE_DB`                     | Custom database path                          |
| `OPENCODE_PERMISSION`             | Permission mode override                      |

## Feature Toggles

| Variable                              | Description                             |
| ------------------------------------- | --------------------------------------- |
| `OPENCODE_DISABLE_AUTOCOMPACT`        | Disable automatic compaction            |
| `OPENCODE_DISABLE_AUTOUPDATE`         | Disable automatic updates               |
| `OPENCODE_DISABLE_PRUNE`              | Disable session pruning                 |
| `OPENCODE_DISABLE_MODELS_FETCH`       | Disable fetching models from models.dev |
| `OPENCODE_DISABLE_DEFAULT_PLUGINS`    | Disable built-in plugins                |
| `OPENCODE_DISABLE_LSP_DOWNLOAD`       | Prevent auto-downloading LSP servers    |
| `OPENCODE_DISABLE_TERMINAL_TITLE`     | Disable terminal title changes          |
| `OPENCODE_DISABLE_FILETIME_CHECK`     | Disable file modification time checking |
| `OPENCODE_DISABLE_PROJECT_CONFIG`     | Skip loading project-level config       |
| `OPENCODE_DISABLE_EXTERNAL_SKILLS`    | Disable external skills                 |
| `OPENCODE_DISABLE_CLAUDE_CODE`        | Disable Claude Code integration         |
| `OPENCODE_DISABLE_CLAUDE_CODE_PROMPT` | Disable loading ~/.claude/CLAUDE.md     |
| `OPENCODE_DISABLE_CLAUDE_CODE_SKILLS` | Disable Claude Code skills              |
| `OPENCODE_ENABLE_EXPERIMENTAL_MODELS` | Enable experimental models              |
| `OPENCODE_ENABLE_EXA`                 | Enable Exa-powered tools (websearch, codesearch) for non-OpenCode providers |
| `OPENCODE_ENABLE_QUESTION_TOOL`       | Enable the question tool                |

## Experimental

| Variable                                        | Description                      |
| ----------------------------------------------- | -------------------------------- |
| `OPENCODE_EXPERIMENTAL`                         | Enable all experimental features |
| `OPENCODE_EXPERIMENTAL_WORKSPACES`              | Enable workspaces feature        |
| `OPENCODE_EXPERIMENTAL_PLAN_MODE`               | Enable plan mode                 |
| `OPENCODE_EXPERIMENTAL_LSP_TOOL`                | Enable LSP tool                  |
| `OPENCODE_EXPERIMENTAL_FILEWATCHER`             | Enable file watcher              |
| `OPENCODE_EXPERIMENTAL_BASH_DEFAULT_TIMEOUT_MS` | Custom bash timeout (ms)         |
| `OPENCODE_EXPERIMENTAL_OUTPUT_TOKEN_MAX`        | Max output tokens                |

## Server

| Variable                   | Description                                        |
| -------------------------- | -------------------------------------------------- |
| `OPENCODE_SERVER_PASSWORD` | Password for HTTP Basic Auth                       |
| `OPENCODE_SERVER_USERNAME` | Username for HTTP Basic Auth (default: "opencode") |
| `OPENCODE_CLIENT`          | Client identifier (cli, acp)                       |

## System

| Variable | Description                                         |
| -------- | --------------------------------------------------- |
| `SHELL`  | Shell used by the bash tool (fallback: `/bin/bash`) |
| `VISUAL` | Preferred text editor (checked before `EDITOR`)     |
| `EDITOR` | Fallback text editor for `<leader>e`                |

## See Also

- [configuration.md](configuration.md) -- `opencode.json` reference
