# IMPLEMENTING Phase

This document contains the detailed implementation workflow. It's loaded after project planning is complete.

## Table of Contents

1. [Implementation Workflow](#1-implementation-workflow)
   - 1.1 [Issue Selection](#11-issue-selection)
   - 1.2 [Issue Plan Confirmation](#12-issue-plan-confirmation)
   - 1.3 [Branch Setup](#13-branch-setup)
   - 1.4 [Test Development](#14-test-development-slow-only)
   - 1.5 [Task Implementation](#15-task-implementation)
   - 1.6 [Pre-Commit Validation](#16-pre-commit-validation)
   - 1.7 [Pre-Push Checks](#17-pre-push-checks)
   - 1.8 [Commit Review & Commit](#18-commit-review--commit)
   - 1.9 [Interrupt Flows](#19-interrupt-flows-bugfixes--conflicts)
   - 1.10 [PR Review & Merge](#110-pr-review--merge)
   - 1.11 [Post-PR-Merge Actions](#111-post-pr-merge-actions)
   - 1.12 [Project Archive](#112-project-archive-project-complete)
2. [FAST vs SLOW Mode Differences](#2-fast-vs-slow-mode-differences)
3. [Checkpoint System](#3-checkpoint-system)
4. [State Management](#4-state-management)
5. [Documentation Updates](#5-documentation-updates)
6. [Templates Reference](#6-templates-reference)

## 1. Implementation Workflow

**The complete implementation cycle:**

1. **Issue Selection** - Select first/next issue from milestone (see 1.1)
2. **Issue Plan Confirmation** - Review and confirm task list from Issue Planning phase (see 1.2)
3. **Branch Setup** - Create feature branch, set issue to "In Progress" (see 1.3)
4. **Task Loop:**
   a. **Test Development** - {{SLOW ONLY}} Write tests using TDD (see 1.4)
   b. **Task Implementation** - Code one task following defined approach (see 1.5)
   c. **Pre-Commit Validation** - If configured, run checks defined in `.agents/rules/PRE-COMMIT-RULES.md` (see 1.6)
   d. **Commit Review** - Present review to user, get approval [CHECKPOINT - Review] (see 1.8)
   e. **Commit** - Execute git commit with proper message format (see 1.8)
   f. **Linear Task Checkbox** - Check off completed task in issue description
   g. **Linear Comment** - Add commit details as comment
   h. **State Update** - Update state.json (increment task number)
   i. **Check Documentation** - Does this commit introduce new systems or change existing ones? Update if needed (see Section 5)
   j. **More Tasks?** If yes → Loop back to step 4a
5. **Pre-Push Checks** - If configured locally, run checks defined in `.agents/rules/PRE-PUSH-RULES.md` before pushing; otherwise rely on CI
6. **PR Process:**
   a. **State Update** - Update state.json (move to next issue or milestone)
   b. **Create PR** - Push branch and create PR from feature branch to master (see 1.10)
   c. **PR Review** - Present PR review to user, get approval [CHECKPOINT - Review] (see 1.10)
   d. **Merge PR** - Auto-approve and merge after approval (see 1.10)
   e. **Mark Issue Done** - Update issue status to Done in Linear (see 1.11)
   f. **Linear PR Comment** - Add PR review content as comment
   g. **Check Documentation** - Review what changed in this issue, update docs if needed (see Section 5)
   h. **More Issues?** If yes → Return to step 1
7. **Milestone Completion:**
   a. **Milestone Review** - Present milestone review, get approval [CHECKPOINT - Review] (see 1.11)
   b. **State Update** - Update state.json (move to next milestone)
   c. **Update Parent Ticket** - Mark milestone complete in parent ticket (see 1.11)
   d. **Linear Milestone Comment** - Add milestone review as comment to Parent Ticket
   e. **Check Documentation** - Comprehensive review of all milestone changes, update docs if needed (see Section 5)
   f. **More Milestones?** If yes → Return to step 1 for next milestone

### 1.1 Issue Selection

- Start with first issue (typically Data layer) or select based on dependencies
- Load task list from Linear, review dependencies, note shared components
- Load relevant systems/*.md (if they exist), rules, {{SLOW ONLY}} test rules based on task type

### 1.2 Issue Plan Confirmation

**Review:**
- Read Linear task list and planned approaches
- Verify shared components still apply
- Check for codebase changes since planning
- Confirm dependencies and patterns unchanged

**Adapt when:**
- Architecture changes from previous issues
- Better patterns emerged
- Dependencies shifted
- Technical constraints discovered

**Adaptation Process:**
1. Document what changed
2. Present CHECKPOINT: Decision (original vs new, impact, recommendation)
3. Update issue: description, tasks, dependencies
4. Add Linear comment about plan change
5. Update docs: PRODUCT-OVERVIEW.md, systems/*.md

### 1.3 Branch Setup

- `git pull origin master`
- Create feature branch (naming: AGENTS.md Section 2)
- Set Issue to "In Progress" in Linear

**Branch Naming:**
- Single PR per issue (default): `{{TICKET_PREFIX}}-XXX`
- Multi-part PRs: `{{TICKET_PREFIX}}-XXX-pt-2`, `{{TICKET_PREFIX}}-XXX-pt-3`

**When to Split into Multiple PRs:**
- User explicitly requests split
- Issue requires refactoring existing code before adding new features
- Bug fixes discovered that should merge independently
- Large issue where early feedback is valuable

**Note:** Default is ONE PR per issue. Multi-part PRs are the exception, not the rule.


### 1.4 Test Development **{{SLOW ONLY}}**

*Test notation: PLAN.md Section 5. Commands: PRE-COMMIT-RULES.md. Watch mode recommended.*

**`[+unit tests]`:** Red (write failing test per rules) → Green (minimal code) → Refactor → Commit together

**`[intg test]`:** Review features → Write integration tests per *-INTEGRATION-TEST-RULES.md → Verify pass → Commit

**E2E Testing issues:** Implement scenarios per *-E2E-TEST-RULES.md → Run full suite → Coverage review (REQUIRED) → Document gaps

### 1.5 Task Implementation

**⚠️ CRITICAL: Work on exactly ONE task at a time**

- Follow approach defined during Issue Planning
- Follow existing codebase patterns
- Apply security rules
- **{{SLOW ONLY}}** Use TDD workflow (Section 1.4)
- Unexpected fork in road → CHECKPOINT: Decision

**Flow per task:**
a. Implement the task
b. Identify modified code type (frontend/backend/both)
c. Run validation (1.6)
d. Present commit review [CHECKPOINT]
e. Get user approval
f. Execute git commit
g. Check off task in Linear
h. Add Linear comment
i. Update state.json

### 1.6 Pre-Commit Validation

Use `.agents/rules/PRE-COMMIT-RULES.md` to define commit-time checks (for example, format, lint, typecheck, unit/integration tests).

- If the file is missing, skip this step.
- Run the configured checks and ensure all exit with code 0.
- In SLOW mode, include test commands; do not commit until checks pass.

### 1.7 Pre-Push Checks

Use `.agents/rules/PRE-PUSH-RULES.md` to define checks to run before pushing (for example, Playwright E2E and a production build).

- If the file is missing or the project is in FAST mode, skip locally.
- Run the configured checks and ensure all exit with code 0.
- In SLOW mode, ensure checks pass before merge; run via a local pre-push hook or rely on CI.

Run locally (pre-push) or in CI to keep commits fast while protecting the main branch.

### 1.8 Commit Review & Commit

**[CHECKPOINT - Review]**
- Confirm all lint/format checks passed
- Determine next step based on remaining tasks
- Present commit review using {{COMMIT-REVIEW-TEMPLATE}}
- Get explicit user approval
- Do not execute git commands until approved

**Post-Commit-Review Actions:**
- Execute git commit
- Check off task in Linear (`- [x]` format, no emojis)
- Add Linear comment (exact Commit Review text that was approved)
- Update state.json (increment task number)
- Check documentation: Did this commit introduce new systems or change existing ones? Update if needed (see Section 5)
- **Next:** More tasks? → 1.5, Tasks done? → 1.10

### 1.9 Interrupt Flows (Bugfixes & Conflicts)

#### 1.9.1 Bug Documentation Flow

When bug discovered during implementation:

a. Diagnose using {{PROBLEM-INVESTIGATION-TEMPLATE}} if needed
b. Identify which issue/task introduced the bug
c. Navigate to source Linear issue and uncheck the broken task (mark incomplete)
d. Add comment to source issue: "Bug found: [description]. Discovered during M[current].[issue]. Needs fixing."
e. (Optional) Track in current issue: Add task "Track: Bug in M[X].[Y].[Z] - [description]"
f. Surface to user: Present findings and ask "Fix now in current branch, or continue current work?"
g. User decides priority

**Important:** Do NOT automatically fix bugs. Document them and let user decide when to address.

#### 1.9.2 Merge Conflict

When conflicts occur:
a. Surface conflict to user
b. Present conflicting sections
c. Get user guidance
d. Apply resolution
e. Continue workflow

### 1.10 PR Creation & Review

**Create PR first, then present review:**

a. Update state.json (move to next issue/milestone)
b. Push branch to remote
c. Create PR: feature branch to master (title per AGENTS.md Section 2, description using {{PR-REVIEW-TEMPLATE}})
d. **[CHECKPOINT - Review]** Present PR review to user (PR is live for viewing in GitHub or fresh agent session)
e. Get explicit user approval

**Post-PR-Review Actions:**

f. Auto-approve and merge PR (after user approval)
g. Mark Issue as Done in Linear
h. Add Linear comment (exact PR Review content)
i. Check documentation: Review what changed in this issue, update docs if needed (see Section 5)

### 1.11 Post-PR-Merge Actions

#### 1.11.1 Check Milestone Status
- Check for remaining issues in milestone
- **Next:** More issues? → 1.1, Milestone complete? → Continue to 1.11.2

#### 1.11.2 Milestone Review **[CHECKPOINT - Review]**
- Determine next step based on remaining milestones
- Present milestone review using {{MILESTONE-REVIEW-TEMPLATE}}
- Review all shipped features
- Verify demo readiness
- Get user approval

#### 1.11.3 Post-Milestone-Review Actions
- Update state.json (move to next milestone)
- Update Parent Ticket (mark milestone complete)
- Add Linear comment with milestone review to Parent Ticket
- Check documentation: Comprehensive review of all milestone changes, update docs if needed (see Section 5)
- **Next:** More milestones? → 1.1, Project complete? → 1.12

### 1.12 Project Archive (Project Complete)

1. **Celebrate completion!** 🎉
2. Ask user: "Would you like to archive the project documentation from Linear?"
3. **If YES, create archive:**

   **Archive Structure:**
   ```
   .agents/documentation/project-archive/{{PROJECT_NAME}}-{{PARENT_TICKET_ID}}/
   ├── PARENT-TICKET.md           # Parent ticket with all milestones
   ├── {{TICKET_PREFIX}}-101.md   # First issue (M1.1)
   ├── {{TICKET_PREFIX}}-102.md   # Second issue (M1.2)
   └── {{TICKET_PREFIX}}-XXX.md   # ... all issues
   ```

   **File Contents:**
   - Copy exact content from Linear including:
     - Issue title and description
     - All tasks (with [x] checkboxes showing completion)
     - All comments
     - Issue metadata (status, assignee, dates)

4. Update PRODUCT-OVERVIEW.md with final state
5. Reset state.json for next project

---

## 2. FAST vs SLOW Mode Differences

**FAST Mode:**
- Goal: Ship demos quickly
- Testing: Manual only
- Skip: Section 1.4 (Test Development), test results in reviews
- When: MVPs, prototypes, demos
 - Pre-Push Checks: Skipped (no E2E/build enforced)

**SLOW Mode:**
- Goal: Production-ready code
- Testing: Full TDD with unit & E2E tests
- Requires: Tests before implementation, coverage in reviews
- When: Production systems, critical features
 - Pre-Push Checks: Required (E2E suite + production build must pass)

**Both Modes Require:**
- Linting and formatting
- Manual testing
- All review checkpoints
- Documentation updates

---

## 3. Checkpoint System

*See AGENTS.md Section 6.1 for complete Decision Framework*

**CHECKPOINT: Decision**
- Fork in road requiring user input
- Use {{DECISION-CHECKPOINT-TEMPLATE}}
- Present options with pros/cons
- Get guidance on path

**CHECKPOINT: Review**
- Commit Review: Use {{COMMIT-REVIEW-TEMPLATE}}
- PR Review: Use {{PR-REVIEW-TEMPLATE}}
- Milestone Review: Use {{MILESTONE-REVIEW-TEMPLATE}}

**All checkpoints mandatory - cannot proceed without explicit approval**

---


## 4. State Management

*State structure defined in AGENTS.md Section 5.1*

### When to Update state.json

**Update Triggers:**
- After Commit Review approval: increment task number
- After PR Review approval: move to next issue, reset task to 1
- After Milestone Review approval: move to next milestone, reset issue/task
- Phase transitions: update project_phase

**Timing:** State updates AFTER approval checkpoints (Commit Review, PR Review, Milestone Review). State = approved work only.

### State Format

`M[milestone].[issue].[task]` (Example: `M2.4.3`)

### Workflow Continuity

- Auto-saves after task completion and issue switching
- Persists across sessions
- On resume: Shows "Resuming M2.4.3 on branch {{TICKET_PREFIX}}-101"

## 5. Documentation Updates

### When to Update Documentation

**Progressive documentation approach - catch changes early:**

**After EACH COMMIT:** Quick check
- Did this introduce a new system or pattern?
- Did this change existing system behavior?
- Update systems/*.md if needed (or create new doc)

**After EACH PR:** Issue-level check
- Review all commits in this issue
- What systems were touched?
- Update PRODUCT-OVERVIEW.md if core features or architecture changed
- Update/create systems/*.md for any new or modified systems
- Legacy systems replaced? Remove [LEGACY] tag

**After EACH MILESTONE:** Comprehensive check
- Full PRODUCT-OVERVIEW.md review (tech stack, features, architecture, vision)
- Ensure all systems/*.md reflect milestone changes
- Identify patterns that emerged (ask user if they want new rules added)

**What to Document:**

**PRODUCT-OVERVIEW.md** - Update when commits/PRs/milestones change:
- Tech stack or versions
- Core features or capabilities
- System architecture
- Project vision or goals

**System Documentation** `.agents/documentation/systems/*.md` - Update when commits/PRs/milestones change:
- New system implemented (create doc)
- Existing system behavior
- Integration points or APIs
- Legacy system status (remove [LEGACY] tag when replaced)

**Rules** `.agents/rules/*.md` - **USER DECIDES ONLY:**
- Never automatically create or update rules
- Ask user after milestones: "Did any patterns emerge that should be documented as rules?"
- User provides rules or guidance, agent updates files

### Documentation Guidelines

**System docs include:** Purpose, tech stack + links, key components, data flow, config

**Stay concise:** Systems = "how it works" not details, PRODUCT-OVERVIEW under 200 lines, Rules = must-follow patterns only

## 6. Templates Reference

### Decision & Investigation Templates

**Templates for problem-solving:**
- **DECISION-CHECKPOINT** - Multiple viable approaches
- **PROBLEM-INVESTIGATION** - Bugs, failures, unexpected behavior

### Review Templates

**Templates for review checkpoints:**
- **COMMIT-REVIEW** - Pre-commit approval
- **PR-REVIEW** - Pre-PR approval
- **MILESTONE-REVIEW** - Milestone completion

**Formatting conventions defined in AGENTS.md Section 2:**
- Commit messages
- PR titles
- Branch naming

### Documentation Templates

**Templates for new documentation:**
- {{SYSTEM-DOCUMENTATION-TEMPLATE}} - For new systems
- {{PRODUCT-OVERVIEW-TEMPLATE}} - Initial project setup

---

## Transition Back to Planning

To redefine milestones or restructure: See `.agents/PLAN.md`

Otherwise, continue implementing until project complete.
