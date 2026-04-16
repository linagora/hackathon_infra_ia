# Skills

Skills are `SKILL.md` files the agent can load on demand to follow a specific workflow or technique. They are injected into the agent's context when invoked via the `skill` tool or the `/skills` command palette entry.

## SKILL.md Format

A skill is a Markdown file named `SKILL.md` with YAML frontmatter:

```markdown
---
name: my-skill
description: Use this skill when you need to do X.
---

## Instructions

Step-by-step instructions the agent will follow...
```

| Frontmatter key  | Required | Description                                                             |
| ---------------- | -------- | ----------------------------------------------------------------------- |
| `name`           | yes      | Unique skill identifier (used to reference it)                          |
| `description`    | yes      | When to use this skill — shown in `/skills` picker                      |
| `user-invocable` | no       | Set to `false` to prevent plugins from auto-registering a slash command for this skill (default: `true`) |

The file body is the skill content injected into the agent context when the skill is loaded.

## Locations

Skills are scanned at startup from the following directories (in load order):

### External directories (compatible with Claude Code / other tools)

| Location                             | Scope   |
| ------------------------------------ | ------- |
| `~/.claude/skills/**/SKILL.md`       | Global  |
| `~/.agents/skills/**/SKILL.md`       | Global  |
| `<project>/.claude/skills/**/SKILL.md`  | Project (walks up to worktree root) |
| `<project>/.agents/skills/**/SKILL.md` | Project (walks up to worktree root) |

> Set `OPENCODE_DISABLE_EXTERNAL_SKILLS=1` to skip all external directory scanning.
> Set `OPENCODE_DISABLE_CLAUDE_CODE_SKILLS=1` to skip only the `.claude/` directories (keeps `.agents/`).

### OpenCode directories

| Location                                          | Scope   |
| ------------------------------------------------- | ------- |
| `~/.config/opencode/skill/**/SKILL.md`            | Global  |
| `~/.config/opencode/skills/**/SKILL.md`           | Global  |
| `<project>/.opencode/skill/**/SKILL.md`           | Project |
| `<project>/.opencode/skills/**/SKILL.md`          | Project |

### Custom paths and remote URLs

Additional skill sources can be configured in `opencode.json`:

```json
{
  "skills": {
    "paths": [
      "~/my-skills",
      "./.team/skills"
    ],
    "urls": [
      "https://example.com/opencode-skills"
    ]
  }
}
```

| Key             | Type     | Description                                                    |
| --------------- | -------- | -------------------------------------------------------------- |
| `skills.paths`  | string[] | Additional local directories to scan for `SKILL.md` files     |
| `skills.urls`   | string[] | Remote skill registries — fetched and cached locally           |

Remote URLs must expose an `index.json` listing skills with their files.

## How Skills Are Used

The agent automatically has access to all available skills via the `skill` tool. It can load a skill by name when the task matches the skill's description. Users can also browse and invoke skills manually via the command palette:

```
/skills        # open the skill picker in the TUI
```

If a skill is denied via `permission.skill.<name>: deny` in the agent config, the agent cannot use it.

## Example

```markdown
---
name: conventional-commits
description: Use this skill when writing git commit messages to follow the Conventional Commits specification.
---

## Conventional Commits

Format: `<type>(<scope>): <description>`

Types: feat, fix, docs, style, refactor, perf, test, chore

Rules:
- Use imperative mood in description
- Keep description under 72 characters
- Add body for breaking changes with `BREAKING CHANGE:` footer
```

## See Also

| Directory                                           | Description                                                |
| --------------------------------------------------- | ---------------------------------------------------------- |
| [superpowers/](superpowers/)                        | 14 agentic workflow skills (brainstorm, plan, TDD, debug)  |

- [../config/superpowers.md](../config/superpowers.md) -- Superpowers plugin installation
- [../config/configuration.md](../config/configuration.md) -- `skills` config key
