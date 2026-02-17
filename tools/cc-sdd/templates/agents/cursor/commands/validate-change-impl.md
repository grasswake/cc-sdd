<meta>
description: Validate change implementation against change specs
argument-hint: <feature-name:$1> [task-numbers:$2]
</meta>

# Change Implementation Validation

<background_information>
- **Mission**: Verify that change implementation aligns with change request, change design, and change tasks, with special focus on regression prevention
- **Success Criteria**:
  - All specified change tasks marked as completed
  - Tests exist and pass for changed functionality
  - No regressions in existing functionality
  - Change requirements traceability confirmed
  - Clear GO/NO-GO decision
</background_information>

<instructions>
## Core Task
Validate change implementation for feature **$1** based on change specifications.

## Execution Steps

### 1. Detect Validation Target

**If no task arguments provided** (`$2` empty):
- Detect all completed change tasks `[x]` in `{{KIRO_DIR}}/specs/$1/change-tasks.md`

**If task numbers provided** (`$2` present):
- Validate specified change tasks only (e.g., `C1.1,C2`)

### 2. Load Context

- Read `{{KIRO_DIR}}/specs/$1/spec.json` for metadata
- Read `{{KIRO_DIR}}/specs/$1/change-request.md` for change requirements
- Read `{{KIRO_DIR}}/specs/$1/change-design.md` for change design (including integrated specs)
- Read `{{KIRO_DIR}}/specs/$1/change-tasks.md` for change task list
- Read `{{KIRO_DIR}}/specs/$1/requirements.md` for original requirements (baseline)
- Read `{{KIRO_DIR}}/specs/$1/design.md` for original design (baseline)
- **Load ALL steering context**: Read entire `{{KIRO_DIR}}/steering/` directory

### 3. Execute Validation

For each change task, verify:

#### Task Completion Check
- Checkbox is `[x]` in change-tasks.md
- If not completed, flag as "Task not marked complete"

#### Test Coverage Check
- Tests exist for changed functionality
- Tests pass (no failures or errors)
- Use Bash to run test commands
- If tests fail or don't exist, flag as "Test coverage issue"

#### Regression Check (Critical)
- Run full test suite
- Compare against baseline (all original tests must still pass)
- If any regression detected, flag as "CRITICAL: Regression detected"

#### Change Requirements Traceability
- Each change item from change-request.md is traceable to implementation
- Use Grep to search for evidence of change implementation
- If change not traceable, flag as "Change not implemented"

#### Design Alignment
- Implementation follows integrated design from change-design.md
- Changed components match design changes
- New components exist as specified

### 4. Generate Report

Provide summary in the language specified in spec.json:
- Validation summary by change task
- Regression report (critical)
- Change requirements coverage
- GO/NO-GO decision

## Important Constraints
- **Regression is highest priority**: Any regression is automatic NO-GO
- **Change traceability required**: All change items must be traceable
- **Test-first focus**: Test coverage is mandatory for GO decision

### Language Reminder
- Markdown prompt content must remain in English, even when spec.json requests another language for design output. The generated validation report should use the spec language.
</instructions>

## Tool Guidance
- **Read context**: Load all change specs and steering before validation
- **Bash for tests**: Execute test commands to verify pass status and regressions
- **Grep for traceability**: Search codebase for change implementation evidence
- **LS/Glob for structure**: Verify file structure matches design

## Output Description
Provide output in the language specified in spec.json with:

1. **Detected Target**: Change tasks being validated
2. **Regression Report**: Full test suite results (PASS/FAIL with details)
3. **Change Coverage**: Change requirements implemented/missing
4. **Issues**: Validation failures with severity
5. **Decision**: GO (ready for archive) / NO-GO (needs fixes)

**Format**: Markdown with tables, severity indicators

## Safety & Fallback

### Error Scenarios
- **No Active Change**: If `change.active` is not `true`, stop: "No active change for this feature"
- **Missing Change Files**: If change spec files missing, stop with error
- **Test Command Unknown**: Warn and skip test validation (manual verification required)
- **Language Undefined**: Default to English (`en`)

### Next Steps

**If GO**:
- Run `/kiro/spec-archive-change $1` to archive change and update original specs

**If NO-GO**:
- Address critical issues (especially regressions)
- Re-run `/kiro/spec-change-impl $1 [tasks]` for fixes
- Re-validate with `/kiro/validate-change-impl $1`
