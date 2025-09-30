# Planning Phase Orchestrator

This document orchestrates the planning phase and contains shared definitions used across both project and issue planning.

## Table of Contents

1. [Planning Phase Overview](#1-planning-phase-overview)
2. [Planning Phase Transitions](#2-planning-phase-transitions)
3. [Documentation Updates](#3-documentation-updates)
4. [Work Unit Definitions & Guidelines](#4-work-unit-definitions--guidelines)
5. [Test Notation & Examples](#5-test-notation--examples-slow-mode-only)

## 1. Planning Phase Overview

The Planning Phase consists of two sequential workflows:

1. **Project Planning** (PLAN-PROJECT.md) - One-time setup
   - Define project vision and requirements
   - Select tech stack
   - Create all milestones
   - Create Parent Ticket with issue structure

2. **Issue Planning** (PLAN-ISSUE.md) - Per milestone
   - Research implementation approaches
   - Break down all issues into tasks
   - Identify cross-issue dependencies
   - Update Linear with complete task lists

### Which Workflow to Load

Check `project_phase` in state.json:
- `"planning-project"` → Load PLAN-PROJECT.md
- `"planning-issue"` → Load PLAN-ISSUE.md
- `"implementing"` → Transition to IMPLEMENT.md

## 2. Planning Phase Transitions

### State Management

Update state.json per schema in AGENTS.md Section 5.1.

### Transition Points

1. **Start of Planning** → Set `project_phase: "planning-project"`
2. **After Project Planning** → Set `project_phase: "planning-issue"`
3. **After Issue Planning** → Set `project_phase: "implementing"`
4. **Begin Implementation** → Load IMPLEMENT.md

### Next Steps

- For Project Planning → Load `.agents/PLAN-PROJECT.md`
- For Issue Planning → Load `.agents/PLAN-ISSUE.md`
- For Implementation → Load `.agents/IMPLEMENT.md`

## 3. Documentation Updates

**During Project Planning:**
- Create PRODUCT-OVERVIEW.md
- Create Project and Parent Ticket in Linear
- {{LEGACY ONLY}} Create M0.1 Legacy Analysis

**During Issue Planning:**
- Update Linear issues with task lists
- Update systems/*.md if new tech introduced
- {{LEGACY ONLY}} Add migration strategies to issues

## 4. Work Unit Definitions & Guidelines

### Hierarchy: Linear Project → Parent Ticket → Milestone → Issue → Task

#### Linear Project Definition
- Container in Linear with simple 1-line description
- Holds all tickets for the software project (e.g., "Portfolio Site")

#### Parent Ticket Definition
- First ticket ({{TICKET_PREFIX}}-XXX) inside the Linear Project
- Contains all milestones and complete project structure
- Title: "[Project Name] - Parent Ticket"
- Use {{PARENT-TICKET-TEMPLATE}}

#### Milestone Definition & Guidelines
- Vertical feature slice with demonstrable user value
- Complete, shippable feature; usually 3-8 issues

**Good:** "User Authentication", "Direct Messages", "Content Management"
**Bad:** "Database Setup" (not user-facing), "UI Components" (incomplete)

#### Issue Definition & Guidelines
- Horizontal technical layer within milestone (may not be user-visible alone)
- Single responsibility, max ~20 tasks, clear dependencies

**Patterns:** Data → API → UI → Integration (full-stack), or layer-specific (frontend/backend)

**Good:** "DM Data Layer", "DM API Layer", "DM UI Layer"
**Bad:** "Frontend" (too broad), "Miscellaneous tasks" (no focus)

#### Task Definition & Guidelines
- Atomic unit = one commit, single functionality with clear "done" criteria
- Includes tests in same commit {{SLOW MODE}}

**Good:** "Create Message model with validation", "Add POST /messages endpoint"
**Bad:** "Add validation" (vague), "Write tests" (part of implementation)

### Complexity Ratings

*See AGENTS.md Section 4.2 for complexity rating definitions*

## 5. Test Notation & Examples {{SLOW MODE ONLY}}

### Test Task Notation
- Regular tasks with unit tests: `[+unit tests]`
- Integration test tasks: `[intg test]`
- E2E testing issue: `[E2E Testing]` in issue name

### Task Examples by Mode

**FAST:** `"Create User model with SQLAlchemy schema"`

**SLOW:** `"Create User model with SQLAlchemy schema [+unit tests]"` + final integration task with `[intg test]`

**E2E Testing Issue (SLOW final issue per milestone):**
`M2.5: DMs [E2E Testing]` - Test flows, persistence, edge cases, coverage review