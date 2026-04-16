# Configuration

OpenCode uses `opencode.json` (or `opencode.jsonc`) configuration files.

## File Locations

Config files are merged in order (later overrides earlier):

| Location                            | Scope   |
| ----------------------------------- | ------- |
| `~/.config/opencode/opencode.json`  | Global  |
| `opencode.json` (project directory) | Project |
| `.opencode/opencode.json`           | Project |

TUI settings are in a **separate file**: `tui.json` (same directories).

A JSON schema is available:
```json
{ "$schema": "https://opencode.ai/config.json" }
```

## Top-Level Options

| Key                  | Type                              | Description                                                             |
| -------------------- | --------------------------------- | ----------------------------------------------------------------------- |
| `model`              | string                            | Default model (`provider/model` format)                                 |
| `small_model`        | string                            | Small model for title generation                                        |
| `default_agent`      | string                            | Default agent name                                                      |
| `username`           | string                            | Custom display username                                                 |
| `logLevel`           | string                            | Log level: `DEBUG`, `INFO`, `WARN`, `ERROR`                             |
| `snapshot`           | boolean                           | Enable snapshot tracking (default: `true`). When `false`, undo/revert do not restore file changes. |
| `share`              | `"manual"` \| `"auto"` \| `"disabled"` | Sharing behavior (see [Share](#share))                             |
| `autoupdate`         | boolean \| `"notify"`             | Auto-update: `true` = update silently, `false` = disable, `"notify"` = prompt |
| `disabled_providers` | string[]                          | Disable auto-loaded providers by ID                                     |
| `enabled_providers`  | string[]                          | Whitelist-only providers — all others are ignored                       |
| `instructions`       | string[]                          | Additional instruction files, globs, or URLs (merged with `AGENTS.md`) |

## Compaction

Controls how OpenCode handles context window limits.

| Key                   | Type    | Default | Description                                                               |
| --------------------- | ------- | ------- | ------------------------------------------------------------------------- |
| `compaction.auto`     | boolean | `true`  | Auto-compact when context is full                                         |
| `compaction.prune`    | boolean | `true`  | Prune old tool outputs to reclaim tokens before triggering full compaction |
| `compaction.reserved` | number  |         | Token buffer reserved during compaction to avoid overflow mid-summary     |

## Share

Controls whether sessions can be shared as public URLs.

| Value        | Behavior                                                          |
| ------------ | ----------------------------------------------------------------- |
| `"manual"`   | Sharing allowed via `/share` command or `opencode run --share`    |
| `"auto"`     | Every new session is automatically shared on creation             |
| `"disabled"` | Sharing disabled entirely — `/share` command returns an error     |

```json
{ "share": "manual" }
```

Can also be set via environment variable: `OPENCODE_AUTO_SHARE=1` enables auto-share.

## Instructions (Context Files)

The `instructions` field accepts file paths, globs, and URLs. These are merged into the agent's context alongside the default `AGENTS.md` files.

Default instruction files loaded automatically:
- `AGENTS.md` (project root)
- `~/.config/opencode/AGENTS.md` (global)

```json
{
  "instructions": [
    "docs/architecture.md",
    ".opencode/context/*.md",
    "https://example.com/context.md"
  ]
}
```

## Providers

See [providers.md](providers.md) for the full provider configuration reference.

## Agents

See [agents.md](agents.md) for the agent role configuration reference.

## MCP Servers

Configure MCP servers under the `mcp` key. Two types: `local` and `remote`.

See [mcp/context7.md](../mcp/context7.md) and [mcp/chrome-devtools.md](../mcp/chrome-devtools.md) for detailed setup guides.

```json
{
  "mcp": {
    "context7": {
      "type": "remote",
      "url": "https://mcp.context7.com/mcp"
    },
    "chrome-devtools": {
      "type": "local",
      "command": ["npx", "-y", "chrome-devtools-mcp@latest"]
    }
  }
}
```

### Local (stdio)

```json
{
  "mcp": {
    "my-server": {
      "type": "local",
      "command": ["npx", "-y", "my-mcp-server@latest"],
      "environment": { "KEY": "value" },
      "timeout": 30000
    }
  }
}
```

| Key                      | Type                  | Description                      |
| ------------------------ | --------------------- | -------------------------------- |
| `mcp.<name>.type`        | `"local"`             | Local MCP server (stdio)         |
| `mcp.<name>.command`     | string[]              | Command and arguments (as array) |
| `mcp.<name>.environment` | Record<string,string> | Environment variables            |
| `mcp.<name>.enabled`     | boolean               | Enable/disable this server       |
| `mcp.<name>.timeout`     | number                | Timeout in milliseconds          |

### Remote (SSE)

```json
{
  "mcp": {
    "remote-server": {
      "type": "remote",
      "url": "https://mcp.example.com/mcp",
      "headers": { "Authorization": "Bearer ..." }
    }
  }
}
```

| Key                  | Type                  | Description             |
| -------------------- | --------------------- | ----------------------- |
| `mcp.<name>.type`    | `"remote"`            | Remote MCP server (SSE) |
| `mcp.<name>.url`     | string                | Server URL              |
| `mcp.<name>.headers` | Record<string,string> | HTTP headers            |
| `mcp.<name>.enabled` | boolean               | Enable/disable          |
| `mcp.<name>.timeout` | number                | Timeout in milliseconds |

## LSP (Language Server Protocol)

OpenCode includes 37 built-in LSP servers (21 auto-installed). See [lsp/](../lsp/) for the full list.

Set `lsp` to `false` to disable all LSP servers, or configure per-language:

| Key                         | Type                  | Description                                               |
| --------------------------- | --------------------- | --------------------------------------------------------- |
| `lsp.<lang>.command`        | string[]              | Command and arguments (as array)                          |
| `lsp.<lang>.extensions`     | string[]              | File extensions to associate (required for custom servers)|
| `lsp.<lang>.disabled`       | boolean               | Disable this LSP                                          |
| `lsp.<lang>.env`            | Record<string,string> | Environment variables                                     |
| `lsp.<lang>.initialization` | object                | Initialization options                                    |

## Formatter

Override or disable the code formatter for specific languages. Set `formatter` to `false` to disable all formatting.

```json
{
  "formatter": {
    "typescript": {
      "command": ["prettier", "--write", "$FILE"],
      "extensions": [".ts", ".tsx"],
      "environment": { "NODE_ENV": "development" }
    },
    "python": {
      "disabled": true
    }
  }
}
```

| Key                              | Type                  | Description                                        |
| -------------------------------- | --------------------- | -------------------------------------------------- |
| `formatter.<name>.command`       | string[]              | Formatter command (use `$FILE` for the file path)  |
| `formatter.<name>.extensions`    | string[]              | File extensions this formatter applies to          |
| `formatter.<name>.environment`   | Record<string,string> | Environment variables for the formatter process    |
| `formatter.<name>.disabled`      | boolean               | Disable this formatter                             |

## Server

Persistent server configuration (applies to `opencode serve`, `opencode web`, and `opencode acp`). Command-line flags override these values.

```json
{
  "server": {
    "port": 4096,
    "hostname": "127.0.0.1",
    "mdns": false,
    "mdnsDomain": "opencode.local",
    "cors": []
  }
}
```

| Key                  | Type     | Default            | Description                                                   |
| -------------------- | -------- | ------------------ | ------------------------------------------------------------- |
| `server.port`        | number   | `0` (random)       | Port to listen on                                             |
| `server.hostname`    | string   | `"127.0.0.1"`      | Hostname to listen on                                         |
| `server.mdns`        | boolean  | `false`            | Enable mDNS service discovery (forces hostname to `0.0.0.0`) |
| `server.mdnsDomain`  | string   | `"opencode.local"` | Custom mDNS domain name                                       |
| `server.cors`        | string[] | `[]`               | Additional origins allowed for CORS                           |

## Skills

| Key            | Type     | Description                  |
| -------------- | -------- | ---------------------------- |
| `skills.paths` | string[] | Additional skill directories |
| `skills.urls`  | string[] | Remote skill index URLs      |

## Watcher

| Key              | Type     | Description                                                       |
| ---------------- | -------- | ----------------------------------------------------------------- |
| `watcher.ignore` | string[] | File patterns to ignore in the file watcher (glob syntax)        |

## Plugins

```json
{
  "plugin": ["superpowers@git+https://github.com/obra/superpowers.git"]
}
```

See [plugins.md](plugins.md) for installation and authoring details.

## Experimental

Advanced options that may change or be removed in future versions.

```json
{
  "experimental": {
    "batch_tool": false,
    "openTelemetry": false,
    "primary_tools": ["bash", "edit"],
    "continue_loop_on_deny": false,
    "mcp_timeout": 5000,
    "disable_paste_summary": false
  }
}
```

| Key                                     | Type     | Description                                                              |
| --------------------------------------- | -------- | ------------------------------------------------------------------------ |
| `experimental.batch_tool`               | boolean  | Enable the batch tool (run multiple tools in one call)                   |
| `experimental.openTelemetry`            | boolean  | Enable OpenTelemetry spans for AI SDK calls                              |
| `experimental.primary_tools`            | string[] | Tools restricted to primary agents only (not available to subagents)     |
| `experimental.continue_loop_on_deny`    | boolean  | Continue the agent loop when a tool call is denied (instead of stopping) |
| `experimental.mcp_timeout`              | number   | Timeout in milliseconds for MCP requests (default: `5000`)               |
| `experimental.disable_paste_summary`    | boolean  | Disable the summary shown when pasting large content                     |

## TUI Configuration (tui.json)

TUI settings live in a **separate file** (`tui.json` or `tui.jsonc`):

```json
{
  "theme": "catppuccin",
  "scroll_speed": 3,
  "scroll_acceleration": { "enabled": true },
  "mouse": true,
  "diff_style": "stacked",
  "keybinds": {
    "session_new": "ctrl+n"
  },
  "plugin_enabled": {
    "my-tui-plugin": false
  }
}
```

| Key                         | Type    | Default      | Description                                              |
| --------------------------- | ------- | ------------ | -------------------------------------------------------- |
| `theme`                     | string  | `"opencode"` | UI color theme                                           |
| `scroll_speed`              | number  |              | Scroll speed multiplier                                  |
| `scroll_acceleration`       | object  |              | Scroll acceleration: `{ "enabled": true/false }`        |
| `mouse`                     | boolean | `true`       | Enable or disable mouse capture                          |
| `diff_style`                | string  | `"auto"`     | Diff display: `"auto"` (adapts to terminal width) or `"stacked"` (single column) |
| `keybinds`                  | object  |              | Keybind overrides (see [global-shortcuts.md](../shortcuts/global-shortcuts.md)) |
| `plugin`                    | array   |              | TUI-specific plugins to load                             |
| `plugin_enabled`            | object  |              | Per-plugin enable/disable: `{ "<plugin-id>": false }`   |

## Variable Substitution

Use `{env:VAR}` to inject environment variables or `{file:path}` to read from a file anywhere in the config:

```json
{
  "provider": {
    "anthropic": {
      "options": {
        "apiKey": "{env:ANTHROPIC_API_KEY}"
      }
    },
    "openai": {
      "options": {
        "apiKey": "{file:~/.secrets/openai-key}"
      }
    }
  }
}
```

- `{env:VAR}` — replaced with the value of the environment variable (empty string if unset)
- `{file:path}` — replaced with the file contents (path can be relative to the config directory, absolute, or `~/`)

## Example Configuration

### `opencode.json` (project root)

```json
{
  "$schema": "https://opencode.ai/config.json",
  "model": "provider/model-name",
  "agent": {
    "build": {
      "steps": 100
    }
  },
  "mcp": {
    "context7": {
      "type": "local",
      "command": ["npx", "-y", "@upstash/context7-mcp"]
    }
  },
  "lsp": {
    "typescript": {
      "command": ["typescript-language-server", "--stdio"],
      "extensions": [".ts", ".tsx"]
    }
  },
  "instructions": ["docs/architecture.md"]
}
```

### `~/.config/opencode/opencode.json` (global)

```json
{
  "$schema": "https://opencode.ai/config.json",
  "plugin": ["superpowers@git+https://github.com/obra/superpowers.git"],
  "provider": {
    "my-provider": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "My Provider",
      "options": {
        "baseURL": "http://<url>:<port>/v1"
      },
      "models": {
        "org/model-name": {
          "name": "Model Display Name",
          "modalities": {
            "input": ["text", "image"],
            "output": ["text"]
          },
          "limit": {
            "context": 262144,
            "output": 16384
          }
        }
      }
    }
  }
}
```

> **Important**: Replace `<url>:<port>` with the actual address of your inference server.
