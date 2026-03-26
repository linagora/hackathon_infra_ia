# grep.app

[grep.app](https://grep.app) searches code across millions of public GitHub repositories. Free, no authentication required.

## Installation

```json
{
  "mcp": {
    "grep": {
      "type": "remote",
      "url": "https://mcp.grep.app"
    }
  }
}
```

## Tools

### grep

Search code across public GitHub repositories.

| Parameter    | Type   | Required | Description                                   |
| ------------ | ------ | -------- | --------------------------------------------- |
| `query`      | string | yes      | Search query or regex pattern                 |
| `language`   | string | no       | Filter by language (e.g., "python", "go")     |
| `repository` | string | no       | Filter by repository (e.g., "vercel/next.js") |
| `path`       | string | no       | Filter by file path pattern                   |

Returns file paths, line numbers, code snippets, and repository information.

## Usage Examples

**Find usage patterns:**
> "Search grep.app for how projects implement rate limiting in Go"

**Find examples in a specific repo:**
> "Search grep.app for middleware usage in vercel/next.js"

**Regex search:**
> "Search grep.app for the regex pattern `func.*Handler.*http\.`"
