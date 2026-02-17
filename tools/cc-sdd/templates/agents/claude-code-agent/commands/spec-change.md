---
description: Create a change request for an existing specification
allowed-tools: Read, Task
argument-hint: <feature-name> <change-specification>
---

# Change Request Generator

## Parse Arguments

- Feature name: `$1`
- Change specification: `$2`

## Validate

Check that specification has been completed:

- Verify `{{KIRO_DIR}}/specs/$1/` exists
- Verify `{{KIRO_DIR}}/specs/$1/requirements.md` exists

If validation fails, inform user to complete specification phase first.

## Invoke Subagent

Delegate change request generation to spec-change-agent:

Use the Task tool to invoke the Subagent with file path patterns:

```
Task(
  subagent_type="spec-change-agent",
  description="Generate change request and update spec metadata",
  prompt="""
Feature: $1
Spec directory: {{KIRO_DIR}}/specs/$1/
Change specification: $2

File patterns to read:
- {{KIRO_DIR}}/specs/$1/*.{json,md}
- {{KIRO_DIR}}/steering/*.md
- {{KIRO_DIR}}/settings/rules/ears-format.md
- {{KIRO_DIR}}/settings/templates/specs/change-request.md

Language: respect spec.json language for change-request.md output
"""
)
```

## Display Result

Show Subagent summary to user, then provide next step guidance:

### Next Phase: Change Design

**If Change Request Approved**:

- Review generated change request at `{{KIRO_DIR}}/specs/$1/change-request.md`
- **Optional**: Run `/kiro:validate-change $1` for change request validation
- Then `/kiro:spec-change-design $1` to generate change design with integrated requirements/design

**If Modifications Needed**:

- Provide feedback and re-run `/kiro:spec-change $1 "updated specification"`

**Note**: Change request approval is recommended before proceeding to change design.
