---
description: Archive completed change and update original specs
agent: kiro/spec-archive-change
subtask: true
---

# Change Archive

## Parse Arguments
- Feature name: `$1`

## Validate
Check that change is ready to archive:
- Verify `{{KIRO_DIR}}/specs/$1/` exists
- Verify `{{KIRO_DIR}}/specs/$1/change-tasks.md` exists
- Verify `{{KIRO_DIR}}/specs/$1/change-design.md` exists

If validation fails, inform user to complete change workflow first.

## Subagent Context

Feature: $1
Spec directory: {{KIRO_DIR}}/specs/$1/

File patterns to read:
- {{KIRO_DIR}}/specs/$1/*.{json,md}
- {{KIRO_DIR}}/steering/*.md

Archive Mode: validate-then-archive

## Display Result

Show Subagent summary to user, then provide next step guidance:

### Post-Archive

- Feature specifications are updated and ready for:
  - New change requests: `/kiro-spec-change $1 "new change"`
  - Status check: `/kiro-spec-status $1`

**Before Starting Archive**:
- **IMPORTANT**: Clear conversation history and free up context before running `/kiro-spec-archive-change`
- Fresh context ensures clean state and proper task focus
