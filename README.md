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

### In Cursor

Type `/` in Agent chat and select the phase:

- `/init-project` -- start a new project
- `/plan-feature` -- plan a feature
- `/exec-feature` -- implement a planned feature
- `/embed-feature` -- integrate a verified feature

### In OpenCode

Type `/` in the TUI and select the command:

- `/init-project` -- start a new project
- `/plan-feature` -- plan a feature
- `/exec-feature` -- implement a planned feature
- `/embed-feature` -- integrate a verified feature

OpenCode also auto-discovers the skills via its `skill` tool.

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