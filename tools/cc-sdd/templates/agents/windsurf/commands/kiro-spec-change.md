---
description: Create a change request for an existing specification
auto_execution_mode: 3
---
<meta>
description: Create a change request for an existing specification
argument-hint: <feature-name:$1> <change-specification:$2>
</meta>

# Change Request Generator

<background_information>
- **Mission**: Generate a structured change request document that captures what needs to change in an existing specification, why, and what areas are impacted
- **Success Criteria**:
  - Change request clearly describes current vs. changed behavior
  - New/modified requirements follow EARS format
  - Impact analysis identifies all affected and unaffected components
  - spec.json updated with active change tracking
</background_information>

<instructions>
## Core Task
Generate a change request document for feature **$1** based on the change specification: **$2**.

## Execution Steps

### Step 1: Load Context

**Read all necessary context**:
- `{{KIRO_DIR}}/specs/$1/spec.json` for language and metadata
- `{{KIRO_DIR}}/specs/$1/requirements.md` for current requirements
- `{{KIRO_DIR}}/specs/$1/design.md` for current design
- **Entire `{{KIRO_DIR}}/steering/` directory** for complete project memory

**Validate preconditions**:
- Feature must exist with completed implementation (tasks approved)
- No active change already in progress (`change.active` must not be `true` in spec.json)

### Step 2: Read Guidelines

- Read `{{KIRO_DIR}}/settings/rules/ears-format.md` for EARS syntax rules
- Read `{{KIRO_DIR}}/settings/templates/specs/change-request.md` for document structure

### Step 3: Determine Change Number

- Check spec.json for previous `change.change_number`
- If exists, increment by 1; otherwise start at 1
- Format as 3-digit zero-padded number for directory names (e.g., 001)

### Step 4: Analyze Change Scope

- Parse the change specification (`$2`) against current requirements and design
- Identify which existing requirements are modified, which are new, which are removed
- Analyze codebase with Grep to understand affected components
- Determine impact on unaffected components (confirm no impact)

### Step 5: Generate Change Request

Using the template structure, create `{{KIRO_DIR}}/specs/$1/change-request.md`:
- **Change Overview**: Purpose and background from the change specification
- **Change Details**: Current vs. changed behavior for each modification
- **New Requirements**: In EARS format with numeric IDs continuing from existing
- **Removed Requirements**: With removal rationale
- **Impact Analysis**: Affected components with impact type, unaffected components with rationale
- **Change Rationale**: Overall justification

Use language specified in spec.json.

### Step 6: Update Metadata

Update spec.json with `change` section:
```json
{
  "change": {
    "active": true,
    "change_number": N,
    "phase": "change-request-generated",
    "approvals": {
      "change_request": { "generated": true, "approved": false },
      "change_design": { "generated": false, "approved": false },
      "change_tasks": { "generated": false, "approved": false }
    },
    "description": "Change description summary"
  }
}
```
Update `updated_at` timestamp.

## Important Constraints
- Do NOT modify requirements.md or design.md at this stage
- New requirements must use EARS format and numeric IDs
- Impact analysis must be comprehensive (both affected AND unaffected)
- Change request is a proposal document, not yet integrated

### Language Reminder
- Markdown prompt content must remain in English, even when spec.json requests another language for design output. The generated change-request.md should use the spec language.
</instructions>

## Tool Guidance
- **Read first**: Load all context (spec, steering, rules, templates) before generation
- **Grep**: Analyze codebase for affected components
- **Write last**: Create change-request.md only after complete analysis
- Use **WebSearch/WebFetch** only if external domain knowledge needed

## Output Description
Provide output in the language specified in spec.json with:

1. **Change Request Summary**: CR number, purpose, scope (3-5 bullets)
2. **Impact Overview**: Number of modified/new/removed requirements, affected components
3. **Document Status**: Confirm change-request.md created and spec.json updated
4. **Next Steps**: Guide user on review and proceeding

**Format**: Concise Markdown (under 300 words)

## Safety & Fallback

### Error Scenarios
- **Feature Not Found**: If spec directory doesn't exist, stop with message: "No spec found for `$1`. Run `/kiro-spec-init` first."
- **Active Change Exists**: If `change.active` is `true`, stop with message: "Active change CR-N already in progress. Complete or archive it first with `/kiro-spec-archive-change $1`."
- **Missing Change Specification**: If `$2` is empty, ask user to describe the desired change
- **Requirements Missing**: If requirements.md doesn't exist, stop with message: "Complete specification first."
- **Language Undefined**: Default to English (`en`) if spec.json doesn't specify language

### Next Phase: Change Design

**If Change Request Approved**:
- Review generated change request at `{{KIRO_DIR}}/specs/$1/change-request.md`
- **Optional**: Run `/kiro-spec-validate-change $1` for change request validation
- Then `/kiro-spec-change-design $1` to generate change design with integrated requirements/design

**If Modifications Needed**:
- Provide feedback and re-run `/kiro-spec-change $1 "updated specification"`

**Note**: Change request approval is recommended before proceeding to change design.
