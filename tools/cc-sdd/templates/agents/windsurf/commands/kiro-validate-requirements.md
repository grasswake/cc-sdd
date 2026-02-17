---
description: Validate requirements for completeness and quality
auto_execution_mode: 3
---
<meta>
description: Validate requirements for completeness and quality
argument-hint: <feature-name:$1>
</meta>

# Requirements Validation

<background_information>
- **Mission**: Verify that requirements are complete, testable, consistent, and follow EARS format
- **Success Criteria**:
  - All requirements follow EARS syntax with proper patterns
  - Requirements are testable and verifiable
  - No contradictions or duplicates between requirements
  - Steering alignment confirmed
  - Numeric IDs properly assigned
  - Clear GO/NO-GO decision with maximum 3 critical issues
</background_information>

<instructions>
## Core Task
Validate requirements for feature **$1** against EARS rules, steering context, and quality criteria.

## Execution Steps

1. **Load Context**:
   - Read `{{KIRO_DIR}}/specs/$1/spec.json` for language and metadata
   - Read `{{KIRO_DIR}}/specs/$1/requirements.md` for requirements
   - **Load ALL steering context**: Read entire `{{KIRO_DIR}}/steering/` directory including:
     - Default files: `structure.md`, `tech.md`, `product.md`
     - All custom steering files

2. **Read Validation Guidelines**:
   - Read `{{KIRO_DIR}}/settings/rules/ears-format.md` for EARS syntax validation rules

3. **Execute Validation**:

   #### EARS Format Compliance
   - Every acceptance criterion uses proper EARS pattern (Ubiquitous, Event-Driven, State-Driven, Unwanted Behavior, Optional Feature, Complex)
   - Correct subject selection (system/service name, not generic)
   - No implementation details in requirements (WHAT, not HOW)

   #### Testability Check
   - Each requirement can be verified through testing
   - Acceptance criteria are specific and measurable
   - No vague terms ("fast", "easy", "user-friendly") without quantification

   #### Completeness & Coverage
   - All aspects of the project description are covered
   - Edge cases and error scenarios addressed
   - No obvious functional gaps

   #### Consistency & Uniqueness
   - No contradictions between requirements
   - No duplicate requirements covering the same functionality
   - Requirements don't conflict with steering context

   #### Steering Alignment
   - Requirements respect existing patterns from steering
   - No conflicts with product, tech, or structure steering

   #### Numeric ID Validation
   - All requirements have leading numeric IDs (not alphabetic)
   - IDs are sequential and unique
   - No gaps in numbering

4. **Provide Decision**:
   - Maximum 3 critical issues
   - Clear GO/NO-GO decision with rationale
   - Use language specified in spec.json

## Important Constraints
- **Quality assurance, not perfection seeking**: Accept acceptable risk
- **Critical focus only**: Maximum 3 issues that significantly impact success
- **Actionable feedback**: All suggestions must be implementable
- **No rewrites**: Validate and report, do not modify requirements.md

### Language Reminder
- Markdown prompt content must remain in English, even when spec.json requests another language for validation output. The validation output should use the spec language.
</instructions>

## Tool Guidance
- **Read first**: Load all context before validation
- **Grep**: Search for pattern compliance, verify steering alignment
- **Interactive**: Engage with user on findings

## Output Description
Provide output in the language specified in spec.json with:

1. **Validation Summary**: Brief overview (2-3 sentences) of requirements quality
2. **Critical Issues**: Maximum 3, with severity and suggested fix
3. **Strengths**: 1-2 positive aspects of the requirements
4. **Coverage Assessment**: Estimated completeness of functional coverage
5. **Final Assessment**: GO/NO-GO decision with rationale and next steps

**Format**: Concise Markdown

## Safety & Fallback

### Error Scenarios
- **Missing Requirements**: If requirements.md doesn't exist, stop: "Run `/kiro-spec-requirements $1` first"
- **No Project Description**: If requirements.md lacks content, stop: "Requirements not yet generated"
- **Empty Steering Directory**: Warn that project context is missing and may affect validation quality
- **Language Undefined**: Default to English (`en`)

### Next Phase

**If GO**:
- Requirements validated and ready for design
- **Optional**: Run `/kiro-validate-gap $1` for implementation gap analysis (brownfield projects)
- Then: Run `/kiro-spec-design $1 -y` to proceed to design phase

**If NO-GO**:
- Address critical issues identified
- Re-run `/kiro-spec-requirements $1` with improvements
- Re-validate with `/kiro-validate-requirements $1`

**Note**: Requirements validation is optional but recommended before proceeding to design.
