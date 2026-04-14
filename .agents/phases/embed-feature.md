# Phase: Embed Feature

You are integrating a user-verified feature implementation into the project. Your goal is to update documentation to reflect what was actually built, create cross-feature tests, and ensure everything works together.

## Inputs

- Feature ID or reference to the feature that was just implemented and verified by the user

## Prerequisites

Before proceeding, confirm with the user:
- The exec-feature phase is complete
- All acceptance tests pass
- The user has verified the implementation

## Step 1: Load Context

Read these files:

1. `docs/prd.md` -- current project state
2. `docs/features/FEATURE-ID.md` -- the original specification and plan
3. The implemented source code for this feature
4. Existing `tests/integration/` and `tests/e2e/` tests

## Step 2: Update Feature Document

Update `docs/features/FEATURE-ID.md` to reflect what was actually built:

- Note any deviations from the original plan and the reasons
- Document the final API surface, data models, or interfaces
- Record key architectural decisions made during implementation
- Update the status to "integrated"

The feature doc becomes the permanent record of this feature -- both what was planned and what was delivered.

## Step 3: Update PRD

Update `docs/prd.md`:

- Mark the feature as "integrated" in the feature table
- Adjust the Architecture section if the implementation changed the high-level architecture
- Do NOT duplicate feature details -- the feature doc is the source of truth

## Step 4: Create/Update Integration Tests

Identify how the new feature interacts with existing features and create appropriate tests:

### Cross-feature integration tests (`tests/integration/`)
- Test interactions between the new feature and previously integrated features
- Focus on data flow, shared state, and API contracts between features
- Name tests descriptively (e.g., `auth-api-gateway.test.*`)

### E2E tests (`tests/e2e/`)
- If the feature affects user-facing journeys, create or update E2E tests
- Cover the happy path and critical error paths that span multiple features

If there are no meaningful cross-feature interactions yet (e.g., this is the first feature), skip this step and note it.

## Step 5: Run All Tests

Run the complete test suite:

1. All acceptance tests (`*.acceptance.test.*`) across all features
2. All implementation tests (`*.test.*`) across all features
3. All integration tests (`tests/integration/`)
4. All E2E tests (`tests/e2e/`)

If any test fails:
- Fix the code (not the tests) for implementation and integration issues
- If an acceptance test from a DIFFERENT feature fails, investigate carefully -- this indicates a regression
- Report any regressions to the user before fixing

## Step 6: Update Rules (if needed)

If the implementation revealed new patterns, conventions, or best practices:

- Update the relevant `.agents/rules/*.md` files
- Update `AGENTS.md` if new rule files were created
- Update `opencode.json` instructions if new rule files were created

## Step 7: Summarize

Present to the user:

- What documentation was updated
- Integration and E2E tests created
- Full test suite results
- Any new rules or conventions added
- Suggestions for the next feature to implement (based on dependencies in `docs/prd.md`)

## Output Checklist

Before finishing, verify:

- [ ] `docs/features/FEATURE-ID.md` updated with implementation reality
- [ ] `docs/prd.md` feature table updated (status: "integrated")
- [ ] Integration tests created for cross-feature interactions (if applicable)
- [ ] Full test suite passes
- [ ] Rules updated if new patterns emerged
