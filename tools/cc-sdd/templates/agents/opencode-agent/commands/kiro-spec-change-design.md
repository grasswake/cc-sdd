---
description: Create change design with integrated requirements and design
agent: kiro/spec-change-design
subtask: true
---

# Change Design Generator

## Parse Arguments
- Feature name: `$1`
- Auto-approve flag: `$2` (optional, "-y")

## Validate
Check that change request has been completed:
- Verify `{{KIRO_DIR}}/specs/$1/` exists
- Verify `{{KIRO_DIR}}/specs/$1/change-request.md` exists
- Verify `{{KIRO_DIR}}/specs/$1/requirements.md` exists
- Verify `{{KIRO_DIR}}/specs/$1/design.md` exists

If validation fails, inform user to complete change request phase first.

## Subagent Context

Feature: $1
Spec directory: {{KIRO_DIR}}/specs/$1/
Auto-approve: {true if $2 == "-y", else false}

File patterns to read:
- {{KIRO_DIR}}/specs/$1/*.{json,md}
- {{KIRO_DIR}}/steering/*.md
- {{KIRO_DIR}}/settings/rules/design-*.md
- {{KIRO_DIR}}/settings/rules/ears-format.md
- {{KIRO_DIR}}/settings/templates/specs/change-design.md

Discovery: auto-detect based on change complexity
Language: respect spec.json language for change-design.md outputs

## Display Result

Show Subagent summary to user, then provide next step guidance:

### Next Phase: Change Tasks

**If Change Design Approved**:
- Review generated change design at `{{KIRO_DIR}}/specs/$1/change-design.md`
- **Optional**: Run `/kiro-validate-change-design $1` for quality review
- Then `/kiro-spec-change-tasks $1` to generate change implementation tasks

**If Modifications Needed**:
- Provide feedback and re-run `/kiro-spec-change-design $1`

**Note**: Change design approval is recommended before proceeding to change tasks.