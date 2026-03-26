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

| Key                  | Type     | Description                                    |
| -------------------- | -------- | ---------------------------------------------- |
| `model`              | string   | Default model (`provider/model` format)        |
| `small_model`        | string   | Small model for title generation               |
| `default_agent`      | string   | Default agent name                             |
| `username`           | string   | Custom display username                        |
| `logLevel`           | string   | Log level (DEBUG, INFO, WARN, ERROR)           |
| `snapshot`           | boolean  | Enable/disable snapshot tracking               |
| `share`              | string   | Sharing behavior: `manual`, `auto`, `disabled` |
| `autoupdate`         | boolean  | Auto-update behavior (or `"notify"`)           |
| `disabled_providers` | string[] | Disable auto-loaded providers                  |
| `enabled_providers`  | string[] | Whitelist-only providers                       |
| `instructions`       | string[] | Additional instruction files, globs, or URLs   |

## Compaction

| Key                   | Type    | Default | Description                       |
| --------------------- | ------- | ------- | --------------------------------- |
| `compaction.auto`     | boolean | `true`  | Auto-compact when context is full |
| `compaction.prune`    | boolean |         | Prune old messages                |
| `compaction.reserved` | number  |         | Reserved tokens                   |

## Instructions (Context Files)

The `instructions` field replaces the old `contextPaths`. Accepts file paths, globs, and URLs.

Default instruction files loaded automatically:
- `AGENTS.md` (project root)
- `~/.config/opencode/AGENTS.md` (global)

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

OpenCode includes 37 built-in LSP servers (21 auto-installed). See [lsp/built-in.md](../lsp/built-in.md) for the full list.

Set `lsp` to `false` to disable all LSP servers, or configure per-language:

| Key                         | Type                  | Description                      |
| --------------------------- | --------------------- | -------------------------------- |
| `lsp.<lang>.command`        | string[]              | Command and arguments (as array) |
| `lsp.<lang>.extensions`     | string[]              | File extensions to associate     |
| `lsp.<lang>.disabled`       | boolean               | Disable this LSP                 |
| `lsp.<lang>.env`            | Record<string,string> | Environment variables            |
| `lsp.<lang>.initialization` | object                | Initialization options           |

## Skills

| Key            | Type     | Description                  |
| -------------- | -------- | ---------------------------- |
| `skills.paths` | string[] | Additional skill directories |
| `skills.urls`  | string[] | Remote skill index URLs      |

## Plugins

```json
{
  "plugin": ["superpowers@git+https://github.com/obra/superpowers.git"]
}
```

See [superpowers.md](superpowers.md) for the Superpowers plugin guide.

## TUI Configuration (tui.json)

TUI settings live in a **separate file** (`tui.json` or `tui.jsonc`):

```json
{
  "theme": "catppuccin",
  "scroll_speed": 3,
  "diff_style": "stacked",
  "keybinds": {
    "session_new": "ctrl+n"
  }
}
```

| Key            | Type   | Default      | Description                     |
| -------------- | ------ | ------------ | ------------------------------- |
| `theme`        | string | `"opencode"` | UI color theme                  |
| `keybinds`     | object |              | Keybind overrides               |
| `scroll_speed` | number |              | Scroll speed                    |
| `diff_style`   | string |              | Diff display: `auto`, `stacked` |

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
