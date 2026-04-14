# Phase: Init Project

You are initializing a new project. Your goal is to produce comprehensive project documentation through iterative Q&A with the user, then generate project rules tailored to the chosen stack.

## Inputs

- **Project description** from the user — a free-form text describing what they want to build
  - If a description is provided, use it as the starting point — skip generic questions already answered by the description and jump to focused follow-ups
  - If no description is provided, ask the user to briefly describe their project idea before starting the Q&A

## Step 1: Gather Requirements

Analyze the user's initial description (if provided) and identify which areas below are already covered and which need clarification. Then conduct an iterative Q&A, asking focused questions in small batches (2-4 at a time). Cover these areas until you have sufficient clarity:

- **Domain & purpose**: What problem does this solve? Why does it need to exist?
- **Target audience**: Who are the primary users? What are their roles, goals, and pain points?
- **Primary benefits**: What are the key value propositions? What outcomes will users achieve?
- **Scope & boundaries**: What is in scope for v1? What is explicitly out of scope?
- **Tech stack**: Language, framework, database, infrastructure preferences. If the user has no preference, recommend a stack and explain why.
- **Scale expectations**: Expected load, data volume, number of users
- **Deployment**: Cloud provider, containerization, CI/CD preferences
- **Integrations**: External APIs, third-party services, auth providers
- **Security & compliance**: Authentication method, data sensitivity, regulatory requirements
- **Non-functional requirements**: Performance targets, availability, accessibility standards
- **Risks**: Known technical risks, external dependencies that could fail, areas of uncertainty

Do NOT proceed to documentation until the user confirms they have provided enough information or explicitly asks you to proceed.

## Step 2: Generate PRD

Read the template at `.agents/templates/prd.md`. Generate `docs/prd.md` following that template structure.

The PRD is a **compact index** (~200-500 lines). It must NOT contain detailed feature specifications. Instead:

- Write a clear project overview, goals, target audience, and primary benefits
- Define scope boundaries (in/out of scope for v1)
- Document NFR as cross-cutting concerns
- Describe architecture at a summary level (components, data flow, tech stack decisions)
- Identify key risks and mitigations (technical risks, uncertain areas, critical dependencies)
- List all identified features in a table with priorities and status ("planned")
- Each feature in the table links to a future `docs/features/FEATURE-ID.md` (these will be created during plan-feature phase)

Adapt depth based on the product type: for highly technical products, emphasize architecture and tech stack decisions; for user-facing products, emphasize target audience, user needs, and experience goals.

## Step 3: Generate Project Rules

### 3a: Analyze existing codebase (if applicable)

If the project already has code, analyze it before generating rules to ensure they reflect actual patterns rather than assumptions:

1. **Identify project type** -- web app (full-stack, frontend-only, API/backend), library/package, CLI tool, monorepo, or script/automation
2. **Analyze configuration files** -- package.json, tsconfig.json, pyproject.toml, go.mod, Cargo.toml, build configs, linter configs, and similar. Extract the actual tech stack, dependencies, and tool settings.
3. **Map directory structure** -- where source code lives, where tests are, shared code locations, configuration layout
4. **Extract existing patterns** -- study the code for naming conventions (files, functions, classes), code organization within files, error handling patterns, type/interface definitions, and test structure
5. **Identify key files** -- entry points, core business logic, shared utilities, type definitions

If the project is new (no existing code), base rules on the tech stack and architecture decided during Step 1.

### 3b: Generate rule files

Generate domain-specific rule files in `.agents/rules/`. Create only the files relevant to the project. Common examples:

- `.agents/rules/backend.md` -- backend conventions, API patterns, error handling
- `.agents/rules/frontend.md` -- UI conventions, component patterns, styling
- `.agents/rules/database.md` -- schema conventions, migration patterns, query patterns
- `.agents/rules/testing.md` -- test framework config, naming conventions, test runner commands

Each rule file should contain actionable, specific conventions -- not generic advice the agent already knows. Include concrete examples where they help. Focus on patterns and conventions, not exhaustive documentation. Do not duplicate information that belongs in the PRD or feature docs.

## Step 4: Update AGENTS.md

Update the root `AGENTS.md` with:

- Project-specific global conventions (commit format, branch naming, code style)
- References to the generated rule files in `.agents/rules/`

Format the references as instructions the agent can follow:

```
When working on backend code, read .agents/rules/backend.md for conventions.
When writing tests, read .agents/rules/testing.md for conventions.
```

## Step 5: Update opencode.json

Add the generated rule file paths to the `instructions` array in `opencode.json` so OpenCode loads them automatically.

## Output Checklist

Before finishing, verify:

- `docs/prd.md` exists and follows the template structure
- Feature table in PRD lists all identified features with priorities
- `.agents/rules/` contains at least one rule file relevant to the stack
- `AGENTS.md` references the generated rule files
- `opencode.json` instructions array includes the rule file paths

Report to the user:

- Summary of what was generated (PRD overview, rule files created)
- Any assumptions made due to missing information
- The feature list with priorities from the PRD
- Suggested first feature to plan

Ask the user to review the generated PRD and rules. Incorporate any corrections before considering init-project complete.