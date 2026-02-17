---
description: Validate requirements for completeness and quality
allowed-tools: Read, Task
argument-hint: <feature-name>
---

# Requirements Validation

## Parse Arguments

- Feature name: `$1`

## Validate

Check that requirements have been completed:

- Verify `{{KIRO_DIR}}/specs/$1/` exists
- Verify `{{KIRO_DIR}}/specs/$1/requirements.md` exists

If validation fails, inform user to complete requirements phase first.

## Invoke Subagent

Delegate requirements validation to validate-requirements-agent:

Use the Task tool to invoke the Subagent with file path patterns:

```
Task(
  subagent_type="validate-requirements-agent",
  description="Interactive requirements review",
  prompt="""
Feature: $1
Spec directory: {{KIRO_DIR}}/specs/$1/

File patterns to read:
- {{KIRO_DIR}}/specs/$1/spec.json
- {{KIRO_DIR}}/specs/$1/requirements.md
- {{KIRO_DIR}}/steering/*.md
- {{KIRO_DIR}}/settings/rules/ears-format.md
"""
)
```

## Display Result

Show Subagent summary to user, then provide next step guidance:

### Next Phase

**If GO**:

- Requirements validated and ready for design
- **Optional**: Run `/kiro:validate-gap $1` for implementation gap analysis (brownfield projects)
- Then: Run `/kiro:spec-design $1 -y` to proceed to design phase

**If NO-GO**:

- Address critical issues identified
- Re-run `/kiro:spec-requirements $1` with improvements
- Re-validate with `/kiro:validate-requirements $1`

**Note**: Requirements validation is optional but recommended before proceeding to design.
