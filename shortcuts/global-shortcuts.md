# Global Shortcuts

OpenCode uses a **leader key** system. The leader key defaults to `Ctrl+X`. Chord shortcuts are written as `<leader>KEY` meaning: press `Ctrl+X`, release, then press `KEY` within 2 seconds.

## Application

| Shortcut                          | Action                    |
| --------------------------------- | ------------------------- |
| `Ctrl+C` / `Ctrl+D` / `<leader>q` | Quit                      |
| `Ctrl+P`                          | Open command palette      |
| `Ctrl+Z`                          | Suspend terminal          |
| `Escape`                          | Interrupt current session |

## Session Management

| Shortcut    | Action                    |
| ----------- | ------------------------- |
| `<leader>n` | New session               |
| `<leader>l` | List / switch sessions    |
| `<leader>g` | Session timeline          |
| `<leader>c` | Compact / summarize       |
| `<leader>x` | Export session transcript |

## Model & Agent Selection

| Shortcut    | Action                         |
| ----------- | ------------------------------ |
| `<leader>m` | List available models          |
| `<leader>a` | List available agents          |
| `F2`        | Cycle to next recent model     |
| `Shift+F2`  | Cycle to previous recent model |
| `Tab`       | Cycle to next agent            |
| `Shift+Tab` | Cycle to previous agent        |
| `Ctrl+T`    | Cycle model variants           |

## UI Toggles

| Shortcut    | Action               |
| ----------- | -------------------- |
| `<leader>t` | List themes          |
| `<leader>b` | Toggle sidebar       |
| `<leader>s` | View status          |
| `<leader>h` | Toggle tips / help   |
| `<leader>e` | Open external editor |

## Message Actions

| Shortcut    | Action                      |
| ----------- | --------------------------- |
| `<leader>u` | Undo (revert to previous)   |
| `<leader>r` | Redo undone message         |
| `<leader>y` | Copy last assistant message |

## Message Navigation

| Shortcut                  | Action                |
| ------------------------- | --------------------- |
| `PageUp` / `Ctrl+Alt+B`   | Scroll one page up    |
| `PageDown` / `Ctrl+Alt+F` | Scroll one page down  |
| `Ctrl+Alt+U`              | Scroll half page up   |
| `Ctrl+Alt+D`              | Scroll half page down |
| `Ctrl+Alt+Y`              | Scroll one line up    |
| `Ctrl+Alt+E`              | Scroll one line down  |
| `Ctrl+G` / `Home`         | Go to first message   |
| `Ctrl+Alt+G` / `End`      | Go to last message    |

## Subagent Navigation

| Shortcut       | Action                    |
| -------------- | ------------------------- |
| `<leader>Down` | Go to first child session |
| `Right`        | Next child session        |
| `Left`         | Previous child session    |
| `Up`           | Go to parent session      |

## Customization

All bindings can be overridden in `tui.json`:

```json
{
  "keybinds": {
    "session_new": "ctrl+n",
    "command_list": "ctrl+k"
  }
}
```

Use `"none"` to disable a binding.
