# AI Workflow Kit

A structured, cross-tool workflow for building applications with AI coding agents. Works with both **Cursor** and **OpenCode**.

The kit defines a 4-phase development pipeline that guides agents through documentation, planning, implementation, and integration -- feature by feature.

## Phases

```
init-project → plan-feature → exec-feature → embed-feature
                    ↑                              |
                    └──────── next feature ─────────┘
```


| Phase             | Purpose                                       | Key Output                                   |
| ----------------- | --------------------------------------------- | -------------------------------------------- |
| **init-project**  | Generate project documentation through Q&A    | `docs/prd.md`, `.agents/rules/`, `AGENTS.md` |
| **plan-feature**  | Plan a feature with spec and acceptance tests | `docs/features/*.md`, `tests/features/*/`    |
| **exec-feature**  | Implement until all acceptance tests pass     | Source code, implementation tests            |
| **embed-feature** | Integrate into docs, run cross-feature tests  | Updated PRD, integration/e2e tests           |


## Quick Start

### Use as a template

Clone the repository and start building:

```bash
git clone https://github.com/z101/ai-workflow-kit.git my-project
cd my-project
rm -rf .git && git init
```

### Add to an existing project

Copy these files/directories into your project:

- `.agents/` -- skills, phases, templates, rules
- `AGENTS.md` -- project rules (both tools read this)
- `opencode.json` -- OpenCode command definitions
- `docs/` -- documentation output directory

## Usage

In both Cursor and OpenCode, invoke phases via `/command-name` followed by arguments.

### 1. Initialize the project

Provide an initial project description. The agent asks focused follow-up questions, then generates the PRD, project rules, and `AGENTS.md`.

```
/init-project Real-time Kanban app for small teams with Slack integration and role-based access
```

The agent will:

- Ask 2-4 follow-up questions at a time (tech stack, auth, deployment, etc.)
- Wait for your confirmation before generating documentation
- Generate `docs/prd.md` with a feature table, `.agents/rules/`, and update `AGENTS.md`
- Ask you to review and suggest the first feature to plan

### 2. Plan a feature

Pass a Feature ID from the PRD, or describe a new feature.

```
# Feature from PRD — uses the existing ID and PRD context
/plan-feature user-auth

# New feature with explicit ID
/plan-feature realtime-notifications: push notifications when tasks change

# Free-form description — agent proposes an ID and confirms before proceeding
/plan-feature Add push notifications when tasks are updated

# No argument — agent lists planned features from PRD and asks which to plan
/plan-feature
```

The agent will:

- Research the codebase for relevant files, patterns, and dependencies
- Ask up to 5 clarifying questions if requirements are unclear
- Create `docs/features/FEATURE-ID.md` (spec + context references + implementation plan)
- Create acceptance tests in `tests/features/FEATURE-ID/` (immutable contract)
- Update the PRD feature table

### 3. Execute the feature

Pass the Feature ID. Optionally add instructions to adjust execution.

```
# Standard execution
/exec-feature user-auth

# With additional instructions
/exec-feature user-auth — start from task 3, data layer is done
/exec-feature user-auth — use passport v0.7, ignore v0.6 references in the plan
```

The agent will:

- Read the feature doc, acceptance tests, context references, and rules
- Verify acceptance tests exist and fail (no implementation yet)
- Implement task by task, running tests after each significant change
- Self-review the code (plan conformance, data alignment, style consistency)
- Report results and wait for your verification

### 4. Embed the feature

After you verify the implementation, integrate it into the project.

```
# Standard integration
/embed-feature user-auth

# With scope adjustments
/embed-feature user-auth — skip e2e tests for now
```

The agent will:

- Update `docs/features/FEATURE-ID.md` with what was actually built
- Update `docs/prd.md` (mark feature as "integrated")
- Create integration and E2E tests for cross-feature interactions
- Run the full test suite, fix any regressions
- Update rules if new patterns emerged
- Suggest the next feature to plan

### Full workflow example

```
/init-project Kanban board SaaS with team collaboration
  → Q&A → generates PRD with features: user-auth, kanban-board, slack-integration

/plan-feature user-auth
  → creates docs/features/user-auth.md + acceptance tests

/exec-feature user-auth
  → implements until all tests pass → you verify

/embed-feature user-auth
  → integrates docs, runs cross-feature tests → suggests next feature

/plan-feature kanban-board
  → next cycle begins...
```

## Project Structure

```
.agents/
├── skills/                        # Entry points (both tools discover these)
│   ├── init-project/SKILL.md
│   ├── plan-feature/SKILL.md
│   ├── exec-feature/SKILL.md
│   └── embed-feature/SKILL.md
├── phases/                        # Detailed workflow instructions
│   ├── init-project.md
│   ├── plan-feature.md
│   ├── exec-feature.md
│   └── embed-feature.md
├── templates/                     # Document templates
│   ├── prd.md                     # PRD structure (compact index)
│   └── feature.md                 # Feature doc structure (spec + plan)
└── rules/                         # Domain-specific rules (generated per project)

opencode.json                      # OpenCode commands + config
AGENTS.md                          # Project rules (both tools)
docs/                              # Generated documentation
```

## How It Works

### Documentation strategy

- `**docs/prd.md**` is a compact index (~200-500 lines) that never duplicates feature details. It contains project overview, NFR, architecture summary, and a feature table with links.
- `**docs/features/*.md**` are the source of truth for each feature. They evolve through the lifecycle: created during plan, referenced during exec, updated during embed.

### Test organization

```
tests/
├── features/                      # Per-feature tests
│   └── FEATURE-ID/
│       ├── *.acceptance.test.*    # Plan creates (IMMUTABLE in exec)
│       └── *.test.*               # Exec creates (mutable)
├── integration/                   # Cross-feature tests (embed creates)
└── e2e/                           # End-to-end tests (embed creates)
```

**Key rule**: `*.acceptance.test.`* files are created during plan-feature and serve as an immutable contract. The exec-feature phase must make them pass without modifying them.

### Modular rules

- `AGENTS.md` in the project root contains global conventions (loaded by both tools automatically).
- `.agents/rules/` contains domain-specific rule files (backend, frontend, testing, etc.) generated during init-project and updated as the project evolves.
- For OpenCode, rule files are also listed in `opencode.json` under `instructions` for automatic loading.

## Cross-Tool Compatibility


| Feature                  | Cursor                     | OpenCode                            |
| ------------------------ | -------------------------- | ----------------------------------- |
| Skills discovery         | `.agents/skills/` (native) | `.agents/skills/` (native)          |
| Slash command invocation | `/skill-name`              | `/command-name` via `opencode.json` |
| Project rules            | `AGENTS.md` (native)       | `AGENTS.md` (native)                |
| Rule auto-loading        | Via AGENTS.md references   | Via `opencode.json` instructions    |


Both tools read `AGENTS.md` and discover `.agents/skills/` natively. The only tool-specific file is `opencode.json`, which provides explicit `/command` invocation and automatic rule loading for OpenCode.

## License

MIT