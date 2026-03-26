# Models

A model is the LLM used for generation. In OpenCode, models are specified as `provider/model-name`.

## Selecting a Model

### In the TUI

- `<leader>m` or `/models` to open the model selector
- `F2` / `Shift+F2` to cycle through recently used models
- `Ctrl+T` to cycle model variants

### From the CLI

```bash
opencode run -m "provider/model-name" "your prompt"
opencode run --variant high "your prompt"
```

### In Configuration

Set a default model in `opencode.json`:

```json
{
  "model": "provider/model-name",
  "small_model": "provider/small-model-name"
}
```

Or per agent:

```json
{
  "agent": {
    "build": {
      "model": "provider/model-name",
      "variant": "high"
    }
  }
}
```

## Model Properties

| Property     | Description                                   |
| ------------ | --------------------------------------------- |
| `name`       | Display name                                  |
| `modalities` | Input/output types (`text`, `image`, `audio`) |
| `limit`      | Context window and output token limits        |
| `reasoning`  | Whether the model supports reasoning          |
| `tool_call`  | Whether the model supports tool calls         |
| `cost`       | Pricing (input/output/cache per token)        |

## Variants

Variants are provider-specific configurations (e.g., reasoning effort). Set via `--variant` flag or `agent.<name>.variant` config.

| Variant   | Typical meaning          |
| --------- | ------------------------ |
| `minimal` | Fastest, least reasoning |
| `low`     | Light reasoning          |
| `medium`  | Balanced (default)       |
| `high`    | More reasoning           |
| `max`     | Maximum reasoning effort |

## See Also

- [providers.md](providers.md) -- Custom provider and model configuration
- [agents.md](agents.md) -- Per-agent model assignment
- [context-window.md](context-window.md) -- Context window and token limits
