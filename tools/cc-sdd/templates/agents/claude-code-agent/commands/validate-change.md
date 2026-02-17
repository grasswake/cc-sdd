---
description: Validate a change request for completeness and consistency
allowed-tools: Read, Task
argument-hint: <feature-name>
---

# Change Request Validation

## Parse Arguments
- Feature name: `$1`

## Validate
Check that change request has been created:
- Verify `{{KIRO_DIR}}/specs/$1/` exists
- Verify `{{KIRO_DIR}}/specs/$1/change-request.md` exists

If validation fails, inform user to run `/kiro:spec-change $1` first.

## Invoke Subagent

Delegate change request validation to validate-change-agent:

Use the Task tool to invoke the Subagent with file path patterns:

```
Task(
  subagent_type="validate-change-agent",
  description="Change request validation review",
  prompt="""
Feature: $1
Spec directory: {{KIRO_DIR}}/specs/$1/

File patterns to read:
- {{KIRO_DIR}}/specs/$1/*.{json,md}
- {{KIRO_DIR}}/steering/*.md
- {{KIRO_DIR}}/settings/rules/ears-format.md
"""
)
```

## Display Result

Show Subagent summary to user, then provide next step guidance:

### Next Phase

**If GO**: Proceed to `/kiro:spec-change-design $1`
**If NO-GO**: Address issues and re-run `/kiro:spec-change $1 "updated description"`

**Note**: Change request validation is recommended but optional. Quality review helps catch issues early.
