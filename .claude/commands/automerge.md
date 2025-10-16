---
description: Process one PR at a time, react to CI/bot comments, get green, and squash merge
allowed-tools: Bash(git:*), Bash(gh:*)
---

# Auto-Merge Single PR

Process ONE pull request from start to finish: respond to failures, fix CI issues, handle conflicts, and merge when green.

## Workflow:

### 1. List & Select PR

```bash
gh pr list --limit 20 --json number,title,state,statusCheckRollup,reviewDecision,mergeable
```

Show user a formatted table:
```
Available Pull Requests:
#127 - "feat: Add line tool" - ✅ CI passing, mergeable
#128 - "fix: Collision detection" - ❌ CI failing (ESLint)
#129 - "refactor: Extract hooks" - ⚠️ Has conflicts
#130 - "docs: Update README" - ⏳ CI pending

Which PR should I process? (enter number)
```

Wait for user to provide PR number.

### 2. Analyze PR Status

```bash
gh pr view <number> --json number,title,state,statusCheckRollup,reviewDecision,mergeable,mergeable,commits

gh pr checks <number>
```

Determine current state:
- ✅ **Ready to merge**: All checks pass, no conflicts
- ❌ **CI failing**: One or more checks failed
- ⚠️ **Has conflicts**: Needs rebase
- ⏳ **Pending**: Checks still running
- 📝 **Needs review**: Bot comments or review requests

### 3. Fix Issues Iteratively

**If CI failing:**
```bash
# Get detailed check output
gh pr checks <number>

# Common fixes:
# - ESLint errors: npm run lint:fix
# - Prettier: npm run format
# - Tests failing: npm test (read output, fix code)
# - Build failing: npm run build (read errors, fix)

# Checkout the PR branch
gh pr checkout <number>

# Make fixes
[fix the specific issues]

# Commit and push
git add <fixed-files>
git commit -m "fix: Address CI failures - [specific issue]"
git push
```

**If has conflicts:**
```bash
gh pr checkout <number>
git fetch origin main
git rebase origin/main

# If conflicts occur:
# - Show user conflicted files
# - Ask if they want auto-resolution or manual
# - For auto: accept incoming for simple conflicts
# - For manual: ask user to resolve, then continue

git rebase --continue
git push --force-with-lease
```

**If bot commented:**
```bash
gh pr view <number> --comments

# Read bot comments and respond:
# - Dependabot: Review and approve if safe
# - Lint bots: Fix issues automatically
# - Security bots: Report to user, ask for guidance
```

### 4. Wait for CI (Polling)

After pushing fixes, wait for CI to complete:

```bash
echo "⏳ Waiting for CI checks to complete..."

for i in {1..20}; do
  STATUS=$(gh pr checks <number> --json state,conclusion -q '.[] | select(.state != "success") | .state')

  if [ -z "$STATUS" ]; then
    echo "✅ All checks passed!"
    break
  fi

  if [ $i -eq 20 ]; then
    echo "⏰ Timeout: CI took longer than 10 minutes"
    exit 1
  fi

  echo "⏳ Checks still running... ($i/20) - waiting 30s"
  sleep 30
done
```

### 5. Merge When Green

Once all checks pass:

```bash
# Final verification
gh pr view <number> --json mergeable,statusCheckRollup

# Squash merge with auto-generated message
gh pr merge <number> \
  --squash \
  --auto \
  --delete-branch

echo "✅ PR #<number> successfully merged and branch deleted"
```

### 6. Update Local Repository

```bash
git checkout main
git pull origin main

echo "✅ Local main branch updated"
```

### 7. Report Summary

```
✅ Auto-merge completed for PR #<number>

Summary:
- Title: [PR title]
- Fixes applied: [list of fixes made]
- Final status: Merged and deleted
- Commits: [number] squashed into main
- Time taken: [duration]

Next steps:
- Run /automerge again for next PR
- Or return to development
```

## Iteration Loop

If CI fails after fixes:
1. Analyze new failure
2. Apply fix
3. Commit and push
4. Wait for CI again
5. Repeat until green OR max 5 iterations

After 5 iterations, report:
```
⚠️ Unable to auto-fix after 5 attempts.

Remaining issues:
- [list issues]

Recommendation: Manual intervention required.
Would you like me to:
1. Continue trying (5 more iterations)
2. Create a comment on PR describing issues
3. Stop and let you handle manually
```

## Critical Rules:

- Process ONLY ONE PR per invocation
- ALWAYS wait for CI after pushing changes
- NEVER force-merge with failing checks
- NEVER merge without user confirmation if complex issues found
- ASK user for guidance on breaking changes or architecture decisions
- Auto-fix ONLY safe issues: linting, formatting, simple test fixes
- Report progress every 30 seconds during CI wait

## Auto-fixable Issues:

✅ Safe to auto-fix:
- ESLint errors (run `npm run lint:fix`)
- Prettier formatting (run `npm run format`)
- Missing imports (add them)
- Simple merge conflicts in non-critical files
- Outdated branch (rebase on main)

❌ Require user input:
- Test failures requiring logic changes
- Breaking changes in dependencies
- Security vulnerabilities
- Large merge conflicts
- Changes to critical files (auth, payments, etc.)

## Error Handling:

- If PR doesn't exist: "❌ PR #<number> not found"
- If already merged: "✅ PR #<number> already merged"
- If draft PR: "⚠️ PR #<number> is a draft. Convert to ready first?"
- If required review missing: "⏳ PR #<number> needs approval. Request review?"

Proceed with single PR auto-merge workflow.
