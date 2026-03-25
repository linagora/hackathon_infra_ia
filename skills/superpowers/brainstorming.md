# brainstorming

> Use before any creative work -- creating features, building components, adding functionality, or modifying behavior.

Explores user intent, requirements and design before implementation.

## Process

1. **Explore** project context and existing codebase
2. **Ask** clarifying questions one at a time
3. **Propose** 2-3 approaches with trade-offs
4. **Present** design sections for incremental approval
5. **Write** design doc to `docs/superpowers/specs/YYYY-MM-DD-<topic>-design.md`
6. **Review** spec (max 3 iterations with spec reviewer)
7. **Get** user approval
8. **Transition** to `writing-plans`

## Hard Gate

No implementation is allowed until the design is approved. This is non-negotiable.

## Scope Decomposition

For large projects, break down into isolated subsystems. Design for isolation -- each component should be independently testable and deployable.

## Visual Companion

Optionally offers browser-based mockups and diagrams for visual components.

## Bundled Files

- `spec-document-reviewer-prompt.md` -- Prompt for the spec review subagent
- `visual-companion.md` -- Guide for visual mockup tooling
- `scripts/` -- Helper scripts for the visual companion server
