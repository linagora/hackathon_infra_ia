# Context Window

The context window is the maximum number of tokens a model can process in a single request (input + output combined).

## How It Affects OpenCode

- **Input tokens**: system prompt, AGENTS.md, conversation history, tool results
- **Output tokens**: the model's response (capped by `limit.output` in provider config)
- When input tokens approach the context limit, [compaction](storage.md) is triggered automatically

## Typical Context Sizes

| Size  | Example              |
| ----- | -------------------- |
| 8K    | Older/smaller models |
| 128K  | Most current models  |
| 200K+ | Large context models |

## Configuration

Context and output limits are defined per model in `opencode.json`:

```json
{
  "provider": {
    "my-provider": {
      "models": {
        "model-id": {
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

## Monitoring

Use `<leader>s` or `/status` to view current token usage for the active session.

## See Also

- [storage.md](storage.md) -- Compaction (automatic context management)
- [providers.md](providers.md) -- Model limit configuration
