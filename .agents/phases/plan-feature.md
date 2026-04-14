# Phase: Plan Feature

You are planning the implementation of a specific feature. Your goal is to produce a detailed feature document, generate acceptance tests that serve as an immutable contract, and update project rules if needed.

## Inputs

- Feature description from the user (provided as context or arguments)
- If the user references a feature by ID/name from `docs/prd.md`, look it up there

## Step 1: Understand Context

Read these files to understand the current project state:

1. `docs/prd.md` -- project overview, architecture, NFR, feature list
2. `.agents/rules/` -- relevant rule files for the feature's domain
3. Existing `docs/features/*.md` -- understand what has been built already
4. Existing `tests/` -- understand current test coverage and patterns

## Step 2: Create Feature Document

Read the template at `.agents/templates/feature.md`. Create `docs/features/FEATURE-ID.md` following that template.

Use a short, descriptive kebab-case ID for the feature (e.g., `user-auth`, `api-gateway`, `payment-processing`).

The feature document must contain:

### Specification section
- Clear description of what the feature does
- Acceptance criteria (specific, testable conditions)
- Edge cases and error scenarios
- Dependencies on other features or external services
- API contracts or interface definitions where applicable

### Implementation Plan section
- Ordered task breakdown (what to implement first, second, etc.)
- Files to create or modify
- Key architectural decisions specific to this feature
- Any new patterns or libraries introduced

### Acceptance Test Manifest
- Explicit list of `*.acceptance.test.*` files that form the contract
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

- [ ] `docs/features/FEATURE-ID.md` exists with both spec and implementation plan sections
- [ ] Acceptance test manifest is listed in the feature doc
- [ ] `tests/features/FEATURE-ID/*.acceptance.test.*` files exist and are runnable (expected to fail)
- [ ] `docs/prd.md` feature table is updated
- [ ] Rules updated if new patterns were introduced

Report to the user:
- Summary of the feature specification
- List of acceptance tests created
- Recommended order of implementation tasks
- Any questions or ambiguities that need resolution before execution
