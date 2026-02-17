---
description: Validate change implementation against change specs
allowed-tools: Read, Task
argument-hint: <feature-name> [task-numbers]
---

# Change Implementation Validation

## Parse Arguments
- Feature name: `$1` (required)
- Task numbers: `$2` (optional)

## Auto-Detection Logic

**Perform detection before invoking Subagent**:

**If no task arguments** (`$2` empty):
- Detect all completed change tasks `[x]` in `{{KIRO_DIR}}/specs/$1/change-tasks.md`
- Pass feature and detected tasks to Subagent

**If task numbers provided** (`$2` present):
- Pass directly to Subagent without detection

## Invoke Subagent

Delegate validation to validate-change-impl-agent:

Use the Task tool to invoke the Subagent with file path patterns:

```
Task(
  subagent_type="validate-change-impl-agent",
  description="Validate change implementation",
  prompt="""
Feature: {$1}
Target tasks: {$2 or auto-detected}
Mode: {feature-all or explicit}

File patterns to read:
- {{KIRO_DIR}}/specs/$1/*.{json,md}
- {{KIRO_DIR}}/steering/*.md

Validation scope: {based on detection results}
"""
)
```

## Display Result

Show Subagent summary to user, then provide next step guidance:

### Next Steps

**If GO**:
- Run `/kiro:spec-archive-change $1` to archive change and update original specs

**If NO-GO**:
- Address critical issues (especially regressions)
- Re-run `/kiro:spec-change-impl $1 [tasks]` for fixes
- Re-validate with `/kiro:spec-validate-change-impl $1`

**Note**: Change implementation validation is recommended before archiving to ensure spec alignment and quality.
