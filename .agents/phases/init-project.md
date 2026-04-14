# Phase: Init Project

You are initializing a new project. Your goal is to produce comprehensive project documentation through iterative Q&A with the user, then generate project rules tailored to the chosen stack.

## Inputs

- User's free-form project description (provided as context or arguments)

## Step 1: Gather Requirements

Conduct an iterative Q&A with the user. Ask focused questions in small batches (2-4 at a time). Cover these areas until you have sufficient clarity:

- **Domain & purpose**: What problem does this solve? Who are the target users?
- **Scope & boundaries**: What is in scope for v1? What is explicitly out of scope?
- **Tech stack**: Language, framework, database, infrastructure preferences. If the user has no preference, recommend a stack and explain why.
- **Scale expectations**: Expected load, data volume, number of users
- **Deployment**: Cloud provider, containerization, CI/CD preferences
- **Integrations**: External APIs, third-party services, auth providers
- **Security & compliance**: Authentication method, data sensitivity, regulatory requirements
- **Non-functional requirements**: Performance targets, availability, accessibility standards

Do NOT proceed to documentation until the user confirms they have provided enough information or explicitly asks you to proceed.

## Step 2: Generate PRD

Read the template at `.agents/templates/prd.md`. Generate `docs/prd.md` following that template structure.

The PRD is a **compact index** (~200-500 lines). It must NOT contain detailed feature specifications. Instead:

- Write a clear project overview, goals, and scope
- Document NFR as cross-cutting concerns
- Describe architecture at a summary level (components, data flow, tech stack decisions)
- List all identified features in a table with priorities and status ("planned")
- Each feature in the table links to a future `docs/features/FEATURE-ID.md` (these will be created during plan-feature phase)

## Step 3: Generate Project Rules

Based on the determined tech stack and architecture, generate domain-specific rule files in `.agents/rules/`. Create only the files relevant to the project. Common examples:

- `.agents/rules/backend.md` -- backend conventions, API patterns, error handling
- `.agents/rules/frontend.md` -- UI conventions, component patterns, styling
- `.agents/rules/database.md` -- schema conventions, migration patterns, query patterns
- `.agents/rules/testing.md` -- test framework config, naming conventions, test runner commands

Each rule file should contain actionable, specific conventions -- not generic advice the agent already knows. Include concrete examples where they help.

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

- [ ] `docs/prd.md` exists and follows the template structure
- [ ] Feature table in PRD lists all identified features with priorities
- [ ] `.agents/rules/` contains at least one rule file relevant to the stack
- [ ] `AGENTS.md` references the generated rule files
- [ ] `opencode.json` instructions array includes the rule file paths

Report to the user what was generated and suggest which feature to plan first.
