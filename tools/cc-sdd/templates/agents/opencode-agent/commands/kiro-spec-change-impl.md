---
description: Execute change tasks using TDD with regression focus
agent: kiro/spec-change-impl
subtask: true
---

# Change Implementation Executor

## Parse Arguments
- Feature name: `$1`
- Task numbers: `$2` (optional)
  - Format: "C1.1" (single task) or "C1,C2,C3" (multiple tasks)
  - If not provided: Execute all pending change tasks

## Validate
Check that change tasks have been generated:
- Verify `{{KIRO_DIR}}/specs/$1/` exists
- Verify `{{KIRO_DIR}}/specs/$1/change-tasks.md` exists

If validation fails, inform user to complete change tasks generation first.

## Task Selection Logic

**Parse task numbers from `$2`** (perform this in Slash Command before invoking Subagent):
- If `$2` provided: Parse task numbers (e.g., "C1.1", "C1,C2,C3")
- Otherwise: Read `{{KIRO_DIR}}/specs/$1/change-tasks.md` and find all unchecked tasks (`- [ ]`)

## Subagent Context

Feature: $1
Spec directory: {{KIRO_DIR}}/specs/$1/
Target tasks: {parsed task numbers or "all pending"}

File patterns to read:
- {{KIRO_DIR}}/specs/$1/*.{json,md}
- {{KIRO_DIR}}/steering/*.md

TDD Mode: strict (test-first, regression-focused)

## Display Result

Show Subagent summary to user, then provide next step guidance:

### Task Execution

**Execute specific task(s)**:
- `/kiro-spec-change-impl $1 C1.1` - Single task
- `/kiro-spec-change-impl $1 C1,C2` - Multiple tasks

**Execute all pending**:
- `/kiro-spec-change-impl $1` - All unchecked change tasks

**Before Starting Implementation**:
- **IMPORTANT**: Clear conversation history and free up context before running `/kiro-spec-change-impl`
- This applies when starting first task OR switching between tasks
- Fresh context ensures clean state and proper task focus
