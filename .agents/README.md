**Alpha Release** - This system has undergone significant refactoring and is actively being tested. Expect some rough edges and iteration as workflows are refined.

**Source-Available Repository** - This code is publicly available for reference and learning, but contributions are not accepted at this time. However, **feedback is welcomed and encouraged!** If you have suggestions, questions, or find issues, reach out on Twitter: [@MattHProgrammer](https://twitter.com/MattHProgrammer)

# Deliberate Agentic Development

A structured workflow system for AI coding agents like **Claude Code**, **Cursor**, and **Codex**. Guides AI assistants through software development with built-in checkpoints, reviews, and human oversight—keeping you in control from planning to shipping.

**Key Features:**
- **Structured Workflows** - Plan → Build → Ship with clear steps
- **Human-in-the-Loop** - Review and approve at every key decision
- **State Management** - Switch between agents seamlessly, they pick up where the last one left off
- **Linear Integration** - Full project tracking and task management
- **TDD Support** - Optional test-driven development mode

## Table of Contents

- [Overview](#overview)
- [Quick Setup](#quick-setup-4-steps)
- [File Structure](#file-structure)
- [Adapting to Your Tools](#adapting-to-your-tools)
- [Project Modes](#project-modes)
- [Documentation Organization](#documentation-organization)
- [Troubleshooting](#troubleshooting)

## Overview

Structured workflows for AI agents to plan and build software projects systematically:

- **Plan** - Vision → Tech stack → Milestones → Issues → Tasks
- **Build** - One task → One commit → One review → Repeat
- **Track** - State management + Linear integration for full visibility

## Quick Setup (4 steps)

### 1. Install Required Tools

```bash
# Install GitHub CLI (for PRs)
brew install gh  # macOS
# or see: https://cli.github.com

# Setup Linear MCP in your AI agent
# Claude: Settings → Model Context Protocol → Add Linear
# Cursor: Settings → Features → MCP Servers → Add Linear
# Codex: Settings → Extensions → Add Linear MCP
```

**Pro tip:** If using multiple agents, install MCPs globally so all agents share the same connections.

### 2. Copy Files to Your Project

```bash
# Copy workflow files to your project
cp AGENTS.md /path/to/your/project/
cp -r .agents/ /path/to/your/project/
```

**What you're copying:**
- `AGENTS.md` → Root of your project (main workflow entry point)
- `.agents/` → Root of your project (all workflow files and templates)

### 3. Configure PRE-COMMIT-RULES.md and PRE-PUSH-RULES.md (Recommended)

You can enable optional validation files by copying from templates and then editing to match your stack:

```bash
# Enable commit-time checks (recommended)
mkdir -p .agents/rules
cp .agents/templates/PRE-COMMIT-RULES-TEMPLATE.md .agents/rules/PRE-COMMIT-RULES.md

# Enable pre-push checks (recommended)
cp .agents/templates/PRE-PUSH-RULES-TEMPLATE.md .agents/rules/PRE-PUSH-RULES.md
```

Edit `.agents/rules/PRE-COMMIT-RULES.md` with your actual validation commands and, if desired, configure `.agents/rules/PRE-PUSH-RULES.md` for E2E/build checks:

```markdown
## Frontend Checks
- **format**: npm run format
- **lint**: npm run lint
- **typecheck**: npm run typecheck
```

### 4. Start Planning

Open your AI agent and say:
```
"Load AGENTS.md and let's start planning"
```

The agent will load the workflow system and guide you through project planning.

#### Optional: Initialize Product Overview

If `.agents/documentation/PRODUCT-OVERVIEW.md` is missing, create it from the template:

```bash
mkdir -p .agents/documentation
cp .agents/templates/PRODUCT-OVERVIEW-TEMPLATE.md .agents/documentation/PRODUCT-OVERVIEW.md
```

## File Structure

### Minimal Setup

What you start with:
```
your-project/
├── AGENTS.md                    # Main orchestrator
└── .agents/
    ├── PLAN.md                  # Planning orchestrator
    ├── PLAN-PROJECT.md          # Project planning workflow
    ├── PLAN-ISSUE.md            # Issue planning workflow
    ├── IMPLEMENT.md             # Implementation workflow
    ├── state.json               # Tracks progress
    ├── templates/               # Ready-to-use templates
    ├── documentation/
    │   ├── PRODUCT-OVERVIEW.md  # Your project vision (create from template)
    │   ├── systems/             # Feature documentation (empty to start)
    │   └── project-archive/     # Completed project exports (empty to start)
    └── rules/
        ├── PRE-COMMIT-RULES.md  # Commit-time checks (RECOMMENDED, create from template)
        └── PRE-PUSH-RULES.md    # Pre-push checks (RECOMMENDED, create from template)
```

### As Your Project Grows

After several milestones:
```
your-project/
├── AGENTS.md                    # Main orchestrator - loads first, coordinates everything
├── .agents/
│   ├── PLAN.md                  # Planning orchestrator - routes to project or issue planning
│   ├── PLAN-PROJECT.md          # Project planning - vision, tech stack, milestones
│   ├── PLAN-ISSUE.md            # Issue planning - task breakdown for all milestones
│   ├── IMPLEMENT.md             # Implementation - execute tasks, create commits, PRs
│   ├── state.json               # Current position tracker
│   ├── templates/               # All reusable templates
│   ├── documentation/
│   │   ├── PRODUCT-OVERVIEW.md    # Your project vision (create from template)
│   │   ├── systems/
│   │   │   ├── AUTHENTICATION.md    # [CURRENT] OAuth, sessions, permissions
│   │   │   ├── NOTIFICATIONS.md     # [CURRENT] Email, push, in-app
│   │   │   ├── PAYMENTS.md          # [CURRENT] Stripe integration
│   │   │   ├── SEARCH.md            # [LEGACY] Moving to Elasticsearch
│   │   │   └── FILE-UPLOAD.md       # [CURRENT] S3, image processing
│   │   └── project-archive/
│   │       └── 2024-Q1-MVP-{{TICKET_PREFIX}}-100/  # Project name + parent ticket ID
│   │           ├── PARENT-TICKET.md  # {{TICKET_PREFIX}}-100 parent ticket
│   │           ├── {{TICKET_PREFIX}}-101.md        # Individual issue
│   │           ├── {{TICKET_PREFIX}}-102.md        # Individual issue
│   │           └── {{TICKET_PREFIX}}-103.md        # Individual issue
│   └── rules/
│       ├── PRE-COMMIT-RULES.md
│       ├── PRE-PUSH-RULES.md
│       ├── CORE-RULES.md
│       ├── API-RULES.md             # REST conventions, error codes
│       ├── DATABASE-RULES.md        # Schema patterns, migrations
│       ├── FE-RULES.md              # Frontend rules/patterns
│       ├── BE-RULES.md              # Backend rules/patterns
│       └── testing/
│           ├── frontend/
│           │   ├── FE-UNIT-TEST-RULES.md
│           │   ├── FE-INTEGRATION-TEST-RULES.md
│           │   └── FE-E2E-TEST-RULES.md
│           └── backend/
│               ├── BE-UNIT-TEST-RULES.md
│               ├── BE-INTEGRATION-TEST-RULES.md
│               └── BE-API-TEST-RULES.md
```


## Adapting to Your Tools

**Not using Linear?**
- Find/replace "Linear" → "Jira" (or your PM tool)
- Update ticket patterns ({{TICKET_PREFIX}}-XXX → PROJ-XXX)

**Not using GitHub?**
- Replace `gh` commands with GitLab/Bitbucket CLI
- Adjust PR creation commands in workflows

## Project Modes

Set during planning phase:

**Project Speed:**
- **FAST** (default) - Quick demos, manual testing only
- **SLOW** - Production quality, TDD workflow, full test coverage

**Project Types:**
- **NEW** - Starting from scratch
- **LEGACY** - Working with existing codebase (includes M0 analysis phase)

**Special Modes:**
- **FREEMODE** - Lightweight conversations without workflow loading
  - `FREEMODE START` - Skip all workflows, treat as normal chat
  - `FREEMODE END` - Resume normal workflow from AGENTS.md

## Documentation Organization

The `.agents/` system has three documentation areas:

### 1. System Documentation (`.agents/documentation/systems/*.md`)

**Agent-maintained** - Automatically created/updated during implementation

Documents how features work in your application:
- `AUTHENTICATION.md` - OAuth flows, sessions, permissions
- `NOTIFICATIONS.md` - Email, push, in-app systems
- `PAYMENTS.md` - Stripe integration, webhooks
- `FILE-UPLOAD.md` - S3, image processing

**Status tags:** `[CURRENT]` for active systems, `[LEGACY]` for systems being replaced

### 2. Rules Documentation (`.agents/rules/*.md`)

**User-controlled** - Agent can suggest, but you decide

Define coding patterns and conventions:
- `PRE-COMMIT-RULES.md` - **RECOMMENDED** - Commit-time validation commands
- `PRE-PUSH-RULES.md` - **RECOMMENDED** - Pre-push checks (E2E/build)
- `CORE-RULES.md` - Error handling, logging, config
- `API-RULES.md` - REST conventions, error codes
- `DATABASE-RULES.md` - Schema patterns, migrations
- `FRONTEND-RULES.md` - Component structure, state
- `BACKEND-RULES.md` - Service architecture

### 3. Testing Rules (`.agents/rules/testing/`)

**User-controlled, SLOW mode only**

Organize by stack layer:
```
testing/
├── frontend/
│   ├── FE-UNIT-TEST-RULES.md
│   ├── FE-INTEGRATION-TEST-RULES.md
│   └── FE-E2E-TEST-RULES.md
└── backend/
    ├── BE-UNIT-TEST-RULES.md
    ├── BE-INTEGRATION-TEST-RULES.md
    └── BE-API-TEST-RULES.md
```

### Structure Examples by Project Type

<details>
<summary><strong>Full-Stack Web Application</strong></summary>

```
.agents/
├── documentation/
│   ├── PRODUCT-OVERVIEW.md
│   └── systems/
│       ├── AUTHENTICATION.md          # OAuth, JWT, sessions
│       ├── API-GATEWAY.md             # Routing, rate limiting
│       ├── DATABASE.md                # Schema, migrations, ORM
│       ├── FRONTEND-STATE.md          # Redux/Zustand patterns
│       ├── NOTIFICATIONS.md           # Email, push, in-app
│       └── FILE-UPLOAD.md             # S3, image processing
└── rules/
    ├── PRE-COMMIT-RULES.md            # RECOMMENDED (create from template)
    ├── PRE-PUSH-RULES.md              # RECOMMENDED (create from template)
    ├── CORE-RULES.md                  # Error handling, logging
    ├── API-RULES.md                   # REST conventions
    ├── DATABASE-RULES.md              # Schema patterns
    ├── FRONTEND-RULES.md              # Component structure
    ├── BACKEND-RULES.md               # Service architecture
    └── testing/
        ├── frontend/
        │   ├── FE-UNIT-TEST-RULES.md
        │   ├── FE-INTEGRATION-TEST-RULES.md
        │   └── FE-E2E-TEST-RULES.md
        └── backend/
            ├── BE-UNIT-TEST-RULES.md
            ├── BE-INTEGRATION-TEST-RULES.md
            └── BE-API-TEST-RULES.md
```

</details>

<details>
<summary><strong>Backend-Only API Service</strong></summary>

```
.agents/
├── documentation/
│   ├── PRODUCT-OVERVIEW.md
│   └── systems/
│       ├── AUTHENTICATION.md          # API keys, OAuth
│       ├── RATE-LIMITING.md           # Redis, token buckets
│       ├── DATABASE.md                # PostgreSQL, migrations
│       ├── CACHING.md                 # Redis strategy
│       └── QUEUE-PROCESSING.md        # Background jobs, workers
└── rules/
    ├── PRE-COMMIT-RULES.md            # RECOMMENDED (create from template)
    ├── PRE-PUSH-RULES.md              # RECOMMENDED (create from template)
    ├── CORE-RULES.md                  # Error handling, logging
    ├── API-RULES.md                   # Endpoint conventions
    ├── DATABASE-RULES.md              # Schema, indexes
    └── testing/
        └── backend/
            ├── BE-UNIT-TEST-RULES.md
            ├── BE-INTEGRATION-TEST-RULES.md
            └── BE-API-TEST-RULES.md
```

</details>

<details>
<summary><strong>Frontend-Only Application</strong></summary>

```
.agents/
├── documentation/
│   ├── PRODUCT-OVERVIEW.md
│   └── systems/
│       ├── STATE-MANAGEMENT.md        # Zustand/Redux patterns
│       ├── ROUTING.md                 # React Router setup
│       ├── API-CLIENT.md              # Axios/fetch patterns
│       ├── AUTHENTICATION.md          # Token handling, refresh
│       └── UI-COMPONENTS.md           # Design system, shared components
└── rules/
    ├── PRE-COMMIT-RULES.md            # RECOMMENDED (create from template)
    ├── PRE-PUSH-RULES.md              # RECOMMENDED (create from template)
    ├── CORE-RULES.md                  # Error handling, logging
    ├── COMPONENT-RULES.md             # Structure, naming, props
    ├── STYLING-RULES.md               # CSS conventions, Tailwind
    └── testing/
        └── frontend/
            ├── FE-UNIT-TEST-RULES.md
            ├── FE-INTEGRATION-TEST-RULES.md
            └── FE-E2E-TEST-RULES.md
```

</details>

<details>
<summary><strong>Monorepo / Microservices</strong></summary>

```
.agents/
├── documentation/
│   ├── PRODUCT-OVERVIEW.md
│   └── systems/
│       ├── SERVICE-MESH.md            # Inter-service communication
│       ├── AUTH-SERVICE.md            # Centralized auth
│       ├── USER-SERVICE.md            # User management
│       ├── PAYMENT-SERVICE.md         # Stripe integration
│       ├── NOTIFICATION-SERVICE.md    # Email/SMS service
│       └── API-GATEWAY.md             # Request routing
└── rules/
    ├── PRE-COMMIT-RULES.md            # RECOMMENDED (create from template)
    ├── PRE-PUSH-RULES.md              # RECOMMENDED (create from template)
    ├── CORE-RULES.md                  # Cross-service patterns
    ├── API-RULES.md                   # REST/gRPC conventions
    ├── SERVICE-RULES.md               # Service boundaries
    ├── DATABASE-RULES.md              # Per-service DBs
    └── testing/
        ├── shared/
        │   └── INTEGRATION-TEST-RULES.md
        └── services/
            ├── AUTH-TEST-RULES.md
            ├── USER-TEST-RULES.md
            └── PAYMENT-TEST-RULES.md
```

</details>

## Troubleshooting

| Issue | Solution |
|-------|----------|
| "What is the state?" | Check `.agents/state.json` |
| "Validation failing" | If using PRE-COMMIT-RULES.md/PRE-PUSH-RULES.md, ensure commands are real, not placeholders |
| "Wrong workflow" | Planning = PLAN.md, Implementation = IMPLEMENT.md |
| "Can't find Linear" | Install Linear MCP in your AI agent settings |

---

**License:** MIT - See [LICENSE](../LICENSE) file for details.
