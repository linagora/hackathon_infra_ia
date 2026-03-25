# Hooks

OpenCode plugins can register hooks to intercept and modify behavior at various points.

## Available Hooks

### Chat Hooks

| Hook                                   | Description                                         |
| -------------------------------------- | --------------------------------------------------- |
| `chat.message`                         | Modify user message and parts before processing     |
| `chat.params`                          | Modify LLM parameters (temperature, topP, topK)     |
| `chat.headers`                         | Inject custom HTTP headers into LLM requests        |
| `experimental.chat.system.transform`   | Modify the system prompt array before sending       |
| `experimental.chat.messages.transform` | Transform the entire message history before sending |
| `experimental.text.complete`           | Modify assistant text after generation completes    |

### Tool Hooks

| Hook                  | Description                                         |
| --------------------- | --------------------------------------------------- |
| `tool.execute.before` | Called before a tool executes; can modify args      |
| `tool.execute.after`  | Called after a tool executes; can modify the result |
| `tool.definition`     | Modify tool descriptions and parameter schemas      |

### Session Hooks

| Hook                              | Description                                                 |
| --------------------------------- | ----------------------------------------------------------- |
| `command.execute.before`          | Called before a slash command executes                      |
| `experimental.session.compacting` | Customize compaction behavior (add context, replace prompt) |

### System Hooks

| Hook        | Description                                           |
| ----------- | ----------------------------------------------------- |
| `shell.env` | Inject environment variables into shell execution     |
| `config`    | Called once after plugin init with the current config |
| `event`     | Receives all bus events                               |
| `tool`      | Register custom tool definitions                      |
| `auth`      | Provide authentication methods for a provider         |

## Hook Pattern

All trigger hooks follow the signature:

```typescript
(input: { ...context }, output: { ...mutable }) => Promise<void>
```

Plugins mutate the `output` object in-place. Multiple plugins run sequentially in load order.

## Example: System Prompt Injection

```typescript
export default (input) => ({
  "experimental.chat.system.transform": async (input, output) => {
    output.system.push("Always respond in French.")
  }
})
```

## Example: Custom Environment Variables

```typescript
export default (input) => ({
  "shell.env": async (input, output) => {
    output.env["MY_VAR"] = "value"
  }
})
```

## Plugin Loading

Plugins are loaded from:

| Location                                | Description      |
| --------------------------------------- | ---------------- |
| `opencode.json` `plugin` field          | NPM/git packages |
| `<project>/.opencode/plugins/*.{ts,js}` | Project-level    |
| `~/.config/opencode/plugins/*.{ts,js}`  | User-level       |

A plugin is a function `(input: PluginInput) => Promise<Hooks>` that returns the hooks it wants to handle.

## See Also

- [superpowers.md](superpowers.md) -- Uses `experimental.chat.system.transform` and `config` hooks
- [configuration.md](configuration.md) -- `plugin` configuration key
