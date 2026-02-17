---
description: Execute change tasks using TDD with regression focus
allowed-tools: Read, Task
argument-hint: <feature-name> [task-numbers]
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

## Invoke Subagent

Delegate TDD change implementation to spec-change-impl-agent:

Use the Task tool to invoke the Subagent with file path patterns:

```
Task(
  subagent_type="spec-change-impl-agent",
  description="Execute TDD change implementation",
  prompt="""
Feature: $1
Spec directory: {{KIRO_DIR}}/specs/$1/
Target tasks: {parsed task numbers or "all pending"}

File patterns to read:
- {{KIRO_DIR}}/specs/$1/*.{json,md}
- {{KIRO_DIR}}/steering/*.md

TDD Mode: strict (test-first, regression-focused)
"""
)
```

## Display Result

Show Subagent summary to user, then provide next step guidance:

### Task Execution

**Execute specific task(s)**:
- `/kiro:spec-change-impl $1 C1.1` - Single task
- `/kiro:spec-change-impl $1 C1,C2` - Multiple tasks

**Execute all pending**:
- `/kiro:spec-change-impl $1` - All unchecked change tasks

**Before Starting Implementation**:
- **IMPORTANT**: Clear conversation history and free up context before running `/kiro:spec-change-impl`
- This applies when starting first task OR switching between tasks
- Fresh context ensures clean state and proper task focus
