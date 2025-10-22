# claude-code-guide

Current Claude Code flow is:

1. Create feature 1. Here is the context. Use `@creating-implementation-plans.md` to create md file at `_docs/plans/feature1.md`
2. Run command `@execute-plan @_docs/plans/feature1.md`

## Commands

- `@execute-plan` - Execute a structured implementation plan file

## Agents

- plan-coordinator.md
- task-executor.md

## Terminal Shortcuts

| Shortcut | Action |
|----------|--------|
| `Ctrl + Shift + ~` | New terminal |
| `Cmd + \` | New terminal in group |
| `Shift + Cmd + [` | Terminal navigation |
| `Option + Cmd + ←/→` | Terminal group navigation |
| `Ctrl + C` | Interrupt terminal |
| `Ctrl + D` | Close terminal |

## Vibe Code Claude Primer

### ESSENTIALS
- Type "ultrathink" when using plane mode
- Type "/init to auto-generate initial CLAUDE.md (used in prompts to understand your repo better)
- Vibe Code by: Plan Code, Code, Review Code, Refactor
- YOLO MODE: Use "claude --dangerously-skip-permissions" to save time for your kids
- Run "/clear" when changing context
- Run multiple claude instances because why not?
- Be specific, pretend you are talking to your high functioning coding genius autistic little brother
- /context to check your usage
- Create commands in .claude/commands/example.md … run /examples to automate prompts
- Press Tab to turn on "thinking on"
- Ctr + r to search for past prompts
- Run "/help" to show all commands you should know
- /doctor to update cc
- "/config" to turn off auto compact - save context

### Advanced
- Run multiple worktrees … because why not? (advanced)
- [BMAD](https://github.com/bmad-code-org/BMAD-METHOD?tab=readme-ov-file)

### MCPS
- context7 - get up to date docs
- serena - give agent better tools to search repo = save tokens [Serena quick setup guide](https://github.com/anthropics/claude-code)

### Other Links
- https://www.anthropic.com/engineering/claude-code-best-practices
- https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents
