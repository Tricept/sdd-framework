# SDD Framework Context

This file provides reference documentation for the SDD workflow. The actual instructions are in the agent files (`.github/agents/*.agent.md`).

## Workflow Overview

```
/sdd.start (or @sdd-start) - Phases 1-3 combined
    │
    ├── Creates feature branch
    ├── Gathers requirements (with codebase search)
    └── Generates specification
            │
            ▼ [User reviews spec]
/sdd.tasks (or @sdd-tasks) - Phase 4
    │
    └── Creates TDD-ordered task breakdown
            │
            ▼ [User reviews tasks]
/sdd.implement (or @sdd-implement) - Phase 5
    │
    └── TDD implementation (Red-Green-Refactor)
            │
            ▼
/sdd.validate (or @sdd-validate) - Phase 6
    │
    └── Verifies implementation vs spec
            │
            ▼
/sdd.complete (or @sdd-complete) - Phase 7
    │
    └── Moves spec to implemented/, finalizes
```

## Individual Phase Commands

For granular control, use individual phases:

- `/sdd.initiate` → `/sdd.shape` → `/sdd.specify` (instead of `/sdd.start`)

## Key Principles

1. **Specs are source of truth** - AI implements against specs, not imagination
2. **TDD is non-negotiable** - Tests before implementation, always
3. **Existing code first** - Search codebase before specifying new components
4. **Scope control** - Out of Scope section is binding
5. **Human-in-the-loop** - User reviews spec before tasks, tasks before implementation

## Folder Structure

```
specs/
├── active/                    # Specs in development
│   └── YYYY-MM-DD-feature/
│       ├── spec.md
│       ├── tasks.md
│       ├── requirements.md
│       ├── validation-report.md
│       └── visuals/
│
├── implemented/               # Completed specs
│
└── _templates/
    ├── spec.template.md
    └── tasks.template.md

standards/
└── global/
    ├── code-quality.md
    └── testing.md
```

## Jira Integration

When Atlassian MCP is available:
- Start from Jira issues: `/sdd.start PROJ-123`
- Auto-fetch issue details and attachments
- Sync tasks as subtasks
- Update issue status on completion

When unavailable:
- Graceful degradation
- If ticket ID provided without Jira, ask user for description
