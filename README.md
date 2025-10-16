# claude-code-guide

Current Claude Code flow is:

1. Create feature 1. Here is the context. Use `@creating-implementation-plans.md` to create md file at `_docs/plans/feature1.md`
2. Run command `@execute-plan @_docs/plans/feature1.md`

## Commands

- `@execute-plan` - Execute a structured implementation plan file

## Agents

- plan-coordinator.md
- task-executor.md
