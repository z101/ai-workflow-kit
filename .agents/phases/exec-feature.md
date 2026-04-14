# Phase: Execute Feature

You are implementing a feature that has already been planned. Your goal is to write code that passes all acceptance tests without modifying them.

## Inputs

- Feature ID or reference to the feature document

## Step 1: Load Context

Read these files:

1. `docs/features/FEATURE-ID.md` -- the feature specification and implementation plan
2. All files in `tests/features/FEATURE-ID/` -- the acceptance tests (your contract)
3. `.agents/rules/` -- relevant rule files for the feature's domain

You do NOT need to read the full `docs/prd.md` unless the feature doc references it for cross-cutting context.

## Step 2: Verify Acceptance Tests

Before writing any implementation code:

1. Confirm the acceptance test files exist in `tests/features/FEATURE-ID/`
2. Run the acceptance tests -- they should FAIL (no implementation yet)
3. If acceptance tests don't exist or can't be found, STOP and ask the user to run plan-feature first

## Step 3: Implement

Follow the implementation plan from the feature document. Work through tasks in the specified order.

### Implementation rules

- Follow conventions from `.agents/rules/` and `AGENTS.md`
- Run acceptance tests after each significant change to track progress
- You MAY create additional `*.test.*` files (without `.acceptance.` in the name) for unit testing implementation details
- You MUST NOT create, modify, or delete any `*.acceptance.test.*` file

### If an acceptance test seems wrong

If you believe an acceptance test is incorrect, has a bug, or tests something contradictory to the spec:

1. **STOP implementation**
2. Explain to the user which test seems wrong and why
3. Ask the user to return to the plan-feature phase to fix the test
4. Do NOT work around the test or modify it

### If you get stuck

If you cannot make a test pass after reasonable effort:

1. Explain what you've tried and what's blocking you
2. Ask the user for guidance
3. Do NOT skip the test or mark it as pending/skipped

## Step 4: Verify All Tests Pass

When you believe the implementation is complete:

1. Run ALL tests in `tests/features/FEATURE-ID/` (both acceptance and implementation tests)
2. If any test fails, fix the implementation (not the tests)
3. Run all project-wide tests to check for regressions in other features

## Step 5: Report for Verification

Present to the user:

- Summary of what was implemented
- List of files created or modified
- All test results (acceptance + implementation)
- Any deviations from the implementation plan and why
- Any concerns or technical debt introduced

The user will verify the implementation before proceeding to embed-feature.

## Constraints

- `*.acceptance.test.*` files are READ-ONLY. Never modify or delete them.
- Do not modify files outside the feature's scope without explicit user approval.
- Do not modify other feature's test files.
- Do not update `docs/prd.md` or `docs/features/FEATURE-ID.md` -- that happens in embed-feature.
- Do not update `.agents/rules/` or `AGENTS.md` -- that happens in embed-feature.
