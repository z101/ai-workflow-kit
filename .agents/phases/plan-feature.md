# Phase: Plan Feature

You are planning the implementation of a specific feature. Your goal is to produce a detailed feature document, generate acceptance tests that serve as an immutable contract, and update project rules if needed.

## Inputs

- **Feature request** from the user — one of:
  - **Feature ID from PRD** (e.g., `user-auth`) — look up the entry in the `docs/prd.md` feature table for context. Use this ID as-is.
  - **ID + description** (e.g., `realtime-notifications: push notifications on task changes`) — use the provided ID and description directly.
  - **Free-form description only** (e.g., `Add push notifications when tasks are updated`) — propose a kebab-case Feature ID and confirm with the user before creating any files.
  - **No input** — list features from `docs/prd.md` with status "planned" and ask the user which one to plan.

## Step 1: Understand Context

Read these files to understand the current project state:

1. `docs/prd.md` -- project overview, architecture, NFR, feature list
2. `.agents/rules/` -- relevant rule files for the feature's domain
3. Existing `docs/features/*.md` -- understand what has been built already
4. Existing `tests/` -- understand current test coverage and patterns
5. Recent git history -- check `git log` to understand current development focus and recent changes

Then research the codebase to identify specific files, functions, and modules that the feature will need to change or interact with. Pay special attention to:

- **Naming conventions** in the relevant area (casing, prefixes, file naming patterns)
- **Error handling** -- how errors are created, propagated, and reported
- **Integration patterns** -- how existing features connect (routing, middleware, event handling, registration)
- **Dependency usage** -- how relevant libraries are imported, configured, and called; note versions

Also research relevant external documentation for libraries and frameworks the feature will use. Collect links with specific section anchors and note why each is relevant. Look for known gotchas, breaking changes, or migration guides that could affect implementation.

This grounds the plan in reality rather than assumptions.

If requirements are unclear after researching the codebase, ask up to 5 focused clarifying questions before writing the plan. Incorporate the user's answers into the feature document.

## Step 2: Create Feature Document

Read the template at `.agents/templates/feature.md`. Create `docs/features/FEATURE-ID.md` following that template.

The Feature ID must be a short, descriptive kebab-case string (e.g., `user-auth`, `api-gateway`, `payment-processing`). See the Inputs section for how the ID is determined — never generate an ID silently without user confirmation.

Include specific, verbatim details from the user's description to ensure the plan accurately reflects their intent. Do not write actual code in the plan — describe what to do, not how to code it.

The feature document must contain:

### Specification section

- Clear description of what the feature does
- Feature type (New Capability / Enhancement / Refactor / Bug Fix) and estimated complexity (Low / Medium / High)
- Acceptance criteria (specific, testable conditions)
- Edge cases and error scenarios
- Dependencies on other features or external services
- API contracts or interface definitions where applicable

### Context References section

Provide all the context the exec agent needs to implement without additional research:

- **Mandatory reading** -- specific files (with line ranges) the exec agent must read before implementing, and why each is relevant
- **Relevant documentation** -- external docs, library guides, or framework references with section anchors and rationale for each link
- **Patterns to follow** -- codebase patterns extracted during Step 1 that this feature should mirror (reference source file and lines)
- **Anti-patterns to avoid** -- known pitfalls, deprecated approaches, or patterns that exist in the codebase but should not be replicated

### Implementation Plan section

Use action keywords for task clarity: **CREATE** (new files), **UPDATE** (modify existing), **ADD** (insert into existing code), **REFACTOR** (restructure without behavior change).

- Ordered task breakdown (what to implement first, second, etc.)
- Specific files and functions to create or modify (based on codebase research from Step 1)
- Step-by-step explanation of any non-trivial algorithms or logic
- Key architectural decisions specific to this feature
- Any new patterns or libraries introduced
- For large features, break work into logical phases: start with a data layer phase (types, schemas, DB changes), followed by phases that can proceed in parallel (e.g., Phase 2A — UI, Phase 2B — API). Only use phased breakdown when the feature is genuinely large.

Prioritize being concise and precise. Keep the plan tight without losing critical details from the user's requirements.

### Acceptance Test Manifest

- Explicit list of `*.acceptance.test.`* files that form the contract
- Brief description of what each test file covers

## Step 3: Generate Acceptance Tests

Create acceptance tests in `tests/features/FEATURE-ID/`. These tests define the behavioral contract:

- Use the naming convention `*.acceptance.test.*` (e.g., `login.acceptance.test.ts`, `test_login_acceptance.py`)
- Tests should describe WHAT the feature must do, not HOW it is implemented
- Tests should be runnable but expected to FAIL at this point (no implementation exists yet)
- Follow testing conventions from `.agents/rules/testing.md` if it exists
- Each acceptance criteria from the spec should have corresponding test coverage

**CRITICAL**: These acceptance test files become an immutable contract. The exec-feature phase MUST NOT modify or delete them. Document this in the feature file.

## Step 4: Update PRD

Update the feature table in `docs/prd.md`:

- If the feature is already listed, update its status to "in progress" and add the doc link
- If it's a new feature not in the original plan, add it to the table

## Step 5: Update Rules (if needed)

If the feature introduces new patterns, conventions, or architectural decisions that affect future work:

- Update the relevant `.agents/rules/*.md` file
- Or create a new rule file if the domain is new
- Update `AGENTS.md` references if a new rule file was created
- Update `opencode.json` instructions if a new rule file was created

## Output Checklist

Before finishing, verify:

- `docs/features/FEATURE-ID.md` exists with both spec and implementation plan sections
- Acceptance test manifest is listed in the feature doc
- `tests/features/FEATURE-ID/*.acceptance.test.`* files exist and are runnable (expected to fail)
- `docs/prd.md` feature table is updated
- Rules updated if new patterns were introduced

Report to the user:

- Summary of the feature specification
- List of acceptance tests created
- Recommended order of implementation tasks
- Any questions or ambiguities that need resolution before execution