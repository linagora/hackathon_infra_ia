# AGENTS.md

`AGENTS.md` is the main instruction file loaded as context in every OpenCode session. It tells the agent how to work in your project.

## Quick Start

Run `/init` in OpenCode to generate one automatically. It analyzes your codebase and creates an `AGENTS.md` at the project root.

## File Priority

OpenCode looks for these filenames (first match wins per directory):

1. `AGENTS.md` (preferred)
2. `CLAUDE.md` (compatibility)
3. `CONTEXT.md` (deprecated)

## Loading Locations

### Project (loaded into system prompt)

OpenCode walks **up** from the working directory to the git worktree root. All `AGENTS.md` files found along the way are loaded.

```text
project/
├── AGENTS.md                    # Project-wide (always loaded)
├── packages/
│   ├── frontend/
│   │   └── AGENTS.md            # Frontend-specific
│   └── backend/
│       └── AGENTS.md            # Backend-specific
└── tests/
    └── AGENTS.md                # Test-specific
```

Subdirectory `AGENTS.md` files are also loaded dynamically when the agent reads a file in that directory.

### Global

| Location                       | Description                        |
| ------------------------------ | ---------------------------------- |
| `~/.config/opencode/AGENTS.md` | Global instructions (all projects) |

### Additional (via config)

The `instructions` field in `opencode.json` adds more instruction sources:

```json
{
  "instructions": [
    "docs/coding-guidelines.md",
    "https://example.com/team-rules.md"
  ]
}
```

## Recommended Sections

There is no enforced schema -- it's free-form Markdown. The `/init` command generates these sections:

### 1. Build / Lint / Test Commands

```markdown
## Build & Test

- Build: `npm run build`
- Lint: `npm run lint`
- Test all: `npm test`
- Test single file: `npm test -- path/to/file.test.ts`
- Test single case: `npm test -- -t "test name"`
```

### 2. Code Style Guidelines

```markdown
## Code Style

- Imports: use named imports, no default exports
- Formatting: 2-space indentation, no semicolons
- Types: prefer interfaces over type aliases
- Naming: camelCase for variables, PascalCase for types
- Error handling: use Result types, no try/catch for control flow
```

### 3. Project-Specific Rules

```markdown
## Rules

- Never modify generated files in `src/generated/`
- Always run migrations with `npm run db:migrate`
- Use the `Effect.gen` pattern for async operations
```

## Hierarchical Pattern

Place instructions as close to the relevant code as possible:

- **Root `AGENTS.md`** -- Project-wide conventions (style, commands, architecture)
- **Package `AGENTS.md`** -- Package-specific rules (framework conventions, test patterns)
- **Directory `AGENTS.md`** -- Narrow instructions (generated code warnings, special test setup)

## Tips

- Keep it under ~150 lines per file
- Focus on what an agent needs to know, not what a human developer already knows
- Include the exact command to run a single test (agents use this constantly)
- Use `/init` to bootstrap, then manually refine
- The `/learn` command can extract learnings from a session into the appropriate `AGENTS.md`

## See Also

- [init.md](../commands/init.md) -- Generate AGENTS.md with `/init`
- [configuration.md](configuration.md) -- `instructions` config key
