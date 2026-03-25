# Superpowers

[Superpowers](https://github.com/obra/superpowers) is a third-party plugin that injects a structured agentic workflow into OpenCode. Instead of immediately writing code, the agent follows a disciplined pipeline: brainstorm, design, plan, then implement with TDD.

## Installation

Add the plugin to your `config.json` (located at `~/.config/opencode/config.json`):

```json
{
  "plugin": ["superpowers@git+https://github.com/obra/superpowers.git"]
}
```

Then restart OpenCode. The plugin auto-installs via Bun.

### Pin a Specific Version

```json
{
  "plugin": ["superpowers@git+https://github.com/obra/superpowers.git#v5.0.3"]
}
```

### Verify Installation

Ask OpenCode: **"Tell me about your superpowers"**

## Skills Included

| Skill                            | Description                                                 | Details                                                                                      |
| -------------------------------- | ----------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| `brainstorming`                  | Refines ideas through questions, explores alternatives      | [brainstorming.md](../skills/superpowers/brainstorming.md)                                   |
| `writing-plans`                  | Breaks designs into small 2-5 minute tasks with exact specs | [writing-plans.md](../skills/superpowers/writing-plans.md)                                   |
| `executing-plans`                | Systematically executes plan items                          | [executing-plans.md](../skills/superpowers/executing-plans.md)                               |
| `test-driven-development`        | Enforces RED-GREEN-REFACTOR cycle                           | [test-driven-development.md](../skills/superpowers/test-driven-development.md)               |
| `systematic-debugging`           | 4-phase root cause analysis process                         | [systematic-debugging.md](../skills/superpowers/systematic-debugging.md)                     |
| `subagent-driven-development`    | Delegates tasks to fresh sub-agents with two-stage review   | [subagent-driven-development.md](../skills/superpowers/subagent-driven-development.md)       |
| `dispatching-parallel-agents`    | Runs concurrent agent tasks                                 | [dispatching-parallel-agents.md](../skills/superpowers/dispatching-parallel-agents.md)       |
| `using-git-worktrees`            | Creates isolated workspace on new branches                  | [using-git-worktrees.md](../skills/superpowers/using-git-worktrees.md)                       |
| `finishing-a-development-branch` | Workflow for completing branches                            | [finishing-a-development-branch.md](../skills/superpowers/finishing-a-development-branch.md) |
| `requesting-code-review`         | Structured code review requests                             | [requesting-code-review.md](../skills/superpowers/requesting-code-review.md)                 |
| `receiving-code-review`          | Handling incoming code review feedback                      | [receiving-code-review.md](../skills/superpowers/receiving-code-review.md)                   |
| `verification-before-completion` | Evidence-based verification before marking work done        | [verification-before-completion.md](../skills/superpowers/verification-before-completion.md) |
| `using-superpowers`              | Meta-skill for skill discovery and invocation               | [using-superpowers.md](../skills/superpowers/using-superpowers.md)                           |
| `writing-skills`                 | How to write new custom skills                              | [writing-skills.md](../skills/superpowers/writing-skills.md)                                 |

## Usage

- **List skills**: ask "use skill tool to list skills"
- **Load a specific skill**: ask "use skill tool to load superpowers/brainstorming"

## Skill Priority Order

1. Project skills (`.opencode/skills/` in your repo)
2. Personal skills (`~/.config/opencode/skills/`)
3. Superpowers skills (from the plugin)

## Creating Custom Skills

Create a directory under `~/.config/opencode/skills/my-skill/` with a `SKILL.md` file containing YAML frontmatter with `name` and `description` fields.

## See Also

- [configuration.md](configuration.md) -- Full configuration reference
