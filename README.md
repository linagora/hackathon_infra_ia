# OpenCode - Command Reference

[OpenCode](https://opencode.ai) is a terminal-based AI coding assistant (TUI).

## Usage

```bash
opencode                                                            # Launch in interactive mode
opencode run "Explain this file"                                    # Single prompt
opencode run -m "provider/model-name" "Fix the bug"                 # Specific model
opencode run -c "Continue where we left off"                        # Continue last session
```

## Configuration

| Topic                 | Description                                       | Documentation                                               |
| --------------------- | ------------------------------------------------- | ----------------------------------------------------------- |
| Configuration file    | `opencode.json` format, locations, all keys       | [configuration.md](config/configuration.md)                 |
| AGENTS.md             | Project instructions for the agent                | [agents-md.md](config/agents-md.md)                         |
| Providers             | Custom LLM providers setup                        | [providers.md](config/providers.md)                         |
| Agents                | Internal agent roles (build, plan, explore, etc.) | [agents.md](config/agents.md)                               |
| Environment variables | System vars, feature toggles, debug               | [environment-variables.md](config/environment-variables.md) |
| Superpowers           | Agentic workflow plugin (brainstorm, plan, TDD)   | [superpowers.md](config/superpowers.md)                     |

## CLI Flags

See [cli-flags.md](commands/cli-flags.md) for the full reference (`opencode run [message..] --model --continue --format --file ...`).

## Slash Commands

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

| Server                                    | Description                                       |
| ----------------------------------------- | ------------------------------------------------- |
| [context7](mcp/context7.md)               | Up-to-date documentation for any library          |
| [chrome-devtools](mcp/chrome-devtools.md) | Browser automation, debugging, performance audits |

### LSP Servers

37 built-in language servers (21 auto-installed). See [lsp/built-in.md](lsp/built-in.md) for the full list.

### Hooks

Plugin hooks to intercept and modify behavior (system prompt, tool execution, shell env, etc.). See [hooks.md](config/hooks.md).

### Plugins

#### Superpowers

[Superpowers](https://github.com/obra/superpowers) forces a disciplined pipeline: brainstorm, design, plan, then implement with TDD. See [installation and overview](config/superpowers.md), or browse the [14 skill guides](skills/superpowers/).

| Skill                                                                                  | Description                                   |
| -------------------------------------------------------------------------------------- | --------------------------------------------- |
| [brainstorming](skills/superpowers/brainstorming.md)                                   | Explore ideas before implementation           |
| [writing-plans](skills/superpowers/writing-plans.md)                                   | Create bite-sized implementation plans        |
| [executing-plans](skills/superpowers/executing-plans.md)                               | Execute plans task by task                    |
| [test-driven-development](skills/superpowers/test-driven-development.md)               | RED-GREEN-REFACTOR cycle                      |
| [systematic-debugging](skills/superpowers/systematic-debugging.md)                     | 4-phase root cause analysis                   |
| [subagent-driven-development](skills/superpowers/subagent-driven-development.md)       | Fresh subagent per task with two-stage review |
| [dispatching-parallel-agents](skills/superpowers/dispatching-parallel-agents.md)       | Concurrent agent tasks                        |
| [using-git-worktrees](skills/superpowers/using-git-worktrees.md)                       | Isolated workspaces for feature work          |
| [finishing-a-development-branch](skills/superpowers/finishing-a-development-branch.md) | Branch integration options                    |
| [requesting-code-review](skills/superpowers/requesting-code-review.md)                 | Structured review requests                    |
| [receiving-code-review](skills/superpowers/receiving-code-review.md)                   | Handling review feedback with rigor           |
| [verification-before-completion](skills/superpowers/verification-before-completion.md) | Evidence before claims                        |
| [using-superpowers](skills/superpowers/using-superpowers.md)                           | Meta-skill: skill discovery and invocation    |
| [writing-skills](skills/superpowers/writing-skills.md)                                 | How to write new custom skills                |

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
