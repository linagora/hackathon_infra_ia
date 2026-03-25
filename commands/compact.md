# compact

Manually triggers summarization of the current session. Creates a condensed context to free up the token window.

## Access

- `<leader>c` (default: `Ctrl+X` then `c`)
- Or via command palette: `/compact` or `/summarize`

## Behavior

1. Summarizes the current conversation into a condensed context
2. Creates a new session with the summary as starting context
3. Frees up space in the token window

## Auto-Compact

When `compaction.auto` is enabled (default: `true`), OpenCode automatically triggers compaction when the context window is full. This can be disabled in `opencode.json`:

```json
{ "compaction": { "auto": false } }
```

Or via environment variable: `OPENCODE_DISABLE_AUTOCOMPACT=1`

## When to Use Manually

- When the conversation becomes long and the model starts losing context
- Before addressing a new topic in the same session
- When you receive token limit errors

## See Also

- [configuration.md](../config/configuration.md) -- `compaction` settings
