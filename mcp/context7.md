# Context7

[Context7](https://github.com/upstash/context7) delivers up-to-date, version-specific documentation and code examples for any library directly into the LLM prompt.

## Installation

### Remote (recommended, no auth required)

```json
{
  "mcp": {
    "context7": {
      "type": "remote",
      "url": "https://mcp.context7.com/mcp"
    }
  }
}
```

Works without authentication (rate-limited). For higher limits, get an API key at [context7.com/dashboard](https://context7.com/dashboard) and use local mode.

### Local (with API key)

```json
{
  "mcp": {
    "context7": {
      "type": "local",
      "command": ["npx", "-y", "@upstash/context7-mcp@latest"],
      "environment": { "CONTEXT7_API_KEY": "your-key" }
    }
  }
}
```

Both modes connect to the same backend. Local is useful behind corporate firewalls or for lower latency.

## Tools

### resolve-library-id

Resolves a library name to a Context7 library ID. Must be called before `query-docs`.

| Parameter     | Type   | Required | Description                                     |
| ------------- | ------ | -------- | ----------------------------------------------- |
| `libraryName` | string | yes      | Library name (e.g., "nextjs", "react", "flask") |
| `query`       | string | yes      | The question/task, used to rank results         |

### query-docs

Retrieves documentation and code examples for a specific library.

| Parameter   | Type   | Required | Description                                                    |
| ----------- | ------ | -------- | -------------------------------------------------------------- |
| `libraryId` | string | yes      | Context7 library ID (e.g., `/vercel/next.js`, `/mongodb/docs`) |
| `query`     | string | yes      | Specific question                                              |

> Do not call either tool more than 3 times per question.

## Usage Examples

**Resolve then query:**
1. `resolve-library-id` with `libraryName: "nextjs"`, `query: "middleware setup"`
2. Returns `/vercel/next.js`
3. `query-docs` with `libraryId: "/vercel/next.js"`, `query: "middleware setup"`

**Version-specific:**
- `query-docs` with `libraryId: "/vercel/next.js/v14.3.0-canary.87"`, `query: "app router"`
