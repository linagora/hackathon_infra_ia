# Providers

OpenCode supports multiple LLM providers. Configure them in `opencode.json` under the `provider` key, or authenticate interactively with `opencode providers login`.

## OpenRouter

[OpenRouter](https://openrouter.ai) gives access to 300+ models (Anthropic, OpenAI, Google, Meta, Mistral, …) through a single API key.

### Setup

```bash
opencode providers login
# Select "OpenRouter" → paste your API key from openrouter.ai
```

The key is stored in `~/.local/share/opencode/auth.json`. Alternatively, set the environment variable:

```bash
export OPENROUTER_API_KEY=sk-or-...
```

### Selecting a model

```bash
opencode models openrouter   # list available models
```

Or directly in the TUI with `/models`, then filter by `openrouter/`.

### Example `opencode.json`

```json
{
  "$schema": "https://opencode.ai/config.json",
  "model": "openrouter/anthropic/claude-sonnet-4-5"
}
```

Common models:

| Model ID                                        | Description                        |
| ----------------------------------------------- | ---------------------------------- |
| `openrouter/anthropic/claude-sonnet-4-5`        | Claude Sonnet 4.5 (Anthropic)      |
| `openrouter/openai/gpt-4o`                      | GPT-4o (OpenAI)                    |
| `openrouter/google/gemini-2.0-flash-001`        | Gemini 2.0 Flash (Google)          |
| `openrouter/meta-llama/llama-3.3-70b-instruct`  | Llama 3.3 70B (Meta, free tier)    |
| `openrouter/mistralai/mistral-small-3.1-24b`    | Mistral Small 3.1 (Mistral AI)     |

The full catalog is at [openrouter.ai/models](https://openrouter.ai/models).

---

## Custom Provider Configuration

You can configure custom OpenAI-compatible providers. Replace `<url>:<port>` with the actual URL and port of your endpoint.

```json
{
  "provider": {
    "my-provider": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "My Provider",
      "options": {
        "baseURL": "http://<url>:<port>/v1"
      },
      "models": {
        "model-id": {
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

> **Important**: Replace `<url>:<port>` with the actual address of your inference server (e.g., a RunPod endpoint, a local server, etc.).

## Configuration Keys

| Key                                         | Type    | Description                                             |
| ------------------------------------------- | ------- | ------------------------------------------------------- |
| `provider.<name>.npm`                       | string  | NPM package for the provider SDK                        |
| `provider.<name>.name`                      | string  | Display name                                            |
| `provider.<name>.options.baseURL`           | string  | Base URL of the API endpoint (`http://<url>:<port>/v1`) |
| `provider.<name>.models`                    | object  | Map of model IDs to model configurations                |
| `provider.<name>.models.<id>.name`          | string  | Model display name                                      |
| `provider.<name>.models.<id>.modalities`    | object  | Input/output modalities (`text`, `image`)               |
| `provider.<name>.models.<id>.limit.context` | integer | Maximum context window size (tokens)                    |
| `provider.<name>.models.<id>.limit.output`  | integer | Maximum output tokens                                   |

## Example: Multiple Providers

```json
{
  "provider": {
    "my-small-model": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "Small Model (GPU A)",
      "options": {
        "baseURL": "http://<url>:<port>/v1"
      },
      "models": {
        "org/model-small": {
          "name": "Small Model 7B",
          "modalities": { "input": ["text"], "output": ["text"] },
          "limit": { "context": 131072, "output": 8192 }
        }
      }
    },
    "my-large-model": {
      "npm": "@ai-sdk/openai-compatible",
      "name": "Large Model (GPU B)",
      "options": {
        "baseURL": "http://<url>:<port>/v1"
      },
      "models": {
        "org/model-large": {
          "name": "Large Model 122B",
          "modalities": { "input": ["text", "image"], "output": ["text"] },
          "limit": { "context": 262144, "output": 16384 }
        }
      }
    }
  }
}
```

## See Also

- [configuration.md](configuration.md) -- `.opencode.json` and `config.json` reference
- [environment-variables.md](environment-variables.md) -- Environment variables
