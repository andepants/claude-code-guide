---
description: Create PR from current branch with auto-generated title and description
allowed-tools: Bash(git:*), Bash(gh:*)
---

# Create Pull Request

Create a well-formatted pull request from your current branch with smart auto-generated content.

## Workflow:

1. **Verify current state**:
   - Check current branch: `git rev-parse --abbrev-ref HEAD`
   - If on `main`, ERROR and stop: "Cannot create PR from main branch"
   - Check if branch exists on remote: `git ls-remote --heads origin $(git rev-parse --abbrev-ref HEAD)`

2. **Analyze commit history**:
   - Get all commits since branching from main: `git log main..HEAD --oneline`
   - Get detailed changes: `git diff main...HEAD --stat`
   - Read commit messages to understand the scope

3. **Generate PR title**:
   - If single commit: Use that commit message as title (without emoji/trailer)
   - If multiple commits with same prefix (e.g., all `feat:`): Summarize as one
     - Example: `feat: Add line tool with resize and collision detection`
   - If mixed types: Use most significant type or summarize comprehensively
     - Example: `Layers panel improvements and bug fixes`
   - Keep title under 72 characters

4. **Generate PR body**:
   ```markdown
   ## Summary
   [1-2 sentence overview of what this PR accomplishes]

   ## Changes
   - [Bullet point for each logical change from commits]
   - [Group related commits together]
   - [Focus on WHAT changed, not implementation details]

   ## Test Plan
   - [ ] Manual testing completed
   - [ ] Unit tests pass (`npm test`)
   - [ ] No console errors
   - [ ] Checked on [specific scenarios relevant to changes]

   🤖 Generated with [Claude Code](https://claude.com/claude-code)
   ```

5. **Push branch if needed**:
   - If branch doesn't exist on remote: `git push -u origin HEAD`
   - If branch exists but behind: `git push`

6. **Create the PR**:
   ```bash
   gh pr create \
     --title "Your Generated Title" \
     --body "$(cat <<'EOF'
   [Your generated body]
   EOF
   )"
   ```

7. **Auto-add labels** (if possible):
   - Detect from commit prefixes:
     - `feat:` → add `enhancement` label
     - `fix:` → add `bug` label
     - `docs:` → add `documentation` label
     - `refactor:` → add `refactoring` label
   - Run: `gh pr edit --add-label "enhancement"` (if labels exist)

8. **Report success**:
   ```
   ✅ Pull request created!

   Title: [title]
   URL: [PR URL from gh output]
   Commits: [number] commits
   Files: [number] files changed
   ```

## Smart Grouping Examples:

**Example 1: Multiple related commits**
```
Commits:
- feat: Add Line shape component
- feat: Add line resize handles
- fix: Line collision detection
- test: Add line tool tests

Generated Title:
feat: Add line tool with resize and collision detection

Generated Body:
## Summary
Implements a new line drawing tool with full resize functionality and proper collision detection.

## Changes
- Add Line shape component with SVG rendering
- Implement resize handles for line endpoints
- Fix collision detection to work with line bounding boxes
- Add comprehensive test coverage for line tool

## Test Plan
- [ ] Manual testing completed
- [ ] Unit tests pass
- [ ] Draw lines at various angles
- [ ] Resize lines from both endpoints
- [ ] Verify collision detection with other shapes
```

**Example 2: Bug fix with refactoring**
```
Commits:
- fix: Resolve selection box positioning bug
- refactor: Extract selection logic to hook

Generated Title:
fix: Resolve selection box positioning with refactored logic

Generated Body:
## Summary
Fixes selection box positioning issues by refactoring selection logic into a dedicated hook.

## Changes
- Fix selection box positioning when zoomed
- Extract selection logic to useSelection hook
- Improve performance with memoization

## Test Plan
- [ ] Selection box appears correctly at all zoom levels
- [ ] Multi-select works with nested objects
- [ ] No performance degradation
```

## Critical Rules:

- NEVER create PR from `main` branch
- ALWAYS push branch before creating PR
- Generate meaningful titles that summarize the changes
- Group related commits into logical bullet points
- Include test plan relevant to the changes
- If unsure about title/description, show user and ask for approval

## Error Handling:

- If on main: "❌ Cannot create PR from main. Create a feature branch first."
- If no commits ahead of main: "❌ No commits to create PR. Make commits first."
- If `gh` not authenticated: "❌ GitHub CLI not authenticated. Run 'gh auth login'"
- If remote branch conflicts: "❌ Branch exists with conflicts. Pull or use different branch name."

Proceed with PR creation workflow.
