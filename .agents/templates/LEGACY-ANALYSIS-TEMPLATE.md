# Legacy Systems Analysis Template

Used for M0.1 issue in legacy projects to document complete component inventory.

## M0.1 - Legacy Systems Analysis (Aggregated)

### Component Inventory

**Component 1: {{NAME}}**
- **Current Implementation:** {{HOW_IT_CURRENTLY_WORKS}}
- **Tech Stack:** {{LANGUAGES_FRAMEWORKS_LIBRARIES}}
- **Decision:** {{REUSE/REFACTOR/REPLACE}}
- **Justification:** {{WHY_THIS_DECISION}}
- **Dependencies:** {{WHAT_DEPENDS_ON_THIS}}
- **Risk Level:** {{LOW/MEDIUM/HIGH}}

**Component 2: {{NAME}}**
- **Current Implementation:** {{HOW_IT_CURRENTLY_WORKS}}
- **Tech Stack:** {{LANGUAGES_FRAMEWORKS_LIBRARIES}}
- **Decision:** {{REUSE/REFACTOR/REPLACE}}
- **Justification:** {{WHY_THIS_DECISION}}
- **Dependencies:** {{WHAT_DEPENDS_ON_THIS}}
- **Risk Level:** {{LOW/MEDIUM/HIGH}}

[Repeat for all components]

### Migration Order

1. {{COMPONENT_NAME}} - {{REASON_FOR_THIS_ORDER}}
2. {{COMPONENT_NAME}} - {{REASON_FOR_THIS_ORDER}}
3. {{COMPONENT_NAME}} - {{REASON_FOR_THIS_ORDER}}

### Risk Matrix

| Component | Risk Level | Mitigation Strategy |
|-----------|------------|-------------------|
| {{NAME}} | {{LEVEL}} | {{STRATEGY}} |
| {{NAME}} | {{LEVEL}} | {{STRATEGY}} |

### Backwards Compatibility Requirements

- {{REQUIREMENT_1}}
- {{REQUIREMENT_2}}
- {{REQUIREMENT_3}}

### Success Criteria

- [ ] All REPLACE components successfully migrated
- [ ] All REFACTOR components updated
- [ ] All REUSE components integrated
- [ ] No regressions in existing functionality
- [ ] Performance maintained or improved