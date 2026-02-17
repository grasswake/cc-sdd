---
description: Generate change implementation tasks
allowed-tools: Read, Task
argument-hint: <feature-name> [-y] [--sequential]
---

# Change Tasks Generator

## Parse Arguments
- Feature name: `$1`
- Auto-approve flag: `$2` (optional, "-y")
- Sequential mode flag: `$3` (optional, "--sequential")

## Validate
Check that change design has been completed:
- Verify `{{KIRO_DIR}}/specs/$1/` exists
- Verify `{{KIRO_DIR}}/specs/$1/change-design.md` exists
- Determine `sequential = ($3 == "--sequential")`

If validation fails, inform user to complete change design phase first.

## Invoke Subagent

Delegate change task generation to spec-change-tasks-agent:

Use the Task tool to invoke the Subagent with file path patterns:

```
Task(
  subagent_type="spec-change-tasks-agent",
  description="Generate change implementation tasks",
  prompt="""
Feature: $1
Spec directory: {{KIRO_DIR}}/specs/$1/
Auto-approve: {true if $2 == "-y", else false}
Sequential mode: {true if sequential else false}

File patterns to read:
- {{KIRO_DIR}}/specs/$1/*.{json,md}
- {{KIRO_DIR}}/steering/*.md
- {{KIRO_DIR}}/settings/rules/tasks-generation.md
- {{KIRO_DIR}}/settings/rules/tasks-parallel-analysis.md (include only when sequential mode is false)
- {{KIRO_DIR}}/settings/templates/specs/change-tasks.md

Instruction highlights:
- Use C-prefix for all task numbers (C1, C1.1, C2, etc.)
- Map each change item from change-request.md to specific tasks
- Include regression prevention tasks for affected components
- Apply `(P)` markers only when parallel criteria met (omit in sequential mode)
- Do NOT modify original tasks.md
"""
)
```

## Display Result

Show Subagent summary to user, then provide next step guidance:

### Next Phase: Change Implementation

**Before Starting Implementation**:
- **IMPORTANT**: Clear conversation history and free up context before running `/kiro:spec-change-impl`

**If Change Tasks Approved**:
- Execute specific task: `/kiro:spec-change-impl $1 C1.1`
- Execute multiple: `/kiro:spec-change-impl $1 C1,C2`
- Execute all: `/kiro:spec-change-impl $1`

**If Modifications Needed**:
- Provide feedback and re-run `/kiro:spec-change-tasks $1`

**Note**: Change task approval is recommended before implementation.
