# Project Planning Workflow

Executed once per project to establish vision, tech stack, and project structure.

## Table of Contents

1. [Project Planning Steps](#1-project-planning-steps)
2. [Speed & Type Selection](#2-speed--type-selection)
3. [Requirements & Vision](#3-requirements--vision)
4. [Tech Stack Selection](#4-tech-stack-selection)
5. [Project Structure](#5-project-structure)
6. [Legacy Project Considerations](#6-legacy-project-considerations)
7. [Documentation Updates](#7-documentation-updates)
8. [Transition to Issue Planning](#8-transition-to-issue-planning)

## 1. Project Planning Steps

**Executed once per project:**

1. **Speed & Type Selection** - Set project_speed (fast/slow) and project_type (new/legacy)
2. **Initialize State** - Create `.agents/state.json` with initial project state
3. **Define Vision** - Establish project goals and overall vision
4. **Identify Users** - Define target users and their needs
5. **List Features** - Enumerate core features and requirements
6. **Set Constraints** - Define scope boundaries and non-goals
7. **Clear Scope?**
   a. YES → Continue to step 8
   b. NO → Use {{DECISION-CHECKPOINT-TEMPLATE}} to clarify requirements
8. **Create Documentation** - Initialize PRODUCT-OVERVIEW.md using template with vision, users, features, constraints
9. **Verify Access** - Test Linear connection, confirm GitHub access
10. **Choose Stack** - Select technology stack based on project requirements
11. **Research Versions and Documentation** - Research current stable versions and documentation URLs
12. **Select Services** - Choose any third-party services needed
   - **Check Documentation:** Update PRODUCT-OVERVIEW.md with tech stack, versions, services, architecture rationale
13. **Update Coding Rules?**
   a. Ask user: "Would you like to add stack-specific rules to .agents/rules/?"
   b. If YES → User provides rules or guidance, agent updates files
   c. Continue to step 14
14. **Decision Points Check**
   a. Review Decision Framework in AGENTS.md Section 6
   b. Any architecture, security, or scope decisions? → Use {{DECISION-CHECKPOINT-TEMPLATE}}
   c. Otherwise → Continue to step 15
15. **{{LEGACY ONLY}} Component Analysis** - Inventory and assess existing components
   - **Check Documentation:** Create/update systems/*.md for each component with [LEGACY - REUSE/REFACTOR/REPLACE] tags
16. **Define Milestones** - Break project into vertical feature slices
17. **Create Linear Project** - Create Linear Project container with 1-line description
18. **Create Parent Ticket** - Create parent ticket in Linear Project with all milestones
19. **Create Issue Titles** - Create issue titles in Linear for ALL milestones
   - {{SLOW ONLY}} Include E2E testing issue as last issue per milestone
20. **Final Review** - Present complete plan for approval
21. **Approved?**
   a. YES → Continue to step 22
   b. NO → Return to relevant step based on feedback
22. **Update State** - Set `project_phase: "planning-issue"`
23. **Transition to Issue Planning** - Load PLAN-ISSUE.md

## 2. Speed & Type Selection

*See AGENTS.md Section 4.2 for mode definitions*

### Initial State Creation

Create `.agents/state.json`:
```json
{
  "project_phase": "planning-project",
  "project_speed": "fast" | "slow",
  "project_type": "new" | "legacy",
  "current": {
    "milestone": "M1",
    "issue": "",
    "task": ""
  }
}
```

## 3. Requirements & Vision

### Key Questions
- Problem, target users, MVP features, out of scope, architecture needs, constraints

### Documentation
Use {{PRODUCT-OVERVIEW-TEMPLATE}} - Document vision, users, features, constraints, success criteria

**If requirements unclear:** Use {{DECISION-CHECKPOINT-TEMPLATE}}

## 4. Tech Stack Selection

### Research Process

1. **Select Stack** - Based on requirements, team expertise, performance needs, timeline
2. **Research Versions** - Find latest stable (not beta/alpha), official docs, verify compatibility
3. **Select Services** - Payment, auth, storage, email, analytics, error tracking

### Documentation

Update PRODUCT-OVERVIEW.md with: Tech choices + versions, official doc URLs, services, architecture rationale

Update `.agents/rules/` if needed for stack-specific patterns

## 5. Project Structure

### Define Milestones

*See PLAN.md Section 4 for detailed guidelines*

Each milestone = One complete user-facing feature, demoable independently

**Examples:** User Authentication, Direct Messages, Channels

### {{LEGACY ONLY}} M0 Milestone

**Legacy Projects ALWAYS start with M0:**
```
M0: Legacy Systems Analysis
└── M0.1: Complete Component Inventory [Required first issue]
    - Use {{LEGACY-ANALYSIS-TEMPLATE}}
    - Document all existing components
    - Categorize each as REUSE/REFACTOR/REPLACE
    - Create/update systems/*.md for each
```
Note: M0 is for analysis only - no demo deliverable.

### Create Linear Project

Create Linear Project container:
- Simple 1-line description of the software project
- Example: "Personal portfolio site showcasing projects and skills"

### Create Parent Ticket

Create first ticket inside Linear Project using {{PARENT-TICKET-TEMPLATE}}

Include:
- Project description and goals
- All milestone definitions with success criteria
- Issue structure for entire project
- Overall success criteria

### Create Issue Titles

For EACH milestone, create issue titles:

**Standard pattern:**
```
M2: Direct Messages
├── M2.1: DM Data Layer
├── M2.2: DM API Layer
├── M2.3: DM UI Layer
├── M2.4: DM Integration
└── M2.5: DMs [E2E Testing] {{SLOW MODE ONLY}}
```

**Other patterns based on architecture:**
- Frontend-only: Components → State Management → Integration
- Backend-only: Models → Services → Controllers → Routes
- Microservices: Service A → Service B → Gateway → Client

**Important:** Create issue titles for ALL milestones upfront to:
- Catch cross-milestone dependencies early
- Ensure foundational milestones support future requirements
- Get complete system view before implementation

## 6. Legacy Project Considerations

### Component Analysis Process

1. Inventory all components
2. Categorize each: REUSE (minimal changes), REFACTOR (modernize), REPLACE (rebuild)
3. Document in M0.1 using {{LEGACY-ANALYSIS-TEMPLATE}} - Include implementation, justification, dependencies, risks
4. Update systems/*.md - Mark as `[LEGACY - REUSE/REFACTOR/REPLACE]`, document behavior and integration

## 7. Documentation Updates

### When to Update Documentation

**During Project Planning, check documentation after these steps:**

**After Step 8 (Create Documentation):**
- Initialize PRODUCT-OVERVIEW.md with vision, users, features, constraints

**After Step 12 (Select Services):**
- Update PRODUCT-OVERVIEW.md with tech stack, versions, official docs URLs, services, architecture rationale

**After Step 15 (Component Analysis - LEGACY ONLY):**
- Create/update systems/*.md for each existing component
- Mark each as `[LEGACY - REUSE/REFACTOR/REPLACE]`
- Document current behavior and integration points

### What to Document

**PRODUCT-OVERVIEW.md** - Created and updated during Project Planning:
- Vision and target users (Step 8)
- Core features and constraints (Step 8)
- Tech stack with versions and doc URLs (Step 12)
- Third-party services (Step 12)
- Architecture rationale (Step 12)

**System Documentation** `.agents/documentation/systems/*.md`:
- **ONLY created during IMPLEMENT phase** when features are actually built
- Exception: {{LEGACY ONLY}} M0.1 creates systems/*.md to document existing code
  - Existing system purpose and behavior
  - Current tech stack and dependencies
  - Status: [LEGACY - REUSE/REFACTOR/REPLACE]
  - Integration points and APIs

**Stay concise:** PRODUCT-OVERVIEW under 200 lines. Systems docs created later during implementation.

## 8. Transition to Issue Planning

**Before transitioning:**
- PRODUCT-OVERVIEW.md created
- Linear Project created with 1-line description
- Parent Ticket + all issue titles created in Linear
- {{LEGACY ONLY}} M0.1 complete

**Update state.json:** `"project_phase": "planning-issue"`

**Next:** Load PLAN-ISSUE.md for detailed task planning