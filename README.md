# SDD Framework (Spec-Driven Development)

A reusable spec-driven development framework for VS Code Copilot that guides TDD-based development through structured phases.

## Quick Start

### New Project

```
/sdd.init-project
```

The AI will ask about your tech stack, architecture, and preferences, then generate a complete project structure with SDD framework configured.

### Existing Project

```
/sdd.onboard
```

The AI will analyze your codebase to discover:
- Tech stack (backend, frontend, database, testing)
- Naming conventions and code patterns
- Existing linting/formatting rules
- Test organization and coverage targets

Then generate standards files that match your existing conventions.

### Manual Installation

If you prefer manual setup:
1. **Copy this folder** to your project root
2. **Customize standards** in `standards/global/` for your tech stack
3. **Start a feature** with `/sdd.start your feature description`

---

## Installation (Existing Projects)

Copy the entire `sdd-framework/` contents to your project:

```bash
cp -r sdd-framework/.github /path/to/your/project/
cp -r sdd-framework/standards /path/to/your/project/
cp -r sdd-framework/specs /path/to/your/project/
cp -r sdd-framework/product /path/to/your/project/  # Optional
```

Your project structure should include:

```
your-project/
├── .github/
│   ├── agents/                # Agent definitions (main logic)
│   │   ├── sdd-init-project.agent.md   # New project setup
│   │   ├── sdd-onboard.agent.md        # Existing project onboarding
│   │   ├── sdd-start.agent.md
│   │   ├── sdd-initiate.agent.md
│   │   ├── sdd-shape.agent.md
│   │   ├── sdd-specify.agent.md
│   │   ├── sdd-tasks.agent.md
│   │   ├── sdd-implement.agent.md
│   │   ├── sdd-validate.agent.md
│   │   └── sdd-complete.agent.md
│   │
│   ├── prompts/               # Command wrappers
│   │   ├── sdd-init-project.prompt.md
│   │   ├── sdd-onboard.prompt.md
│   │   ├── sdd-start.prompt.md
│   │   └── ... (one per phase)
│   │
│   └── instructions/
│       └── sdd-context.md
│
├── standards/
│   └── global/
│       ├── code-quality.md
│       └── testing.md
│
├── specs/
│   ├── active/
│   ├── implemented/
│   └── _templates/
│       ├── spec.template.md
│       └── tasks.template.md
│
├── product/                   # Optional product docs
│   ├── mission.template.md
│   └── roadmap.template.md
│
└── [your project files]
```

---

## Workflow Overview

```
┌─────────────────────────────────────────────────────────────────┐
│  PROJECT SETUP (New Projects Only)                              │
│  ─────────────────────────────────                              │
│  /sdd.init-project                                              │
│    ├── Q&A: tech stack, architecture, testing                   │
│    ├── Generate project structure                               │
│    ├── Generate standards for your stack                        │
│    └── Create mission.md                                        │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│  FEATURE DEVELOPMENT (Repeatable)                               │
│  ────────────────────────────────                               │
│                                                                 │
│  /sdd.start (Phases 1-3 combined)                               │
│      ├── Creates feature branch                                 │
│      ├── Searches codebase for similar features                 │
│      ├── Gathers requirements through Q&A                       │
│      └── Generates specification                                │
│              │                                                  │
│              ▼ [User reviews spec]                              │
│                                                                 │
│  /sdd.tasks (Phase 4)                                           │
│      └── Creates TDD-ordered task breakdown                     │
│              │                                                  │
│              ▼ [User reviews tasks]                             │
│                                                                 │
│  /sdd.implement (Phase 5)                                       │
│      └── TDD implementation (Red-Green-Refactor)                │
│              │                                                  │
│              ▼                                                  │
│  /sdd.validate (Phase 6)                                        │
│      └── Verifies implementation vs spec                        │
│              │                                                  │
│              ▼                                                  │
│  /sdd.complete (Phase 7)                                        │
│      └── Moves spec to implemented/, finalizes                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## Commands

### Project Setup

| Command | Description |
|---------|-------------|
| `/sdd.init-project` | Initialize **new** project - asks about tech stack, generates structure |
| `/sdd.onboard` | Onboard **existing** project - analyzes codebase, infers standards |

### Feature Development

| Command | Description |
|---------|-------------|
| `/sdd.start [description]` | Start new spec - combines phases 1-3 |
| `/sdd.start PROJ-123` | Start from Jira issue (if MCP available) |

### Individual Phase Commands

| Command | Phase | Description |
|---------|-------|-------------|
| `/sdd.initiate` | 1 | Create branch and folder structure |
| `/sdd.shape` | 2 | Gather requirements through Q&A |
| `/sdd.specify` | 3 | Generate specification document |
| `/sdd.tasks` | 4 | Create TDD-ordered task breakdown |
| `/sdd.implement` | 5 | TDD implementation |
| `/sdd.validate` | 6 | Verify implementation matches spec |
| `/sdd.complete` | 7 | Finalize and move to implemented/ |

### Using Agents Directly

Invoke agents with `@`:
- `@sdd-init-project` - Initialize new project
- `@sdd-onboard` - Onboard existing project
- `@sdd-start` - Start new feature spec
- `@sdd-tasks` - Create task breakdown
- etc.

---

## Product Planning (Optional)

The framework includes optional product-level documents:

### Mission Document (`product/mission.md`)

Defines project purpose, target users, and tech stack. Generated by `/sdd.init-project` or created manually from `mission.template.md`.

### Roadmap (`product/roadmap.md`)

Optional local roadmap for feature planning. **Not required** if using external tools like Jira.

**Roadmap → Specs flow:**
```
Roadmap Item → /sdd.start "feature name" → Spec in specs/active/
```

**Jira → Specs flow:**
```
Jira Epic/Ticket → /sdd.start PROJ-123 → Spec in specs/active/
```

---

## Key Features

### Handoffs Between Phases

After each phase completes, the agent suggests the next step via a handoff button. This guides you through the workflow without needing to remember commands.

### TDD-First Approach

All implementation follows Red-Green-Refactor:
1. **RED**: Write failing tests first
2. **GREEN**: Write minimal code to pass
3. **REFACTOR**: Improve while keeping tests green

### Existing Code Discovery

Before specifying new features, the framework searches your codebase for:
- Similar features to reference
- Reusable components
- Patterns to follow

### Scope Control

The "Out of Scope" section prevents feature creep:
- Explicitly lists what's NOT included
- Enforced during implementation
- Verified during validation

### Jira Integration (Optional)

If Atlassian MCP is available:
- Start from Jira issues: `/sdd.start PROJ-123`
- Auto-fetch issue details and attachments
- Graceful fallback if unavailable

---

## Folder Structure

### specs/active/

Specs currently in development:

```
specs/active/2025-01-29-user-auth/
├── spec.md              # The specification
├── tasks.md             # Task breakdown
├── requirements.md      # Raw requirements from shaping
├── validation-report.md # Generated by validate phase
└── visuals/             # Mockups, wireframes
```

### specs/implemented/

Completed specs (moved on completion).

### standards/global/

Quality standards (customize for your stack):
- `code-quality.md` - Code quality rules
- `testing.md` - TDD and testing requirements

### product/

Optional product documentation:
- `mission.md` - Project mission and overview
- `roadmap.md` - Feature roadmap (if not using external tool)

---

## Best Practices

1. **New projects**: Use `/sdd.init-project` to get proper setup
2. **Always start features with `/sdd.start`** - Don't skip the spec
3. **Review before proceeding** - Check specs and tasks before implementation
4. **Follow TDD strictly** - Tests before code, always
5. **Respect scope** - Out of scope means out of scope
6. **Use handoffs** - Let the workflow guide you

---

## Architecture

The framework uses two types of files:

- **Prompt files** (`.github/prompts/`): Minimal wrappers for `/command` invocation
- **Agent files** (`.github/agents/`): Full instructions and handoffs

This separation allows:
- Clean `/sdd.*` command syntax
- Rich agent behavior with handoffs
- Easy customization of either layer

---

## License

MIT - Use freely in your projects.
