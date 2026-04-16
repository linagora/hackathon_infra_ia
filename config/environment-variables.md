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

## Updates

| Variable                         | Description                                                                 |
| -------------------------------- | --------------------------------------------------------------------------- |
| `OPENCODE_DISABLE_AUTOUPDATE`    | Disable automatic updates                                                   |
| `OPENCODE_ALWAYS_NOTIFY_UPDATE`  | Always show update notification even when `autoupdate` is `true`            |

## Models

| Variable                      | Description                                            |
| ----------------------------- | ------------------------------------------------------ |
| `OPENCODE_DISABLE_MODELS_FETCH` | Disable fetching models from models.dev              |
| `OPENCODE_MODELS_URL`         | Custom URL for the models registry (overrides default) |
| `OPENCODE_MODELS_PATH`        | Path to a local models JSON file                       |
| `OPENCODE_ENABLE_EXPERIMENTAL_MODELS` | Enable experimental/preview models             |

## Feature Toggles

| Variable                              | Description                                                                 |
| ------------------------------------- | --------------------------------------------------------------------------- |
| `OPENCODE_DISABLE_AUTOCOMPACT`        | Disable automatic compaction                                                |
| `OPENCODE_DISABLE_PRUNE`              | Disable session pruning                                                     |
| `OPENCODE_DISABLE_DEFAULT_PLUGINS`    | Disable built-in plugins                                                    |
| `OPENCODE_DISABLE_LSP_DOWNLOAD`       | Prevent auto-downloading LSP servers                                        |
| `OPENCODE_DISABLE_TERMINAL_TITLE`     | Disable terminal title changes                                              |
| `OPENCODE_DISABLE_FILETIME_CHECK`     | Disable file modification time checking                                     |
| `OPENCODE_DISABLE_PROJECT_CONFIG`     | Skip loading project-level config                                           |
| `OPENCODE_DISABLE_EXTERNAL_SKILLS`    | Disable external skill directory scanning (`.claude/`, `.agents/`)          |
| `OPENCODE_DISABLE_CLAUDE_CODE`        | Disable Claude Code integration                                             |
| `OPENCODE_DISABLE_CLAUDE_CODE_PROMPT` | Disable loading `~/.claude/CLAUDE.md`                                       |
| `OPENCODE_DISABLE_CLAUDE_CODE_SKILLS` | Disable scanning `.claude/` skill directories (keeps `.agents/`)            |
| `OPENCODE_DISABLE_MOUSE`              | Disable mouse capture in the TUI (overrides `tui.json` `mouse: true`)       |
| `OPENCODE_DISABLE_EMBEDDED_WEB_UI`    | Disable the embedded web UI served by `opencode web`                        |
| `OPENCODE_ENABLE_EXA`                 | Enable Exa-powered websearch and codesearch for non-OpenCode providers      |
| `OPENCODE_ENABLE_QUESTION_TOOL`       | Enable the `question` tool (agent can ask user questions mid-task)          |
| `OPENCODE_SKIP_MIGRATIONS`            | Skip database schema migrations on startup                                  |
| `OPENCODE_STRICT_CONFIG_DEPS`         | Abort on unresolvable config dependencies instead of silently ignoring them |

## Share

| Variable               | Description                                                         |
| ---------------------- | ------------------------------------------------------------------- |
| `OPENCODE_AUTO_SHARE`  | Set to `1` to enable auto-share (equivalent to `"share": "auto"` in config) |

## Git

| Variable                 | Description                                                         |
| ------------------------ | ------------------------------------------------------------------- |
| `OPENCODE_GIT_BASH_PATH` | Path to `git-bash.exe` on Windows (used when `bash` is unavailable) |

## Experimental

| Variable                                        | Description                                                         |
| ----------------------------------------------- | ------------------------------------------------------------------- |
| `OPENCODE_EXPERIMENTAL`                         | Enable all experimental features at once                            |
| `OPENCODE_EXPERIMENTAL_WORKSPACES`              | Enable workspaces feature                                           |
| `OPENCODE_EXPERIMENTAL_PLAN_MODE`               | Enable plan mode                                                    |
| `OPENCODE_EXPERIMENTAL_LSP_TOOL`                | Enable the LSP tool (exposes language server capabilities to agent) |
| `OPENCODE_EXPERIMENTAL_LSP_TY`                  | Enable Ty (experimental Rust-based Python type checker) as LSP      |
| `OPENCODE_EXPERIMENTAL_FILEWATCHER`             | Enable file watcher                                                 |
| `OPENCODE_EXPERIMENTAL_MARKDOWN`                | Enable experimental Markdown rendering in TUI                       |
| `OPENCODE_EXPERIMENTAL_HTTPAPI`                 | Enable the experimental HTTP API (REST endpoints)                   |
| `OPENCODE_EXPERIMENTAL_BASH_DEFAULT_TIMEOUT_MS` | Override default bash tool timeout in milliseconds                  |
| `OPENCODE_EXPERIMENTAL_OUTPUT_TOKEN_MAX`        | Override maximum output tokens for generation                       |

## OpenTelemetry

| Variable                      | Description                                                              |
| ----------------------------- | ------------------------------------------------------------------------ |
| `OTEL_EXPORTER_OTLP_ENDPOINT` | OTLP endpoint URL for exporting OpenTelemetry spans                      |
| `OTEL_EXPORTER_OTLP_HEADERS`  | HTTP headers for OTLP exporter (e.g. `Authorization=Bearer <token>`)     |

> Requires `experimental.openTelemetry: true` in `opencode.json` to enable span collection.

## Server

| Variable                   | Description                                        |
| -------------------------- | -------------------------------------------------- |
| `OPENCODE_SERVER_PASSWORD` | Password for HTTP Basic Auth                       |
| `OPENCODE_SERVER_USERNAME` | Username for HTTP Basic Auth (default: "opencode") |
| `OPENCODE_CLIENT`          | Client identifier (`cli`, `acp`)                   |

## System

| Variable | Description                                         |
| -------- | --------------------------------------------------- |
| `SHELL`  | Shell used by the bash tool (fallback: `/bin/bash`) |
| `VISUAL` | Preferred text editor (checked before `EDITOR`)     |
| `EDITOR` | Fallback text editor for `<leader>e`                |

## See Also

- [configuration.md](configuration.md) -- `opencode.json` reference
