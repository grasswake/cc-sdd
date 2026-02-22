# Change Request: CR-{{CHANGE_NUMBER}}

## Change Overview

- **Feature**: `{{FEATURE_NAME}}`
- **Change Number**: CR-{{CHANGE_NUMBER}}
- **Date**: {{TIMESTAMP}}
- **Purpose**: {{CHANGE_PURPOSE}}
- **Background**: {{CHANGE_BACKGROUND}}

## Change Details

### Modified Requirements

> Both "Current (EARS)" and "After Change (EARS)" columns MUST use EARS syntax.
> EARS patterns: `When [event]` / `While [precondition]` / `If [trigger]` / `Where [feature]` + `, the [system] shall [action]` — or `The [system] shall [action]` for ubiquitous requirements.
> Do NOT write plain-text summaries. Always use a full EARS statement.

| # | Requirement ID | Current (EARS) | After Change (EARS) | Reason |
|---|---------------|----------------|---------------------|--------|
| 1 | {{REQ_ID}} | When [event], the [system] shall [current action] | When [event], the [system] shall [changed action] | {{REASON}} |

### New Requirements

> ALL new requirement descriptions MUST use EARS format. Assign numeric IDs continuing from existing requirements.
> EARS patterns: `When [event]` / `While [precondition]` / `If [trigger]` / `Where [feature]` + `, the [system] shall [action]` — or `The [system] shall [action]` for ubiquitous requirements.
> Do NOT write plain-text summaries (e.g., "Add dark mode support"). Always use a full EARS statement.

| # | Requirement ID | Description (EARS) | Rationale |
|---|---------------|-------------------|----------|
| 1 | {{NEW_REQ_ID}} | When [event], the [system] shall [response/action] | {{RATIONALE}} |

### Removed Requirements

| # | Requirement ID | Description | Removal Reason |
|---|---------------|-------------|---------------|
| 1 | {{DEL_REQ_ID}} | {{DESCRIPTION}} | {{REASON}} |

## Impact Analysis

### Affected Components

| Component | Impact Type | Description |
|-----------|-----------|-------------|
| {{COMPONENT}} | Modified / Extended / Removed | {{IMPACT_DESCRIPTION}} |

### Unaffected Components

| Component | Reason for No Impact |
|-----------|---------------------|
| {{COMPONENT}} | {{REASON}} |

## Change Rationale

{{OVERALL_RATIONALE}}
