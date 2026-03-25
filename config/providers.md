# Providers

OpenCode supports multiple LLM providers. Configure them in `config.json` under the `provider` key.

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
