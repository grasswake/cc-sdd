---
description: Create change design with integrated requirements and design
auto_execution_mode: 3
---
<meta>
description: Create change design with integrated requirements and design
argument-hint: <feature-name:$1> [-y:$2]
</meta>

# Change Design Generator

<background_information>
- **Mission**: Generate a change design document that includes integrated (post-change) versions of both requirements and design, plus delta analysis
- **Success Criteria**:
  - All change request items mapped to design changes
  - Integrated requirements accurately merge current + changes with markers
  - Integrated design accurately merges current + changes with markers
  - Impact analysis is comprehensive
  - Discovery research conducted as appropriate
</background_information>

<instructions>
## Core Task
Generate change design document for feature **$1** based on an approved change request.

## Execution Steps

### Step 1: Load Context

**Read all necessary context**:
- `{{KIRO_DIR}}/specs/$1/spec.json` for language and metadata
- `{{KIRO_DIR}}/specs/$1/change-request.md` for change details
- `{{KIRO_DIR}}/specs/$1/requirements.md` for current requirements
- `{{KIRO_DIR}}/specs/$1/design.md` for current design
- `{{KIRO_DIR}}/specs/$1/research.md` (if exists) for prior research
- **Entire `{{KIRO_DIR}}/steering/` directory** for complete project memory

**Validate preconditions**:
- `change.active` must be `true` in spec.json
- `change.phase` must be `"change-request-generated"`
- If `-y` flag provided ($2 == "-y"): Auto-approve change request
- Otherwise: Verify `change.approvals.change_request.approved` is `true`

### Step 2: Read Guidelines

- Read `{{KIRO_DIR}}/settings/rules/ears-format.md` for EARS syntax rules
- Read `{{KIRO_DIR}}/settings/rules/design-principles.md` for design principles
- Read `{{KIRO_DIR}}/settings/templates/specs/change-design.md` for document structure

### Step 3: Discovery & Analysis

**Classify change complexity**:
- **Light Discovery**: Simple modifications, UI changes, parameter adjustments
- **Full Discovery**: New integrations, architectural changes, new dependencies

**Execute appropriate discovery**:
- For complex changes: Read `{{KIRO_DIR}}/settings/rules/design-discovery-full.md`
- For simple changes: Read `{{KIRO_DIR}}/settings/rules/design-discovery-light.md`
- Analyze affected codebase areas with Grep
- Update `{{KIRO_DIR}}/specs/$1/research.md` if significant findings

### Step 4: Generate Change Design

Using the template structure, create `{{KIRO_DIR}}/specs/$1/change-design.md`:

1. **Change Overview**: Reference CR number and scope
2. **Integrated Requirements (Post-Change)**:
   - Start from current requirements.md
   - Apply all modifications from change-request.md
   - Mark changed items with `[Changed in CR-NNN]`
   - Mark new items with `[Added in CR-NNN]`
   - Omit removed items (list them in Removed Requirements section)
   - Result must be a complete, self-contained requirements document
3. **Impact Analysis**: Affected and unaffected components with details
4. **Design Changes (Delta)**:
   - For each affected area: current design vs. updated design with rationale
   - Focus on what changes, not what stays the same
5. **Integrated Design (Post-Change)**:
   - Start from current design.md
   - Apply all design changes
   - Mark changed sections with `[Changed in CR-NNN]`
   - Mark new sections with `[Added in CR-NNN]`
   - Result must be a complete, self-contained design document

Use language specified in spec.json.

### Step 5: Update Metadata

Update spec.json:
- Set `change.phase: "change-design-generated"`
- Set `change.approvals.change_design.generated: true`
- If `-y` flag: Set `change.approvals.change_request.approved: true`
- Update `updated_at` timestamp

## Critical Constraints
- **Integrated documents must be complete**: They will replace originals during archive
- **Type Safety**: Follow the same type safety rules as spec-design (no `any` in TypeScript, etc.)
- **Markers are mandatory**: Every change must have a CR marker for traceability
- **Do NOT modify** requirements.md or design.md directly at this stage
- **Requirements Traceability IDs**: Use numeric requirement IDs only, matching requirements.md

### Language Reminder
- Markdown prompt content must remain in English, even when spec.json requests another language for design output. The generated change-design.md should use the spec language.
</instructions>

## Tool Guidance
- **Read first**: Load all context before generation
- **Research when uncertain**: Use WebSearch/WebFetch for external dependencies
- **Grep**: Analyze existing codebase patterns and integration points
- **Write last**: Generate change-design.md only after all analysis complete

## Output Description
Provide brief summary in the language specified in spec.json:

1. **Status**: Confirm change design generated at `{{KIRO_DIR}}/specs/$1/change-design.md`
2. **Discovery Type**: Which discovery process was executed (full/light)
3. **Integration Summary**: Number of changed/added/removed requirements, affected design areas
4. **Next Action**: Approval workflow guidance

**Format**: Concise Markdown (under 200 words)

## Safety & Fallback

### Error Scenarios

**No Active Change**:
- **Stop Execution**: `change.active` must be `true`
- **Suggested Action**: "Run `/kiro-spec-change $1 \"description\"` to create a change request first"

**Change Request Not Approved**:
- **Stop Execution**: Cannot proceed without approved change request
- **Suggested Action**: "Run `/kiro-spec-change-design $1 -y` to auto-approve and proceed"

**Missing Requirements or Design**:
- **Stop Execution**: Both documents must exist
- **Suggested Action**: "Complete original specification first"

**Template/Rules Missing**:
- **Fallback**: Use inline basic structure with warning

**Language Undefined**: Default to English (`en`)

### Next Phase: Change Tasks

**If Change Design Approved**:
- Review generated change design at `{{KIRO_DIR}}/specs/$1/change-design.md`
- **Optional**: Run `/kiro-spec-validate-change-design $1` for quality review
- Then `/kiro-spec-change-tasks $1` to generate change implementation tasks

**If Modifications Needed**:
- Provide feedback and re-run `/kiro-spec-change-design $1`

**Note**: Change design approval is recommended before proceeding to change tasks.
