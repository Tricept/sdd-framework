---
name: sdd-implement
description: Phase 5 - TDD implementation (Red-Green-Refactor)
tools:
  - codebase
  - terminal
  - search/codebase
handoffs:
  - label: Validate Implementation
    agent: sdd-validate
    prompt: Verify implementation matches the specification
    send: true
---

# TDD Implementation

Implement following strict Red-Green-Refactor.

## Prerequisites

- Spec: `specs/active/[folder]/spec.md`
- Tasks: `specs/active/[folder]/tasks.md`
- User has reviewed and approved both

---

## Step 1: Load Context

Read these files before starting:

1. **Specification**: `specs/active/[folder]/spec.md`
   - Functional requirements to implement
   - Test strategy to follow
   - Existing code to leverage

2. **Task Breakdown**: `specs/active/[folder]/tasks.md`
   - Ordered tasks to follow
   - Dependencies between tasks
   - Acceptance criteria

3. **Standards**:
   - `standards/global/code-quality.md`
   - `standards/global/testing.md`

### Identify Progress

Check tasks.md for:
- Completed tasks: `[x]`
- Next task to work on
- Current phase

---

## Step 2: TDD Cycle (NON-NEGOTIABLE)

```
┌─────────────────────────────────────────────────────────┐
│  RED: Write a failing test                              │
│  - Test defines expected behavior                       │
│  - Test MUST fail before implementation                 │
│  - Commit: "test: add failing test for [behavior]"      │
├─────────────────────────────────────────────────────────┤
│  GREEN: Write minimal code to pass                      │
│  - Only implement what's needed                         │
│  - No premature optimization                            │
│  - Commit: "feat: implement [behavior] to pass test"    │
├─────────────────────────────────────────────────────────┤
│  REFACTOR: Improve while keeping tests green            │
│  - Clean up duplication                                 │
│  - Improve naming and structure                         │
│  - Tests MUST still pass                                │
│  - Commit: "refactor: improve [component]"              │
└─────────────────────────────────────────────────────────┘
```

---

## Step 3: Execute Tasks

For each task in order:

### 1. Read the Task
- Understand acceptance criteria
- Check dependencies (must be complete)
- Identify related spec requirements (FR-X.X)

### 2. Execute Based on Task Type

**[TEST] Task:**
```
├── Write the test
├── Run test → Verify it FAILS (this is expected!)
├── Commit: "test: [description]"
└── Report: Test written, failing as expected
```

**[IMPL] Task (DB/API/UI):**
```
├── Reference the related [TEST] task
├── Write minimal implementation to pass
├── Run tests → Verify they PASS
├── Commit: "feat: [description]"
└── Report: Implementation complete, tests passing
```

**[VERIFY] Task:**
```
├── Run full test suite for phase
├── Check coverage
└── Report: Phase complete, all tests green
```

### 3. Update tasks.md
- Mark task complete: `[x]`
- Add completion date and notes if needed

### 4. Continue to Next Task

---

## Implementation Guidelines

### Writing Tests (RED)

**DO:**
- Test behavior, not implementation
- Use descriptive test names: `test_[unit]_[scenario]_[expected]`
- One logical assertion per test
- Include edge cases from spec

**DON'T:**
- Test implementation details
- Write tests that pass immediately
- Skip edge cases
- Over-mock dependencies

### Writing Implementation (GREEN)

**DO:**
- Write ONLY what's needed to pass tests
- Reference existing code patterns from spec
- Follow code quality standards
- Keep functions small and focused

**DON'T:**
- Add features not in spec
- Optimize prematurely
- Add "nice to have" code
- Ignore existing patterns

### Refactoring

**DO:**
- Remove duplication
- Improve naming
- Extract helper functions
- Run tests after each change

**DON'T:**
- Add new features
- Change behavior
- Skip running tests

---

## Scope Control (CRITICAL)

### Before Implementing Anything

Ask yourself:
1. Is this in the spec?
2. Is this in the task list?
3. Does this address an FR-X.X requirement?

**If NO to any → Don't implement it.**

### Out of Scope Enforcement

The spec's "Out of Scope" section is **binding**:

- ❌ Do NOT implement out-of-scope items
- ❌ Do NOT add "improvements" beyond spec
- ❌ Do NOT refactor unrelated code
- ❌ No "while I'm here" additions

If you discover something that should be added:
1. Note it in tasks.md under "Notes"
2. Continue with current spec
3. Create separate ticket/spec later

---

## Error Handling

### Test Failures

If tests fail unexpectedly:
1. Check if test is correct (matches spec)
2. Check if implementation matches test
3. Fix the issue
4. Re-run tests
5. Continue only when green

### Blocked by Dependencies

If a task is blocked:
1. Check if dependency task is complete
2. If not, complete dependency first
3. If external blocker, note in tasks.md and ask user

---

## Progress Tracking

### Update tasks.md After Each Task

```markdown
- [x] T004 [TEST] [DB] Write schema tests ✓ 2025-01-29
      └── Note: 3 tests covering constraints
```

### Report Progress After Each Phase

```
## Phase [X] Complete

### Tasks Completed
- [x] T004: Schema tests
- [x] T005: Validation tests
- [x] T006: Migration
- [x] T007: Model

### Test Results
- Tests: [X] passing, 0 failing
- Coverage: [X]%

### Next Phase
Phase [X+1]: [Name]
Starting with: T[next]
```

---

## Output

After implementation session:

```
## Implementation Progress

**Spec**: `specs/active/[folder]/spec.md`
**Tasks**: `specs/active/[folder]/tasks.md`

### Session Summary
| Phase | Status | Tests | Coverage |
|-------|--------|-------|----------|
| Setup | Complete | - | - |
| Database | Complete | [X] pass | [X]% |
| Backend | Complete | [X] pass | [X]% |
| Frontend | In Progress | [X] pass | [X]% |
| Integration | Pending | - | - |
| Polish | Pending | - | - |

### Tasks This Session
- [x] T014: Component tests
- [x] T015: Interaction tests
- [x] T016: Implement component
- [ ] T017: API integration (next)

### Commits
[List commits made]

### Next Steps
[What to do next]
```

When all tasks complete, proceed to validation.
