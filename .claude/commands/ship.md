---
description: Automatically stage changes, commit, create PR, and merge to main if no conflicts
allowed-tools: Bash(git:*), Bash(gh:*)
---

# Automated Git Workflow: Stage, Commit, PR, and Merge

This command will automate your entire git workflow from staging changes to merging into main.

## Workflow Steps:

1. First, check the current git status to see what files have changed
2. Analyze the changes using git diff to understand what was modified
3. Create a new feature branch if currently on main, otherwise use the current branch
4. Stage all changes with `git add .`
5. Generate a descriptive commit message based on the actual changes
6. Create a commit with the generated message
7. Push the branch to remote
8. Create a pull request with a detailed summary
9. Check if the PR has any merge conflicts
10. If no conflicts exist, automatically merge the PR into main
11. Switch back to main and pull the latest changes

Please proceed with this automated workflow. If any step fails, stop and report the error.
