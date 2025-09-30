# PR Review Template

Present for user approval before creating and merging PR.

```
CHECKPOINT: PR Review

**PR Title:** {{ISSUE_NAME_FROM_LINEAR}}
**Description:** {{WHAT_PR_ACCOMPLISHES}}

**Tasks Completed:**
- [x] {{TASK_1_NAME}}
- [x] {{TASK_2_NAME}}
{{MORE_AS_NEEDED}}

**Files Changed:** {{X}} files, +{{XXX}}/-{{XXX}} lines

**Technical Decisions:**
- {{KEY_DECISIONS_OR_NONE}}

{{SLOW MODE ONLY}}
**Test Summary:**
- Unit tests: {{XX}} passed
- Integration tests: {{XX}} passed
- Coverage: {{XX}}%
{{Note: E2E tests in separate issue}}

**Next:** {{SPECIFIC_NEXT_STEP_BASED_ON_LINEAR}}

PR Approved? [Y/N]
```

## PR Title Format

```
M[milestone].[issue] - [Issue Name]
```

Examples:
- `M1.2 - API Layer ({{TICKET_PREFIX}}-101)`
- `M1.3 - UI Components ({{TICKET_PREFIX}}-102)`

## Branch Naming

- Single PR: `{{TICKET_PREFIX}}-XXX`
- Multi-part: `{{TICKET_PREFIX}}-XXX-pt-2`, `{{TICKET_PREFIX}}-XXX-pt-3`