---
description: Execute a structured implementation plan systematically using task-executor sub-agent
---

# Executing Implementation Plan: $ARGUMENTS

I'll coordinate execution of this plan using specialized sub-agents for isolated context per task.

## Initialization

Reading plan document: $ARGUMENTS

[Parse structure and show overview]

## Execution Strategy

I will use **plan-coordinator** sub-agent to:
- Execute ONE task at a time via task-executor
- Verify each task completes successfully
- Update checkboxes `[ ]` → `[x]`
- Track time and progress
- Report status regularly
- Handle blockers gracefully

Each task will:
- ✅ Run with isolated context
- ✅ Verify all success criteria
- ✅ Execute all tests
- ✅ Update "Last Verified" timestamp
- ✅ Confirm ready for next task

## Starting Execution

Delegating to plan-coordinator...

@plan-coordinator Execute the plan in $ARGUMENTS