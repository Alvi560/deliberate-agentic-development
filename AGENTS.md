# Agent Workflow System

**FREEMODE (Context/Workflow Control):**

FREEMODE allows you to temporarily disable all workflow rules and context loading when you need a lightweight conversation without the full agent system.

**Usage:**
- `FREEMODE START` - Early return from AGENTS.md. Ignore all workflow rules and context loading. Treat the chat like a blank slate.
- `FREEMODE END` - Load AGENTS.md from the beginning and follow all standard workflow rules as normal.

**When to use FREEMODE:**
- Quick questions that don't require project context
- Debugging or exploration outside the workflow
- When context loading becomes too heavy (30k+ tokens)
- Experimentation without affecting state.json

**Note:** FREEMODE does not modify state.json. Your project position is preserved.

## Table of Contents

1. [Context Loading Instructions](#1-context-loading-instructions)
2. [Required Tools & Configuration](#2-required-tools--configuration)
3. [Workflows](#3-workflows)
4. [Work Unit Hierarchy](#4-work-unit-hierarchy)
5. [State Management](#5-state-management)
6. [Communication Patterns](#6-communication-patterns)
7. [Documentation Structure](#7-documentation-structure)
8. [Template Notation System](#8-template-notation-system)

---

## 1. Context Loading Instructions

**File Location Note:** All workflow and documentation files referenced below are located in the `.agents/` directory unless otherwise specified.

**MANDATORY FIRST STEPS - EXECUTE IN ORDER (skip if FREEMODE active):**

1. **Check Project State**
   - Read state.json to check workflow phase
   - If no state.json exists:
     a. First time setup → Ask: "Tell me about your project"
     b. Review Required Tools (Section 2) and verify connections
     c. Missing tools? → Configure required MCPs and CLI tools
   - Otherwise continue with workflow phase from state.json

2. **Load Core Documentation**
   - Always load PRODUCT-OVERVIEW.md for product vision and tech stack

3. **Load Phase-Specific Workflow**

   Based on `project_phase` from state.json:
   - `"planning-project"` → Load PLAN.md then PLAN-PROJECT.md
   - `"planning-issue"` → Load PLAN.md then PLAN-ISSUE.md
   - `"implementing"` → Load IMPLEMENT.md

4. **Load Rules & System Documentation**

   **Decision Tree:**
   ```
   Check project_phase from state.json
   │
   ├─ "planning-project"
   │  └─ Load ALL files in rules/
   │     (Architecture decisions need full context)
   │
   └─ "planning-issue" OR "implementing"
      └─ Smart loading based on current work:
         a. List filenames in rules/ and documentation/systems/
         b. Infer relevance from filename (don't search content)
         c. Load only what's relevant to current work

   Example: Task "Create user API"
   → Load: API-RULES.md, AUTHENTICATION.md
   → Skip: UI-RULES.md, TESTING.md (unless relevant)
   ```

   **Important:** Don't assume specific file names - work with what's there

5. **Load Agent-Specific Config** (if exists)
   - Claude → .claude/
   - Cursor → .cursor/
   - Codex → .codex/

---

## 2. Required Tools & Configuration

*Note: This section shows the default setup. See .agents/README.md for configuration options.*

### Core Configuration
```
PM_TOOL=Linear                # Project management tool
GIT_PLATFORM=GitHub           # Git platform
TICKET_PREFIX=MAT             # Ticket prefix (change to your prefix)
PROJECT_NAME=portfolio-site   # Project name (change to your project)
PROJECT_ROOT=/path/to/project # Project root (change to your path)
```

### Required Tools & MCPs

#### Essential Command Line Tools
- **Git** - Version control
- **GitHub CLI** (`gh`) - Pull request and repository management
- **Node.js (>=18) + npm/pnpm** - Frontend build, tests, lint/format
- **Python (>=3.11) + pip/uv** - Backend app, tests, lint/format/typecheck
- **Playwright CLI** - End-to-end browser testing
- **ESLint + Prettier** - Frontend linting/formatting
- **Ruff + Black + Mypy + Pytest** - Backend linting/formatting/type/tests

#### Required MCPs
- **Linear MCP** - Issue tracking and project management

### Optional MCPs
- **Notion MCP** - Documentation and tech stack preferences
- **Playwright MCP** - Browser automation for E2E testing
- **Chrome DevTools MCP** - Browser debugging and inspection

Note: If your project’s toolchain changes, update this section and `.agents/README.md` to keep the workflow accurate.

### Naming Conventions

#### Branches
- **Single PR:** `{{TICKET_PREFIX}}-XXX` (e.g., `{{TICKET_PREFIX}}-123`)
- **Multi-part:** `{{TICKET_PREFIX}}-XXX-pt-2`, `{{TICKET_PREFIX}}-XXX-pt-3`

#### Commits
```
Format: M[milestone].[issue].[task] - Task description
Example: M1.2.3 - Add user authentication
```

#### Pull Requests
```
Format: M[milestone].[issue] - [Issue Name] ({{TICKET_PREFIX}}-XXX)
Example: M1.2 - API Layer ({{TICKET_PREFIX}}-101)
Note: {{TICKET_PREFIX}}-XXX included for Linear linkage
```

---

## 3. Workflows

### 3.1 🚨 MANDATORY WORKFLOW COMPLIANCE 🚨

- Follow sequential workflow steps in exact order - NO skipping, combining, or reordering
- Use templates exactly as provided
- One task = One commit = One review (never bundle)
- Get user approval at all CHECKPOINT steps

**If Confused:** Check state.json, then reload appropriate workflow document

### 3.2 Two Workflow Phases

**PLANNING Phase (PLAN.md + sub-workflows):**
- Project Planning (PLAN-PROJECT.md): One-time setup, vision, tech stack, all milestones
- Issue Planning (PLAN-ISSUE.md): Per-milestone task breakdown (all milestones before implementation)

**IMPLEMENTING Phase (IMPLEMENT.md):**
- Execute tasks one-by-one with reviews
- Create PRs and merge when issues complete

## 4. Work Unit Hierarchy

### 4.1 Work Unit Definitions

*For detailed guidelines and examples, see PLAN.md*

**Project** = The entire deliverable (e.g., "Slack Clone")
- Lives in Linear as a Project
- Contains one Parent Ticket as direct child
- All work organized under Parent Ticket

**Milestone** = Vertical feature slice with demonstrable user benefit
- Complete user-facing feature
- Has clear demo value
- Examples: "Direct Messages", "Channels", "Threads"

**Issue** = Horizontal technical layer within a feature
- Complete technical component within a milestone
- May not be user-visible alone
- Examples: "DM Data Layer", "DM API Layer", "DM UI Layer"
- Max ~20 tasks per issue (split if larger)

**Task** = Atomic implementation unit
- Single piece of functionality
- Can be committed independently
- Examples: "Create Message modal", "Add send endpoint", "Build message input component"

### 4.2 Complexity Ratings

Used during Issue Planning to estimate issue difficulty:

```
🟢 Low (L) - Clear path, straightforward implementation
🟡 Medium (M) - Some complexity, needs design consideration
🔴 High (H) - Complex, affects multiple systems
⚫ Unknown (U) - Needs investigation to determine complexity
```

**Note:** Complexity ratings are for planning only, not time estimates.

### 4.3 Linear Structure

**Hierarchy:**
```
Project (in Linear)
└── Parent Ticket ({{TICKET_PREFIX}}-XXX)
    ├── Issue (M1.1) → {{TICKET_PREFIX}}-101
    ├── Issue (M1.2) → {{TICKET_PREFIX}}-102
    └── Issue (M2.1) → {{TICKET_PREFIX}}-103
```

**Identifiers:**
- **Milestone:** M1, M2, M3 (listed in Parent Ticket)
- **Issue:** M1.2 (canonical) = {{TICKET_PREFIX}}-XXX (Linear ID)
- **Task:** M1.2.3 (checklist item in Issue description)

**Naming:**
- **Parent Ticket:** "[Project Name] - Parent Ticket"
- **Issue:** "M1.2 - [Layer Name] ({{TICKET_PREFIX}}-XXX)"
- **Task:** "M1.2.3 - [Simple action]" (e.g., "M1.2.3 - Create Message model")

*See PLAN.md Section 4 for detailed guidelines and examples*

## 5. State Management

### 5.1 state.json Structure

**Minimal State Schema (.agents/state.json):**
```json
{
  "project_speed": "fast" | "slow",
  "project_type": "new" | "legacy",
  "project_phase": "planning-project" | "planning-issue" | "implementing",
  "current": {
    "milestone": "M1",         // Milestone number only
    "issue": "M1.2",           // Canonical format: M[milestone].[issue]
    "task": "M1.2.3",          // Full canonical format: M[milestone].[issue].[task]
    "branch": "{{TICKET_PREFIX}}-XXX"  // Linear ID for branch/linkage
  }
}
```

**State Management Rules:**
- State structure is defined HERE in AGENTS.md only
- To resume planning after interruption, check Linear for which issues have complete task lists
- Update triggers are defined in respective workflow documents
- Always check state.json first to understand current position
- Phase transitions handled automatically by workflows

### 5.2 Project Speed and Project Type

**SPEED:**
- `fast` - No tests, quick reviews, ship demos quickly
- `slow` - Full TDD, comprehensive reviews, production quality

**TYPE:**
- `new` - Greenfield development
- `legacy` - Existing codebase with migration strategies

### 5.3 Mode Differences Quick Reference

| Aspect | FAST Mode | SLOW Mode |
|--------|-----------|-----------|
| **Testing** | Manual only | TDD: unit, integration, E2E |
| **Task Notation** | `Create User model` | `Create User model [+unit tests]` |
| **E2E Testing** | None | Dedicated E2E issue per milestone |
| **Review Templates** | No test results section | Include test results & coverage |
| **Pre-Commit** | Lint, format, manual test | Lint, format, unit/integration tests, manual test |
| **Pre-Push** | Skipped | Required: E2E suite + production build |
| **Goal** | Ship demos quickly | Production-ready code |

**Both Modes Require:**
- All CHECKPOINT approvals (Commit, PR, Milestone Reviews)
- Lint and format validation
- Manual testing of implemented features
- Documentation updates
- Linear tracking and comments

## 6. Communication Patterns

### 6.1 Decision Framework

**ALWAYS Surface to User (CHECKPOINT Required):**
- **Architecture changes** - Different framework, database, or service than planned
- **Security implications** - Authentication approach, data exposure, API permissions
- **Breaking changes** - API contract changes, database schema migrations
- **External dependencies** - New packages/services not in original plan
- **Scope changes** - Features beyond or different from requirements
- **Cost implications** - Paid services, resource-intensive solutions
- **User experience changes** - Different flow/interaction than specified

**Agent Can Decide (No CHECKPOINT):**
- Implementation details (variable names, file organization)
- Code style following existing patterns
- Refactoring without behavior changes
- Clear bug fixes with single solution
- Tool commands (build, test, lint)
- Standard error handling

**Gray Area Rule:**
If decision affects future work, other systems, testing strategy, or performance → Surface as CHECKPOINT

**Before surfacing CHECKPOINTS, check:**
1. PRODUCT-OVERVIEW.md
2. Existing codebase patterns
3. Parent Ticket or previous discussions

**Examples:**
- REST vs GraphQL → CHECKPOINT (architecture)
- map() vs forEach() → No checkpoint (implementation)
- Caching layer not in plan → CHECKPOINT (scope)
- Input validation → No checkpoint (standard)

### 6.2 Response & Communication

**Format:**
- Lead with answer/action, provide context only when necessary
- Use bullets for lists, cite files (e.g., `src/api.js:42`)
- Add newlines before headings for terminal readability

**If confused:** Check state.json, then ask user

## 7. Documentation Structure

### File Categories

**Workflow Files (Static):**
- AGENTS.md, PLAN*.md, IMPLEMENT.md - Never modified by agent

**Rules (User-Maintained):**
- `.agents/rules/*.md` - Project patterns and standards
- **Recommended:** PRE-COMMIT-RULES.md (commit-time checks) and PRE-PUSH-RULES.md (pre-push checks)
- **TOC Requirement:** Always include Table of Contents for easy navigation
- **Agent Behavior:** Never automatically create or update rules without user approval
  - If user says "never do X" or "always do Y", ask: "Should I create/update a rule for this pattern?"
  - After milestones, ask: "Did any patterns emerge that should be documented as rules?"
  - User must explicitly approve before any rule file is created or modified

**Documentation (Agent-Maintained):**
- `.agents/documentation/PRODUCT-OVERVIEW.md` - Vision and tech stack (always loaded)
- `.agents/documentation/systems/*.md` - How implemented systems work
  - **TOC Requirement:** Always include Table of Contents for easy navigation

### Templates

**Available in `.agents/templates/`:**
- Planning: PARENT-TICKET-TEMPLATE, ISSUE-TEMPLATE, LEGACY-ANALYSIS-TEMPLATE
- Implementation: COMMIT-REVIEW-TEMPLATE, PR-REVIEW-TEMPLATE, MILESTONE-REVIEW-TEMPLATE
- Decisions: DECISION-CHECKPOINT-TEMPLATE, PROBLEM-INVESTIGATION-TEMPLATE
- Documentation: PRODUCT-OVERVIEW-TEMPLATE, SYSTEM-DOCUMENTATION-TEMPLATE

**Usage:** Templates are loaded on-demand at checkpoint moments (see Section 8 for notation)

**TOC Requirements:**
- Templates >100 lines should include Table of Contents
- Templates <100 lines generally don't need TOCs (serve as fill-in forms)
- Exception: Add TOC if template has complex nested structure regardless of length

---

## 8. Template Notation System

**All templates and documentation use consistent placeholder notation:**

- **`{{DYNAMIC_FIELD}}`** or **`{field}`** = Calculate/fill based on context
- **Literal text** = Copy exactly as shown
- **NEVER** output `{{FIELD}}` or `{field}` literally - always replace with actual values

**Common placeholders:**
- `{{PROJECT_NAME}}` - The project name (e.g., "portfolio-site")
- `{{TICKET_PREFIX}}` - Your PM tool ticket prefix (e.g., "MAT", "PROJ")
- `{{TICKET_PREFIX}}-XXX` - Specific ticket ID from Linear/Jira
- `{{SLOW mode only}}` - Content only shown in SLOW mode
- `{{LEGACY ONLY}}` - Content only for legacy projects

**Template references in workflows:**
- When workflows say "use {{TEMPLATE-NAME}}", load from `.agents/templates/TEMPLATE-NAME.md`
- Example: "use {{PARENT-TICKET-TEMPLATE}}" → load `.agents/templates/PARENT-TICKET-TEMPLATE.md`
- Templates are loaded on-demand, not preloaded
