# Commit Review Template

Present for user approval before executing git commit.

```
CHECKPOINT: Commit Review

**Commit Message:** M{{X}}.{{Y}}.{{Z}} - {{TASK_NAME}}

**Issue:** {{CURRENT_ISSUE_NAME_WITH_TICKET_ID}}

**Description:** {{WHAT_WAS_IMPLEMENTED}}

**Files Changed:**
- `{{FILENAME}}` - {{MODIFIED_OR_NEW}} - {{WHAT_CHANGED}}

**Technical Decisions:**
- {{DECISIONS_MADE_OR_NONE}}

{{SLOW MODE ONLY}}
**Test Results:**
- All tests passed ✅
- Coverage: {{X}}% (+{{Y}}%)
- Test type: {{UNIT_INTEGRATION_OR_E2E}}

**Next:** {{SPECIFIC_NEXT_STEP_BASED_ON_LINEAR_TASKS}}

Commit Approved? [Y/N]
```

## Commit Message Format

```
M[milestone].[issue].[task] - [Task description]
```

Examples:
- `M1.2.3 - Install core UI components`
- `M1.2.4 - Update legacy components to use new library`
- `M1.2.BUGFIX - Fix CSS build configuration`