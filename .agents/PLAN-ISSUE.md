# Issue Planning Workflow

Executed for ALL milestones to break down issues into detailed task lists before implementation begins.

## Table of Contents

1. [Issue Planning Steps](#1-issue-planning-steps)
2. [Understanding Milestone Requirements](#2-understanding-milestone-requirements)
3. [Research Implementation Approaches](#3-research-implementation-approaches)
4. [Technical Decision Making](#4-technical-decision-making)
5. [Breaking Down Issues](#5-breaking-down-issues)
6. [Legacy Component Integration](#6-legacy-component-integration)
7. [Documentation Updates](#7-documentation-updates)
8. [Transition to Implementation](#8-transition-to-implementation)

## 1. Issue Planning Steps

**Executed for ALL milestones before transitioning to IMPLEMENT.md:**

1. **Select Milestone** - Choose first/next milestone to plan
2. **Confirm Milestone Context** - Review milestone goals and issue titles from Linear
3. **Research Codebase** - Deep dive into existing patterns and implementations
4. **External Research (if needed)** - If codebase research is insufficient, research external patterns
   - Use web search for implementation approaches, best practices, or library documentation
   - Skip if existing patterns are clear and sufficient
5. **Identify Shared Components** - Find reusable code across milestone issues
6. **Map Dependencies** - Understand issue interdependencies (Data → API → UI)
7. **Technical Decisions** - Make architecture choices for the milestone
8. **Decision Points Check**
   a. Review Decision Framework in AGENTS.md Section 6
   b. Any architecture, security, or scope decisions? → Use {{DECISION-CHECKPOINT-TEMPLATE}}
   c. Otherwise → Continue to step 9
9. **Break Down Issues** - Create detailed task lists for EACH issue in milestone
10. **Optimize Task Sequence** - Order tasks across issues for efficiency
11. **Update Linear** - Add complete task lists to all milestone issues
   - **Check Documentation:** Did you introduce new tech or patterns? Update PRODUCT-OVERVIEW.md if needed
12. **Update State** - Save progress
13. **More Milestones?**
    a. YES → Return to step 1 (Select Milestone)
    b. NO → Continue to step 14
14. **Update State** - Set `project_phase: "implementing"`
15. **Transition to IMPLEMENT.md** - Planning complete, begin implementation

## 2. Understanding Milestone Requirements

**Context:** Read milestone description, review all issue titles, understand success criteria

**Codebase Research:**
- Review PRODUCT-OVERVIEW.md for patterns, architecture, tech choices
- Search code for reusable components, established patterns, integration points
- {{LEGACY ONLY}} Review M0.1 analysis, migration strategies, compatibility needs

## 3. Research Implementation Approaches

**For each issue:** Identify patterns, reusable components, data structures, API contracts, integration points

**Cross-Issue:** Find shared components, common patterns, task dependencies, optimal sequencing

**New Tech:** Check PRODUCT-OVERVIEW.md for versions → If missing, research and add immediately

## 4. Technical Decision Making

*See AGENTS.md Section 6 for complete Decision Framework*

**Present CHECKPOINT when:**
- Multiple valid approaches, affects architecture/UX, sets future patterns, performance tradeoffs

**ALWAYS present:** Architecture, libraries, API/DB design, state management, security, UX patterns
**NEVER present:** Styling, naming, file organization, formatting

**Use {{DECISION-CHECKPOINT-TEMPLATE}} when needed**

## 5. Breaking Down Issues

### Task Creation

*See PLAN.md Section 4-5 for task guidelines and test notation*

**For EACH issue:** Review purpose, define tasks with approaches, consider edge cases, {{SLOW ONLY}} mark tests

**Use {{ISSUE-TEMPLATE}}** - Include description, complexity, success criteria, dependencies, task list, {{LEGACY ONLY}} migration details

**{{SLOW ONLY}} Add E2E testing issue as FINAL issue in milestone** - Focus on user journeys, not implementation

## 6. Legacy Component Integration

### {{LEGACY ONLY}} Per-Issue Analysis

1. Copy relevant M0.1 section to issue description
2. Include migration approach: REUSE (integration/adapters), REFACTOR (changes needed), REPLACE (new implementation)

## 7. Documentation Updates

### When to Update Documentation

**During Issue Planning, check documentation after these steps:**

**After Step 11 (Update Linear with task lists):**
- Did you identify new tech or libraries needed that wasn't in original tech stack?
- Update PRODUCT-OVERVIEW.md with new tech stack choices and versions
- Did architecture or patterns change significantly?
- Update PRODUCT-OVERVIEW.md architecture section

### What to Document

**PRODUCT-OVERVIEW.md** - Update if Issue Planning introduces:
- New frameworks, libraries, or tools not in original tech stack
- New third-party services or integrations
- Significant architecture changes or patterns

**System Documentation** `.agents/documentation/systems/*.md`:
- **DO NOT CREATE during planning** - Systems docs reflect implemented code only
- Created/updated during IMPLEMENT phase as features are actually built
- Exception: {{LEGACY ONLY}} M0.1 creates systems/*.md to document existing code

### Documentation Guidelines

**PRODUCT-OVERVIEW.md during Planning:**
- High-level vision, tech stack, architecture decisions
- No implementation details yet

**systems/*.md during Implementation (not planning):**
- Created when features are implemented
- Reflects actual code, not planned code
- Updated progressively after commits/PRs/milestones

## 8. Transition to Implementation

**Update ALL Linear issues:** Add task lists, complexity, dependencies, approach notes, {{LEGACY ONLY}} migration details

**Update state.json:** Set `"project_phase": "implementing"` with first task details

**Check documentation one final time:** PRODUCT-OVERVIEW.md reflects all planned tech and architecture decisions

**Next:** Load IMPLEMENT.md to begin execution