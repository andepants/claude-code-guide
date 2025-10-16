# Guide: Creating Implementation Plans

Use this guide when creating new implementation plans that work with the task-executor system.

## Quick Start

1. Copy template: `_docs/templates/task-plan-template.md`
2. Fill in header (name, time estimate, dependencies)
3. Complete Phase 0 (Research & Planning)
4. Break work into phases (3-5 hours each)
5. Break phases into task groups (1-2 hours each)
6. Break task groups into specific tasks (15-45 min each)

## Task Writing Best Practices

### ✅ Good Task Example
````markdown
### 1.1.1 Create Canvas Store
- [ ] **Action:** Create `src/stores/canvasStore.ts` with object management
  - **Why:** Centralized state management for canvas objects
  - **Files Modified:**
    - Create: `src/stores/canvasStore.ts`
    - Update: `src/stores/index.ts` (add export)
  - **Implementation Details:**
```typescript
    interface CanvasStore {
      objects: CanvasObject[];
      selectedIds: string[];
      addObject: (obj: CanvasObject) => void;
      removeObject: (id: string) => void;
      selectObject: (id: string) => void;
    }
```
  - **Success Criteria:**
    - [ ] Store created with JSDoc comments
    - [ ] All methods have type annotations
    - [ ] Exported from stores/index.ts
  - **Tests:**
    1. Import store: `import { useCanvasStore } from '@/stores'`
    2. Add object: `addObject({...})`, verify in `objects` array
    3. Select object: `selectObject(id)`, verify in `selectedIds`
  - **Edge Cases:**
    - ⚠️ Duplicate IDs: Use `nanoid()` for guaranteed unique IDs
    - ⚠️ Selecting non-existent object: Validate ID exists before selecting
````

### ❌ Bad Task Example
````markdown
### 1.1.1 Set up store
- [ ] Create the canvas store
  - Add methods
  - Make it work
````

**Problems:**
- Not specific enough
- No file paths
- No success criteria
- No test procedure
- No implementation guidance

## Task Size Guidelines

| Size | Time | Characteristics | Example |
|------|------|-----------------|---------|
| Too Small | < 5 min | Just one tiny change | "Add import statement" |
| Good | 15-45 min | One focused unit of work | "Create store with 3 methods" |
| Too Large | > 1 hour | Multiple concerns mixed | "Build entire feature" |

**Rule:** If a task takes > 45 min, break it into sub-tasks.

## Phase Organization

### Phase 0: Research & Planning
Always start here:
- Document existing patterns
- List technical constraints
- Make key decisions
- Create architecture overview

### Phase 1-N: Implementation
Each phase should:
- Achieve one complete milestone
- Build on previous phases
- Be independently testable
- Take 3-5 hours total

### Final Phase: Integration & Testing
Always end here:
- End-to-end testing
- Performance verification
- Cross-browser testing
- Documentation updates

## Success Criteria Rules

Make them:
- **Specific:** "Store exported from @/stores" not "Store works"
- **Testable:** Include exact test command/procedure
- **Binary:** Either done or not done, no ambiguity
- **Independent:** Each criterion tests one thing

## Test Writing

Every task needs:
1. **Manual test procedure** - Step-by-step human verification
2. **Expected result** - What success looks like
3. **Test data** - What inputs to use

Example:
````markdown
**Tests:**
1. Open browser console
2. Run: `useCanvasStore.getState().addObject({ type: 'rect', x: 0, y: 0 })`
3. Expected: No errors, object appears in canvas
4. Verify: `useCanvasStore.getState().objects.length === 1`
````

## Edge Cases

Always consider:
- **Null/undefined inputs**
- **Empty states** (no objects, no selection)
- **Maximum limits** (100+ objects, very long names)
- **Concurrent actions** (multiple users editing)
- **Network failures** (Firebase disconnects)
- **Browser differences**

## Rollback Strategy

Each task should document:
- How to undo the change
- What dependencies might break
- How to verify rollback worked

## Time Estimation

Use this formula:
- Task coding: X minutes
- Task testing: X/2 minutes
- Task documentation: X/4 minutes
- **Total: X * 1.75 minutes**

Then add 25% buffer for unexpected issues.

## Common Mistakes to Avoid

1. **Tasks without test procedures** → Can't verify completion
2. **Vague success criteria** → Don't know when done
3. **Missing file paths** → task-executor doesn't know what to modify
4. **No edge cases** → Bugs in production
5. **Tasks too large** → Get stuck or overwhelmed
6. **No "Why" explanation** → Future maintainers confused

## Template Checklist

Before starting execution, verify your plan has:
- [ ] Clear phase goals
- [ ] Specific file paths in every task
- [ ] Testable success criteria
- [ ] Step-by-step test procedures
- [ ] Edge cases identified
- [ ] Time estimates
- [ ] Dependencies documented
- [ ] Rollback strategies

## Using the Plan

Once created:
````bash
# In Claude Code terminal
/execute-plan @_docs/plans/your-plan.md
````

The plan-coordinator will:
1. Parse your plan
2. Execute each task via task-executor
3. Update checkboxes as tasks complete
4. Report progress
5. Handle blockers

Your job:
- Monitor progress
- Respond to blocker questions
- Approve phase transitions
- Test final integration