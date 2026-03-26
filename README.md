# OpenCode - Command Reference

[OpenCode](https://opencode.ai) is a terminal-based AI coding assistant (TUI).

## Quick Setup

### 1. Install

```bash
curl -fsSL https://opencode.ai/install | bash                          # macOS, Linux, Windows (MSYS)
curl -fsSL https://opencode.ai/install | bash -s -- --version 1.0.180  # Specific version
```

Other methods: `npm install -g opencode-ai`, `brew install anomalyco/tap/opencode`, or see [opencode.ai](https://opencode.ai).

Installs to `~/.opencode/bin/` and adds it to your PATH (`.zshrc`, `.bashrc`, etc.). Use `--no-modify-path` to skip PATH modification.

| OS      | x64 | arm64 | Install methods                       |
| ------- | --- | ----- | ------------------------------------- |
| macOS   | yes | yes   | curl, brew, npm, bun                  |
| Linux   | yes | yes   | curl, npm, bun, pacman (Arch)         |
| Windows | yes | --    | curl (MSYS/Cygwin), npm, choco, scoop |

### 2. Configure a [provider](config/providers.md)

Create [`~/.config/opencode/opencode.json`](config/configuration.md):

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "my-provider": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "My Provider",
      "options": {
        "baseURL": "http://<url>:<port>/v1"
      },
      "models": {
        "model-id": {
          "name": "Model Name",
          "limit": { "context": 262144, "output": 16384 }
        }
      }
    }
  }
}
```

> Replace `<url>:<port>` with your inference server address. See [providers.md](config/providers.md).

### 3. Install [Superpowers](config/superpowers.md) (optional)

Add to your `opencode.json`:

```json
{
  "plugin": ["superpowers@git+https://github.com/obra/superpowers.git"]
}
```

See [superpowers.md](config/superpowers.md) for details.

### 4. Launch and initialize

```bash
cd /path/to/your/project
opencode
```

Inside OpenCode, run [`/init`](commands/init.md) to generate an [AGENTS.md](config/agents-md.md) for your project.

> **Note**: By default the agent runs tools without confirmation. Add `"permission": "ask"` to your `opencode.json` to require approval before shell commands and file edits. See [permissions.md](config/permissions.md).

> **Tip**: You can ask the agent questions about this documentation by giving it the repo URL. Example: `"Based on https://github.com/linagora/hackathon_infra_ia, which MCP servers are listed?"`

## Usage

```bash
opencode                                                            # Launch in interactive mode
opencode run "Explain this file"                                    # Single prompt
opencode run -m "provider/model-name" "Fix the bug"                 # Specific model
opencode run -c "Continue where we left off"                        # Continue last session
```

## Best Practices

- [Git worktrees](best-practices/git-worktrees.md) -- Isolate feature work in parallel workspaces

## Configuration

| Topic                 | Description                                       | Documentation                                               |
| --------------------- | ------------------------------------------------- | ----------------------------------------------------------- |
| Configuration file    | `opencode.json` format, locations, all keys       | [configuration.md](config/configuration.md)                 |
| Storage & persistence | SQLite, sessions, compaction, cross-session data  | [storage.md](config/storage.md)                             |
| AGENTS.md             | Project instructions for the agent                | [agents-md.md](config/agents-md.md)                         |
| Providers             | Custom LLM providers setup                        | [providers.md](config/providers.md)                         |
| Agents                | Internal agent roles (build, plan, explore, etc.) | [agents.md](config/agents.md)                               |
| Environment variables | System vars, feature toggles, debug               | [environment-variables.md](config/environment-variables.md) |
| Superpowers           | Agentic workflow plugin (brainstorm, plan, TDD)   | [superpowers.md](config/superpowers.md)                     |

## CLI Flags

See [cli-flags.md](commands/cli-flags.md) for the full reference (`opencode run [message..] --model --continue --format --file ...`).

## Slash Commands

Available via the command palette (`Ctrl+P`) or by typing `/` in the input.

| Command     | Description             | Documentation                                               |
| ----------- | ----------------------- | ----------------------------------------------------------- |
| `/new`      | New session             | [command-palette.md](commands/command-palette.md)           |
| `/sessions` | Switch sessions         | [command-palette.md](commands/command-palette.md)           |
| `/models`   | Switch model            | [command-palette.md](commands/command-palette.md)           |
| `/compact`  | Compact session         | [compact.md](commands/compact.md)                           |
| `/skills`   | Browse skills           | [command-palette.md](commands/command-palette.md)           |
| `/init`     | Create/update AGENTS.md | [init.md](commands/init.md)                                 |
| ...         | + 15 more commands      | [slash-commands-extra.md](commands/slash-commands-extra.md) |

## Customization

### Custom Commands

`.md` files sent as prompts, with argument support. See [custom-commands.md](commands/custom-commands.md).

| Location                        | Description            |
| ------------------------------- | ---------------------- |
| `~/.config/opencode/commands/`  | User-level commands    |
| `<project>/.opencode/commands/` | Project-level commands |

### Custom Skills

`SKILL.md` files with YAML frontmatter, loaded automatically by the agent. See [configuration.md](config/configuration.md) (`skills` key).

| Location                      | Description          |
| ----------------------------- | -------------------- |
| `~/.config/opencode/skills/`  | User-level skills    |
| `<project>/.opencode/skills/` | Project-level skills |

### Custom Providers

OpenAI-compatible providers with custom models. See [providers.md](config/providers.md).

### MCP Servers

External tool servers connected via the [Model Context Protocol](https://modelcontextprotocol.io/).

| Server                                    | Description                                       |
| ----------------------------------------- | ------------------------------------------------- |
| [context7](mcp/context7.md)               | Up-to-date documentation for any library          |
| [chrome-devtools](mcp/chrome-devtools.md) | Browser automation, debugging, performance audits |

### LSP Servers

37 built-in language servers (21 auto-installed). See [lsp/](lsp/) for the full list.

### Hooks

Intercept and modify agent behavior (system prompt, tool execution, shell env, etc.). See [hooks.md](config/hooks.md).

### Plugins

Extend OpenCode with third-party plugins installed via `opencode.json`. See [Superpowers](config/superpowers.md) -- agentic workflow (brainstorm, design, plan, TDD) with [14 skill guides](skills/superpowers/).

## Keyboard Shortcuts

The leader key defaults to `Ctrl+X` (configurable in `tui.json`). `<leader>KEY` means press `Ctrl+X`, release, then press `KEY`.

Full reference: [global-shortcuts.md](shortcuts/global-shortcuts.md)

### Application

| Shortcut                          | Action            | Documentation                                        |
| --------------------------------- | ----------------- | ---------------------------------------------------- |
| `Ctrl+C` / `Ctrl+D` / `<leader>q` | Quit              | [quit-dialog.md](shortcuts/quit-dialog.md)           |
| `Ctrl+P`                          | Command palette   | [command-palette.md](commands/command-palette.md)    |
| `Escape`                          | Interrupt session | [global-shortcuts.md](shortcuts/global-shortcuts.md) |

### Session & Navigation

| Shortcut    | Action                 | Documentation                                    |
| ----------- | ---------------------- | ------------------------------------------------ |
| `<leader>n` | New session            | [chat.md](shortcuts/chat.md)                     |
| `<leader>l` | List / switch sessions | [session-dialog.md](shortcuts/session-dialog.md) |
| `<leader>c` | Compact session        | [compact.md](commands/compact.md)                |
| `<leader>u` | Undo message           | [chat.md](shortcuts/chat.md)                     |
| `<leader>r` | Redo message           | [chat.md](shortcuts/chat.md)                     |

### Model & Agent

| Shortcut    | Action              | Documentation                                        |
| ----------- | ------------------- | ---------------------------------------------------- |
| `<leader>m` | List models         | [model-dialog.md](shortcuts/model-dialog.md)         |
| `<leader>a` | List agents         | [global-shortcuts.md](shortcuts/global-shortcuts.md) |
| `F2`        | Cycle recent models | [model-dialog.md](shortcuts/model-dialog.md)         |
| `Tab`       | Cycle agents        | [global-shortcuts.md](shortcuts/global-shortcuts.md) |

### UI & Editor

| Shortcut    | Action               | Documentation                                        |
| ----------- | -------------------- | ---------------------------------------------------- |
| `<leader>t` | List themes          | [theme-dialog.md](shortcuts/theme-dialog.md)         |
| `<leader>e` | Open external editor | [editor.md](shortcuts/editor.md)                     |
| `<leader>b` | Toggle sidebar       | [global-shortcuts.md](shortcuts/global-shortcuts.md) |
| `<leader>s` | View status          | [global-shortcuts.md](shortcuts/global-shortcuts.md) |

### Input

| Shortcut                                                 | Action         | Documentation                    |
| -------------------------------------------------------- | -------------- | -------------------------------- |
| `Return`                                                 | Submit message | [editor.md](shortcuts/editor.md) |
| `Shift+Return` / `Ctrl+Return` / `Alt+Return` / `Ctrl+J` | Insert newline | [editor.md](shortcuts/editor.md) |
