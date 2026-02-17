---
description: Validate a change request for completeness and consistency
---


# Change Request Validation

<background_information>
- **Mission**: Verify that a change request is complete, consistent with existing requirements, and has comprehensive impact analysis
- **Success Criteria**:
  - Change request follows EARS format for new/modified requirements
  - No contradictions with unmodified existing requirements
  - Impact analysis is comprehensive (affected AND unaffected components)
  - Clear GO/NO-GO decision with maximum 3 critical issues
</background_information>

<instructions>
## Core Task
Validate change request for feature **$1** against existing requirements, design, and codebase.

## Execution Steps

1. **Load Context**:
   - Read `{{KIRO_DIR}}/specs/$1/spec.json` for language and metadata
   - Read `{{KIRO_DIR}}/specs/$1/change-request.md` for change request
   - Read `{{KIRO_DIR}}/specs/$1/requirements.md` for current requirements
   - Read `{{KIRO_DIR}}/specs/$1/design.md` for current design
   - **Load ALL steering context**: Read entire `{{KIRO_DIR}}/steering/` directory

2. **Read Review Guidelines**:
   - Read `{{KIRO_DIR}}/settings/rules/ears-format.md` for EARS validation

3. **Execute Validation**:

   #### EARS Format Compliance
   - All new requirements follow EARS syntax
   - Modified requirements maintain EARS format
   - Numeric IDs are properly assigned

   #### Consistency Check
   - No contradictions between changed and unchanged requirements
   - Modified requirements don't invalidate other existing requirements
   - Removed requirements don't leave orphan dependencies

   #### Impact Analysis Completeness
   - All components that could be affected are listed
   - Unaffected components have valid rationale
   - Use Grep to verify claimed impact/non-impact against actual codebase

   #### Change Scope Validation
   - Change is well-bounded (not too broad, not missing related items)
   - Change rationale is clear and justified

4. **Provide Decision**:
   - Maximum 3 critical issues
   - Clear GO/NO-GO decision with rationale
   - Use language specified in spec.json

## Important Constraints
- **Quality assurance, not perfection seeking**: Accept acceptable risk
- **Critical focus only**: Maximum 3 issues that significantly impact success
- **Actionable feedback**: All suggestions must be implementable
</instructions>

## Tool Guidance
- **Read first**: Load all context before validation
- **Grep**: Verify impact analysis against actual codebase
- **Interactive**: Engage with user on findings

## Output Description
Provide output in the language specified in spec.json with:

1. **Validation Summary**: Brief overview (2-3 sentences)
2. **Critical Issues**: Maximum 3, with severity and suggested fix
3. **Strengths**: 1-2 positive aspects of the change request
4. **Final Assessment**: GO/NO-GO decision with rationale and next steps

**Format**: Concise Markdown

## Safety & Fallback

### Error Scenarios
- **Missing Change Request**: If change-request.md doesn't exist, stop: "Run `/kiro-spec-change $1` first"
- **No Active Change**: If `change.active` is not `true`, stop: "No active change for this feature"
- **Language Undefined**: Default to English (`en`)

### Next Phase
**If GO**: Proceed to `/kiro-spec-change-design $1`
**If NO-GO**: Address issues and re-run `/kiro-spec-change $1 "updated description"`
