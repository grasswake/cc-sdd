---
description: Validate change design for completeness and integration accuracy
auto_execution_mode: 3
---
<meta>
description: Validate change design for completeness and integration accuracy
argument-hint: <feature-name:$1>
</meta>

# Change Design Validation

<background_information>
- **Mission**: Verify that change design accurately integrates changes, maintains design consistency, and produces valid integrated documents
- **Success Criteria**:
  - All change request items reflected in design changes
  - Integrated requirements are accurate and complete
  - Integrated design is accurate and complete
  - Type safety and design principles maintained
  - Clear GO/NO-GO decision with maximum 3 critical issues
</background_information>

<instructions>
## Core Task
Validate change design for feature **$1** against change request, existing specifications, and codebase.

## Execution Steps

1. **Load Context**:
   - Read `{{KIRO_DIR}}/specs/$1/spec.json` for language and metadata
   - Read `{{KIRO_DIR}}/specs/$1/change-request.md` for change request
   - Read `{{KIRO_DIR}}/specs/$1/change-design.md` for change design
   - Read `{{KIRO_DIR}}/specs/$1/requirements.md` for current requirements
   - Read `{{KIRO_DIR}}/specs/$1/design.md` for current design
   - **Load ALL steering context**: Read entire `{{KIRO_DIR}}/steering/` directory

2. **Read Review Guidelines**:
   - Read `{{KIRO_DIR}}/settings/rules/design-review.md` for review criteria
   - Read `{{KIRO_DIR}}/settings/rules/design-principles.md` for design principles

3. **Execute Validation**:

   #### Change Request Coverage
   - Every item in change-request.md is addressed in change-design.md
   - No change items are missed or only partially addressed
   - Modified, added, and removed items all accounted for

   #### Integrated Requirements Accuracy
   - Integrated requirements correctly merge original + changes
   - `[Changed in CR-NNN]` markers present on all modified items
   - `[Added in CR-NNN]` markers present on all new items
   - Removed items properly omitted and listed
   - Result is a complete, self-contained requirements document

   #### Integrated Design Accuracy
   - Integrated design correctly merges original + changes
   - Change markers present on all modified/new sections
   - No orphan references to removed components
   - Result is a complete, self-contained design document

   #### Design Quality
   - Type safety maintained (no `any` in TypeScript, etc.)
   - Consistent with existing architectural patterns
   - Interface contracts are clear and complete
   - Steering alignment verified

4. **Provide Decision**:
   - Maximum 3 critical issues
   - Clear GO/NO-GO decision with rationale
   - Use language specified in spec.json

## Important Constraints
- **Integration accuracy is paramount**: These documents will replace originals during archive
- **Completeness check**: Both integrated documents must be fully self-contained
- **Marker verification**: Every change must be traceable via CR markers

### Language Reminder
- Markdown prompt content must remain in English, even when spec.json requests another language for design output. The validation output should use the spec language.
</instructions>

## Tool Guidance
- **Read first**: Load all context before validation
- **Grep**: Verify marker presence and cross-references
- **Compare**: Check integrated versions against originals + changes

## Output Description
Provide output in the language specified in spec.json with:

1. **Validation Summary**: Brief overview (2-3 sentences)
2. **Critical Issues**: Maximum 3, with severity and suggested fix
3. **Integration Check**: Confirm requirements and design integration accuracy
4. **Final Assessment**: GO/NO-GO decision with rationale and next steps

**Format**: Concise Markdown

## Safety & Fallback

### Error Scenarios
- **Missing Change Design**: If change-design.md doesn't exist, stop: "Run `/kiro-spec-change-design $1` first"
- **No Active Change**: If `change.active` is not `true`, stop: "No active change for this feature"
- **Language Undefined**: Default to English (`en`)

### Next Phase
**If GO**: Proceed to `/kiro-spec-change-tasks $1`
**If NO-GO**: Address issues and re-run `/kiro-spec-change-design $1`
