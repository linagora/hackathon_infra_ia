# Glossary

| Term                                             | Definition                                                                                                                                      |
| ------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| **[Agent](config/agents.md)**                    | An AI role with specific tools and permissions. OpenCode has 7 agents: `build`, `plan`, `general`, `explore`, `compaction`, `title`, `summary`. |
| **[AGENTS.md](config/agents-md.md)**             | Markdown file containing project instructions loaded as context in every session. Supports hierarchical placement in subdirectories.            |
| **[Compaction](config/storage.md)**              | Automatic summarization of a long conversation to free up the context window. Triggered when token usage approaches the model limit.            |
| **Context window**                               | The maximum number of tokens a model can process in a single request (input + output).                                                          |
| **[Hook](config/hooks.md)**                      | A function registered by a plugin to intercept and modify OpenCode behavior (e.g., system prompt, tool execution).                              |
| **[Instructions](config/agents-md.md)**          | Files (AGENTS.md, or custom paths/URLs) injected into the system prompt to guide the agent.                                                     |
| **[Leader key](shortcuts/global-shortcuts.md)**  | A prefix key (default `Ctrl+X`) used for chord shortcuts. Press leader, release, then press the second key.                                     |
| **[LSP](lsp/)**                                  | Language Server Protocol. Provides code intelligence (diagnostics, completions) for programming languages.                                      |
| **[MCP](mcp/)**                                  | Model Context Protocol. A standard for connecting AI agents to external tool servers (local or remote).                                         |
| **Model**                                        | The LLM used for generation, specified as `provider/model-name`.                                                                                |
| **[Plugin](config/superpowers.md)**              | A package that extends OpenCode via hooks, tools, or skills. Installed via `opencode.json` `plugin` field.                                      |
| **[Provider](config/providers.md)**              | A service hosting LLM models (e.g., Anthropic, OpenAI, or a custom OpenAI-compatible endpoint).                                                 |
| **[Session](config/storage.md)**                 | A conversation with the agent. Persisted in SQLite, can be resumed, forked, or compacted.                                                       |
| **[Skill](config/superpowers.md)**               | A `SKILL.md` file with YAML frontmatter that the agent can load on demand to follow a specific workflow or technique.                           |
| **[Slash command](commands/command-palette.md)** | A command typed as `/name` in the input or selected from the command palette (`Ctrl+P`).                                                        |
| **[Subagent](config/agents.md)**                 | A child agent spawned by the main agent for a specific task (`general` or `explore`). Runs in a child session.                                  |
| **[Superpowers](skills/superpowers/)**           | A third-party plugin providing 14 skills for structured agentic workflows (brainstorming, TDD, debugging, etc.).                                |
| **[TUI](shortcuts/)**                            | Terminal User Interface. The interactive terminal app launched by `opencode`.                                                                   |
| **[Variant](config/agents.md)**                  | A provider-specific model configuration (e.g., reasoning effort: `high`, `max`, `minimal`).                                                     |
| **[Worktree](config/storage.md)**                | A git worktree. OpenCode uses the worktree root as the project boundary for config and instruction file lookup.                                 |
