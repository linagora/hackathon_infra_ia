# Hooks

Plugins register hooks to intercept and modify OpenCode behavior. All hooks are optional — a plugin only needs to return the hooks it cares about.

## Plugin Structure

A plugin is a function that receives a `PluginInput` object and returns a `Hooks` object:

```typescript
import type { Plugin, PluginInput, Hooks } from "@opencode-ai/plugin"

const MyPlugin: Plugin = async (input: PluginInput): Promise<Hooks> => {
  return {
    // hooks here
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

## Hook Reference

### `chat.message`

Called when a new user message is received. Modify the message or its parts before processing.

```typescript
"chat.message": async (input, output) => {
  // input: { sessionID, agent?, model?, messageID?, variant? }
  // output.message: the UserMessage object (mutable)
  // output.parts: array of message parts (mutable)
  output.parts.push({ type: "text", text: "Additional context..." })
}
```

### `chat.params`

Modify LLM generation parameters before the request is sent.

```typescript
"chat.params": async (input, output) => {
  // input: { sessionID, agent, model, provider, message }
  // output: { temperature, topP, topK, maxOutputTokens, options }
  output.temperature = 0.2
  output.maxOutputTokens = 4096
}
```

### `chat.headers`

Inject custom HTTP headers into LLM API requests.

```typescript
"chat.headers": async (input, output) => {
  // input: { sessionID, agent, model, provider, message }
  // output: { headers: Record<string, string> }
  output.headers["X-Custom-Header"] = "value"
}
```

### `permission.ask`

Called when the agent requests permission for a tool call. Override the default permission decision.

```typescript
"permission.ask": async (input, output) => {
  // input: Permission object ({ tool, args, ... })
  // output.status: "ask" | "allow" | "deny"
  if (input.tool === "bash" && input.args?.command?.startsWith("rm")) {
    output.status = "deny"
  }
}
```

### `command.execute.before`

Called before a slash command executes. Can inject message parts.

```typescript
"command.execute.before": async (input, output) => {
  // input: { command: string, sessionID: string, arguments: string }
  // output.parts: Part[] — injected before command execution
  if (input.command === "review") {
    output.parts.push({ type: "text", text: "Focus on security issues." })
  }
}
```

### `tool.execute.before`

Called before a tool executes. Can modify the tool arguments.

```typescript
"tool.execute.before": async (input, output) => {
  // input: { tool: string, sessionID: string, callID: string }
  // output.args: the tool arguments (mutable)
  if (input.tool === "bash") {
    output.args.command = output.args.command.replace("rm -rf", "echo BLOCKED:")
  }
}
```

### `tool.execute.after`

Called after a tool executes. Can modify the result shown to the agent.

```typescript
"tool.execute.after": async (input, output) => {
  // input: { tool, sessionID, callID, args }
  // output: { title: string, output: string, metadata: any }
  if (input.tool === "bash") {
    output.output = output.output.slice(0, 10000) // truncate large outputs
  }
}
```

### `tool.definition`

Modify tool descriptions or parameter schemas sent to the LLM.

```typescript
"tool.definition": async (input, output) => {
  // input: { toolID: string }
  // output: { description: string, parameters: any }
  if (input.toolID === "bash") {
    output.description += " Never use destructive commands like rm -rf."
  }
}
```

### `shell.env`

Inject environment variables into every shell execution (`bash` tool, subprocesses).

```typescript
"shell.env": async (input, output) => {
  // input: { cwd: string, sessionID?: string, callID?: string }
  // output.env: Record<string, string>
  output.env["MY_API_KEY"] = process.env.MY_API_KEY ?? ""
  output.env["NODE_ENV"] = "development"
}
```

### `tool`

Register custom tool definitions. Tools are available to the agent like built-in tools.

```typescript
import { tool } from "@opencode-ai/plugin"

const MyPlugin: Plugin = async () => ({
  tool: {
    greet: tool({
      description: "Greet someone by name",
      args: {
        name: tool.schema.string().describe("Name to greet"),
      },
      async execute(args) {
        return `Hello, ${args.name}!`
      },
    }),
  },
})
```

### `config`

Called once after plugin initialization with the resolved config. Can be used for one-time setup — or to **mutate the config** to register additional skills paths, slash commands, and agents programmatically.

```typescript
"config": async (config) => {
  // Add a skills directory
  config.skills ??= {}
  config.skills.paths ??= []
  config.skills.paths.push("/path/to/my/skills")

  // Register a slash command
  config.command ??= {}
  config.command["my-command"] = {
    description: "Run my custom workflow",
    template: "Load and follow the my-command skill.",
  }

  // Register an agent
  config.agent ??= {}
  config.agent["my-reviewer"] = {
    description: "Reviews code for quality issues",
    mode: "subagent",
    prompt: "You are an expert code reviewer...",
  }
}
```

| Mutable field          | Type     | Description                                                                              |
| ---------------------- | -------- | ---------------------------------------------------------------------------------------- |
| `config.skills.paths`  | string[] | Additional local directories to scan for `SKILL.md`                                     |
| `config.skills.urls`   | string[] | Remote skill registry URLs                                                               |
| `config.command`       | object   | Slash commands: `{ [name]: { description, template, agent?, model?, subtask? } }`       |
| `config.agent`         | object   | Agent definitions: `{ [name]: { description, mode, prompt, ... } }`                     |
| `config.provider`      | object   | Custom provider configurations (same format as `opencode.json`)                          |
| `config.mcp`           | object   | MCP server configurations                                                                |
| `config.instructions`  | string[] | Additional instruction file patterns                                                     |
| `config.permission`    | object   | Global permission rules                                                                  |
| `config.model`         | string   | Default model (`provider/model`)                                                         |
| `config.default_agent` | string   | Default primary agent                                                                    |

### `event`

Receives all internal bus events. Use for observability or side effects.

```typescript
"event": async ({ event }) => {
  if (event.type === "session.updated") {
    console.log("Session updated:", event.properties.sessionID)
  }
}
```

### `auth`

Provide a custom authentication method for a provider (OAuth or API key flow).

```typescript
"auth": {
  provider: "my-provider",
  methods: [
    {
      type: "api",
      label: "API Key",
      prompts: [{ type: "text", key: "apiKey", message: "Enter your API key" }],
    }
  ]
}
```

### `provider`

Register custom model lists for a provider.

```typescript
"provider": {
  id: "my-provider",
  async models(provider, ctx) {
    return {
      "my-model": {
        id: "my-model",
        name: "My Model",
        // ...
      }
    }
  }
}
```

### `experimental.chat.system.transform`

Modify the system prompt array before it is sent to the LLM.

```typescript
"experimental.chat.system.transform": async (input, output) => {
  // input: { sessionID?, model }
  // output.system: string[] — the system prompt parts
  output.system.push("Always respond in bullet points.")
}
```

### `experimental.chat.messages.transform`

Transform the entire message history before it is sent to the LLM.

```typescript
"experimental.chat.messages.transform": async (_input, output) => {
  // output.messages: { info: Message, parts: Part[] }[]
  output.messages = output.messages.filter((m) => !m.info.synthetic)
}
```

### `experimental.session.compacting`

Customize the compaction prompt when the context window is nearing its limit.

```typescript
"experimental.session.compacting": async (input, output) => {
  // input: { sessionID: string }
  // output.context: string[] — appended to the default prompt
  // output.prompt: string | undefined — replaces the default prompt entirely
  output.context.push("Preserve all TODO items verbatim.")
}
```

### `experimental.compaction.autocontinue`

Called after compaction completes. Control whether the agent automatically continues.

```typescript
"experimental.compaction.autocontinue": async (input, output) => {
  // input: { sessionID, agent, model, provider, message, overflow }
  // output.enabled: boolean (default: true)
  output.enabled = false // pause after compaction
}
```

### `experimental.text.complete`

Modify the assistant's final text after generation completes.

```typescript
"experimental.text.complete": async (input, output) => {
  // input: { sessionID, messageID, partID }
  // output.text: string
  output.text = output.text.trimEnd()
}
```

## Plugin Loading

Plugins are loaded from:

| Location                                | Description      |
| --------------------------------------- | ---------------- |
| `opencode.json` `plugin` field          | NPM/git packages |
| `<project>/.opencode/plugins/*.{ts,js}` | Project-level    |
| `~/.config/opencode/plugins/*.{ts,js}`  | User-level       |

Multiple plugins run sequentially in load order. Each plugin mutates the `output` object in-place.

```json
{
  "plugin": [
    "my-opencode-plugin",
    ["my-plugin-with-options", { "key": "value" }],
    "git+https://github.com/user/my-plugin.git"
  ]
}
```

## See Also

- [superpowers.md](superpowers.md) -- Uses `experimental.chat.system.transform` and `config` hooks
- [configuration.md](configuration.md) -- `plugin` configuration key
