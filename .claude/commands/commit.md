---
description: Commit only changes related to current task, ignoring other work-in-progress
allowed-tools: Bash(git:*)
---

# Smart Scoped Commit

This command commits only the files relevant to YOUR current work, leaving other changes untouched.

## Instructions:

1. **Analyze your current task** - What feature/fix were you just working on? Review recent conversation context.

2. **Check git status** - Run `git status` to see all changed files

3. **Identify your scope** - Based on the task you just completed, determine which files are relevant:
   - If you worked on collision detection → only collision.ts, related tests, and store changes
   - If you worked on keyboard shortcuts → only keyboardShortcuts.ts, Toolbar.tsx, etc.
   - If you worked on layers panel → only layers-panel/** files

4. **Show user the proposed scope** - Present a clear message:
   ```
   I completed work on: [describe task]

   Files I changed for this task:
   - file1.ts
   - file2.tsx

   Other unstaged files (from other work):
   - file3.ts
   - file4.tsx

   Should I commit only the files related to this task?
   ```

5. **Wait for confirmation** - User must approve the scope

6. **Stage selectively** - Use `git add <specific-file>` for each file (NEVER `git add .`)

7. **Generate focused commit message**:
   - Use conventional commit format: `feat:`, `fix:`, `refactor:`, `docs:`, `style:`, `test:`, `chore:`
   - Be concise and specific to the staged changes only
   - Example: "fix: Resolve collision detection for nested objects"
   - Example: "feat: Add Cmd+D keyboard shortcut for duplicate"

8. **Create commit** with Co-Authored-By trailer:
   ```bash
   git commit -m "$(cat <<'EOF'
   [your commit message]

   🤖 Generated with [Claude Code](https://claude.com/claude-code)

   Co-Authored-By: Claude <noreply@anthropic.com>
   EOF
   )"
   ```

9. **Report results**:
   ```
   ✅ Committed: [commit message]

   Staged files:
   - file1.ts
   - file2.tsx

   Left unstaged (for other agents/work):
   - file3.ts
   - file4.tsx
   ```

## Critical Rules:

- NEVER use `git add .` or `git add -A`
- ALWAYS confirm file scope with user first
- Leave unrelated files unstaged for other agents/work
- Focus commit message on ONLY the staged changes
- If user worked on multiple unrelated things, suggest separate commits
- If unsure about a file's scope, ask the user

## Example Session:

```
User: /commit
Agent: I completed work on: Adding line resize handles

Files I changed for this task:
- src/features/canvas-core/shapes/Line.tsx
- src/features/canvas-core/components/LineResizeHandles.tsx
- src/features/canvas-core/utils/lineHelpers.ts

Other unstaged files (from other work):
- src/features/layers-panel/components/LayerItem.tsx
- src/stores/canvasStore.ts

Should I commit only the line-related files?

User: Yes
Agent: [stages only line files, commits "feat: Add resize handles for line tool"]
```

Proceed with scoped commit workflow.
