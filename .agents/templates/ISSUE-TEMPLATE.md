# Issue Template

Use this template during Issue Planning to fully document each issue in your project management tool.

Always follow the canonical title format from AGENTS.md (`M[milestone].[issue] - [Issue Name] ({{TICKET_PREFIX}}-XXX)`) so issues, PRs, and project skeletons stay in sync.

```markdown
M{{X}}.{{Y}} - {{ISSUE_NAME}} ({{TICKET_PREFIX}}-{{XX}})

## Description
{{WHAT_THIS_ISSUE_ACCOMPLISHES_IN_DETAIL}}

## Complexity
{{🟢/🟡/🔴/⚫}} {{LOW/MEDIUM/HIGH/UNKNOWN}} ({{L/M/H/U}})

{{See AGENTS.md Section 4.2 for complexity rating definitions}}

## Success Criteria
- [ ] {{MEASURABLE_OUTCOME_1}}
- [ ] {{MEASURABLE_OUTCOME_2}}
- [ ] {{MEASURABLE_OUTCOME_3}}

## Dependencies
- Requires: M{{X}}.{{Y-1}} - {{PREVIOUS_ISSUE_NAME}} ({{TICKET_PREFIX}}-{{XX}})
- Blocks: M{{X}}.{{Y+1}} - {{NEXT_ISSUE_NAME}} ({{TICKET_PREFIX}}-{{XX}})

## Technical Approach
{{DETAILED_IMPLEMENTATION_STRATEGY}}

## Tasks

{{FAST mode - Regular tasks}}:
- [ ] **{{TASK_1_NAME}}** - {{IMPLEMENTATION_APPROACH}}
- [ ] **{{TASK_2_NAME}}** - {{IMPLEMENTATION_APPROACH}}
- [ ] **{{TASK_3_NAME}}** - {{IMPLEMENTATION_APPROACH}}

{{SLOW mode - Tasks with test notation}}:
- [ ] **{{TASK_1_NAME}} [+unit tests]** - {{IMPLEMENTATION_APPROACH}}
- [ ] **{{TASK_2_NAME}} [+unit tests]** - {{IMPLEMENTATION_APPROACH}}
- [ ] **{{TASK_3_NAME}} [+unit tests]** - {{IMPLEMENTATION_APPROACH}}
- [ ] **{{INTEGRATION_TEST_NAME}} [intg test]** - {{TEST_APPROACH}}

{{MORE_TASKS_AS_NEEDED}}

{{For E2E Testing issues, tasks focus on user flows rather than implementation:}}
{{- Test complete user journey (signup → login → feature use)}}
{{- Test data persistence across sessions}}
{{- Test error scenarios and recovery}}
{{- **REQUIRED in E2E Issues: Test coverage review for entire milestone**}}

## Technical Notes
- {{KEY_PATTERNS_TO_FOLLOW}}
- {{INTEGRATION_POINTS_WITH_OTHER_ISSUES}}
- {{SHARED_COMPONENTS_WITH_OTHER_ISSUES}}
- {{EDGE_CASES_TO_HANDLE}}

---
## {{LEGACY ONLY}} Legacy Systems Analysis
*(Expanded from initial assessment)*

**Component:** {{COMPONENT_NAME}}
**Decision:** {{REUSE/REFACTOR/REPLACE}}

**Current Implementation:**
- Technology: {{CURRENT_TECH_STACK_VERSION}}
- Location: {{WHERE_IN_CODEBASE}}
- Core Functionality: {{WHAT_IT_DOES}}

**Migration Strategy:**
{{SPECIFIC_STEP_BY_STEP_APPROACH}}

**Risk Mitigation:**
- Rollback Plan: {{HOW_TO_REVERT_IF_ISSUES}}
- Testing Strategy: {{HOW_TO_VERIFY_NO_BREAKAGE}}
```