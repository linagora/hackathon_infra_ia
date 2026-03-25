# writing-skills

> Use when creating new skills, editing existing skills, or verifying skills work before deployment.

Writing skills IS TDD applied to process documentation.

## TDD Mapping

| TDD Concept     | Skills Equivalent                     |
| --------------- | ------------------------------------- |
| Test case       | Pressure scenario with subagent       |
| Production code | SKILL.md                              |
| RED             | Agent violates rule without the skill |
| GREEN           | Agent complies with the skill loaded  |

## When to Create a Skill

- A pattern is repeated across conversations
- A discipline needs enforcement
- A technique needs standardization

## Skill Structure

### Directory

```text
skills/<skill-name>/
├── SKILL.md          # Main skill file (required)
├── helper.md         # Optional bundled reference
└── scripts/          # Optional helper scripts
```

### SKILL.md Frontmatter

```yaml
---
name: my-skill-name       # Hyphens only, no spaces
description: "Use when..." # Trigger conditions only, never summarize workflow
---
```

## Token Efficiency Targets

| Skill Type        | Max Words |
| ----------------- | --------- |
| Getting-started   | 150       |
| Frequently-loaded | 200       |
| Others            | 500       |

## The Iron Law

No skill without a failing test first. Write a pressure scenario that demonstrates the agent fails without the skill, then write the skill to make it pass.

## Bundled Files

- `anthropic-best-practices.md` -- Best practices for skill writing
- `persuasion-principles.md` -- How to write convincing skill instructions
- `testing-skills-with-subagents.md` -- How to test skills with subagents
