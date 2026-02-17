# Change Implementation Plan: CR-{{CHANGE_NUMBER}}

## Change Reference

- **Change Request**: CR-{{CHANGE_NUMBER}}
- **Feature**: `{{FEATURE_NAME}}`
- **Scope**: {{CHANGE_SCOPE_SUMMARY}}

## Change Task Format

Use whichever pattern fits the work breakdown:

### Major task only
- [ ] C{{NUMBER}}. {{TASK_DESCRIPTION}}{{PARALLEL_MARK}}
  - {{DETAIL_ITEM_1}} *(Include details only when needed. If the task stands alone, omit bullet items.)*
  - _Requirements: {{REQUIREMENT_IDS}}_
  - _Change: {{CHANGE_TYPE: Modified / Added / Removed}}_

### Major + Sub-task structure
- [ ] C{{MAJOR_NUMBER}}. {{MAJOR_TASK_SUMMARY}}
- [ ] C{{MAJOR_NUMBER}}.{{SUB_NUMBER}} {{SUB_TASK_DESCRIPTION}}{{SUB_PARALLEL_MARK}}
  - {{DETAIL_ITEM_1}}
  - {{DETAIL_ITEM_2}}
  - _Requirements: {{REQUIREMENT_IDS}}_ *(IDs only; do not add descriptions or parentheses.)*
  - _Change: {{CHANGE_TYPE}}_

> **Task numbering**: Use `C` prefix (C1, C1.1, C2, etc.) to distinguish change tasks from original tasks.
>
> **Parallel marker**: Append ` (P)` only to tasks that can be executed in parallel. Omit the marker when running in `--sequential` mode.

## Change Tasks

{{CHANGE_TASKS}}

## Regression Prevention Tasks

> Tasks to verify that existing functionality is not broken by the changes.

{{REGRESSION_TASKS}}
