# Parent Ticket Template

Use this template when creating a parent ticket in your project management tool (Linear, Jira, GitHub Issues, etc.).

```markdown
# {{PROJECT_NAME}} - Parent Ticket

## Description
{{PROJECT_BACKGROUND_GOALS_AND_SCOPE}}

## Milestones

### M1 - {{FEATURE_NAME}}
**Goal:** {{WHAT_THIS_ENABLES}}
**Demo:** {{WHAT_USERS_CAN_DO_AFTER_THIS}}

**Success Criteria:**
- [ ] {{USER_CAN_DO_X}}
- [ ] {{SYSTEM_HANDLES_Y}}
- [ ] {{FEATURE_Z_WORKS_END_TO_END}}

**Issues:**
- [ ] M1.1 - Data Layer ({{TICKET_PREFIX}}-{{XX}})
- [ ] M1.2 - API Layer ({{TICKET_PREFIX}}-{{XX}})
- [ ] M1.3 - UI Layer ({{TICKET_PREFIX}}-{{XX}})
- [ ] M1.4 - {{FEATURE_NAME}} [E2E Testing] ({{TICKET_PREFIX}}-{{XX}}) {{SLOW MODE ONLY}}

### M2 - {{FEATURE_NAME}}
{{SAME_STRUCTURE_AS_M1}}

## Overall Success Criteria
- [ ] Complete application works
- [ ] All features integrate properly
```