---
description: Validate change design for completeness and integration accuracy
agent: kiro/validate-change-design
subtask: true
---

# Change Design Validation

## Parse Arguments
- Feature name: `$1`

## Validate
Check that change design has been completed:
- Verify `{{KIRO_DIR}}/specs/$1/` exists
- Verify `{{KIRO_DIR}}/specs/$1/change-design.md` exists

If validation fails, inform user to complete change design phase first.

## Subagent Context

Feature: $1
Spec directory: {{KIRO_DIR}}/specs/$1/

File patterns to read:
- {{KIRO_DIR}}/specs/$1/*.{json,md}
- {{KIRO_DIR}}/steering/*.md
- {{KIRO_DIR}}/settings/rules/design-*.md

## Display Result

Show Subagent summary to user, then provide next step guidance:

### Next Phase: Change Tasks

**If Change Design Passes Validation (GO Decision)**:
- Review feedback and apply changes if needed
- Run `/kiro-spec-change-tasks $1` to generate change implementation tasks

**If Change Design Needs Revision (NO-GO Decision)**:
- Address critical issues identified
- Re-run `/kiro-spec-change-design $1` with improvements
- Re-validate with `/kiro-validate-change-design $1`

**Note**: Change design validation is recommended but optional. Quality review helps catch integration issues early.