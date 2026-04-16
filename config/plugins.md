# Plugins

Plugins extend OpenCode with new hooks, tools, agents, slash commands, and providers. They are JavaScript or TypeScript modules loaded at startup.

## Installation

### From npm or Git

Declare plugins in `opencode.json` under the `plugin` key:

```json
{
  "plugin": [
    "my-opencode-plugin",
    ["my-plugin-with-options", { "key": "value" }],
    "git+https://github.com/user/my-plugin.git",
    "superpowers@git+https://github.com/obra/superpowers.git"
  ]
}
```

Each entry can be:

| Format                           | Description                                      |
| -------------------------------- | ------------------------------------------------ |
| `"package-name"`                 | npm package (installed automatically via Bun)    |
| `["package-name", { options }]`  | npm package with options passed to the plugin    |
| `"git+https://github.com/..."`   | Git repository URL                               |
| `"name@git+https://..."`         | Named alias for a Git repository                 |

### Local plugins

Drop `.ts` or `.js` files into one of these directories — no config entry needed:

| Location                                | Scope   |
| --------------------------------------- | ------- |
| `<project>/.opencode/plugins/*.{ts,js}` | Project |
| `~/.config/opencode/plugins/*.{ts,js}`  | User    |

Local plugins are loaded in alphabetical order within each directory.

### Disable built-in plugins

Set `OPENCODE_DISABLE_DEFAULT_PLUGINS=1` to skip all built-in plugin loading.

---

## Writing a Plugin

A plugin is a default-exported async function:

```typescript
import type { Plugin, PluginInput, Hooks } from "@opencode-ai/plugin"

const MyPlugin: Plugin = async (input: PluginInput): Promise<Hooks> => {
  return {
    // return only the hooks you need
  }
}

export default MyPlugin
```

### PluginInput

| Property                 | Type     | Description                                                               |
| ------------------------ | -------- | ------------------------------------------------------------------------- |
| `client`                 | object   | OpenCode SDK client (typed API access)                                    |
| `project`                | object   | Current project info (`id`, `path`, etc.)                                 |
| `directory`              | string   | Active working directory                                                   |
| `worktree`               | string   | Git worktree root                                                          |
| `serverUrl`              | URL      | URL of the running OpenCode server                                        |
| `$`                      | BunShell | Bun shell (available when running under Bun)                              |
| `experimental_workspace` | object   | Register custom workspace adaptors: `register(type, adaptor)` — experimental |

### Accepting options

When users pass options via `["my-plugin", { "key": "value" }]`, receive them as a second argument:

```typescript
const MyPlugin: Plugin = async (input: PluginInput, options: Record<string, unknown>): Promise<Hooks> => {
  const key = options.key ?? "default"
  return { /* ... */ }
}
```

---

## Common Patterns

### Register a custom slash command

```typescript
"config": async (config) => {
  config.command ??= {}
  config.command["my-command"] = {
    description: "Run my workflow",
    template: "Load and follow the my-workflow skill.",
  }
}
```

### Register a custom agent

```typescript
"config": async (config) => {
  config.agent ??= {}
  config.agent["security-reviewer"] = {
    description: "Reviews code for security vulnerabilities",
    mode: "subagent",
    prompt: "You are a security auditor. Review for OWASP Top 10 issues.",
  }
}
```

### Register a custom tool

```typescript
import { tool } from "@opencode-ai/plugin"

"tool": {
  greet: tool({
    description: "Greet someone by name",
    args: {
      name: tool.schema.string().describe("Name to greet"),
    },
    async execute(args) {
      return `Hello, ${args.name}!`
    },
  }),
}
```

### Inject environment variables into the bash tool

```typescript
"shell.env": async (input, output) => {
  output.env["MY_API_KEY"] = process.env.MY_API_KEY ?? ""
}
```

### Modify the system prompt

```typescript
"experimental.chat.system.transform": async (input, output) => {
  output.system.push("Always respond in bullet points.")
}
```

For the full hook reference, see [hooks.md](hooks.md).

---

## Plugin Load Order

1. Built-in plugins (unless `OPENCODE_DISABLE_DEFAULT_PLUGINS=1`)
2. User-level local plugins (`~/.config/opencode/plugins/`)
3. Project-level local plugins (`.opencode/plugins/`)
4. Plugins declared in `opencode.json` `plugin` array (in order)

Multiple plugins can register the same hook — they run sequentially and each mutates the shared `output` object.

---

## Notable Plugins

| Plugin       | Source                                          | Description                                 |
| ------------ | ----------------------------------------------- | ------------------------------------------- |
| `superpowers`| `git+https://github.com/obra/superpowers.git`   | 14 agentic workflow skills (plan, TDD, etc.) |

See [superpowers.md](superpowers.md) for setup and skill list.

## See Also

- [hooks.md](hooks.md) — Full hook reference
- [configuration.md](configuration.md) — `plugin` config key
- [../skills/README.md](../skills/README.md) — Skill system
- [../commands/custom-commands.md](../commands/custom-commands.md) — Custom slash commands
