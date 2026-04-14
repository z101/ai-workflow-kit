# Phase: Execute Feature

You are implementing a feature that has already been planned. Your goal is to write code that passes all acceptance tests without modifying them.

## Inputs

- **Feature ID** (required) — the kebab-case identifier (e.g., `user-auth`) that resolves to:
  - `docs/features/FEATURE-ID.md` — the feature specification and implementation plan
  - `tests/features/FEATURE-ID/` — the acceptance tests
- **Additional instructions** (optional) — the user may provide extra context after the Feature ID, for example:
  - Starting point: "start from task 3, data layer is done"
  - Overrides: "use v2 API instead of what the plan says"
  - Constraints: "don't touch the database schema"
  - These instructions take precedence over the implementation plan where they conflict. Note any such overrides in the report as deviations.
- If no Feature ID is provided, list available feature documents from `docs/features/` and ask the user which one to execute
- If the provided ID doesn't match any existing feature document, show available options and ask for clarification

## Step 1: Load Context

Read these files:

1. `docs/features/FEATURE-ID.md` -- the feature specification and implementation plan
2. All files in `tests/features/FEATURE-ID/` -- the acceptance tests (your contract)
3. `.agents/rules/` -- relevant rule files for the feature's domain
4. **Context References** from the feature doc -- read all files listed under "Mandatory Reading", review "Patterns to Follow" and "Anti-Patterns to Avoid" before writing any code

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
- After each file change, verify syntax, imports, and types are correct before moving to the next task
- Run acceptance tests after each significant change to track progress
- If you encounter issues not addressed in the plan, document them for the report rather than silently working around them
- You MAY create additional `*.test.`* files (without `.acceptance.` in the name) for unit testing implementation details
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

## Step 5: Code Review

After all tests pass, review the implementation before presenting it to the user. Check for issues that tests alone don't catch:

- **Plan conformance**: verify the feature doc's specification and implementation plan were followed correctly, no requirements were missed or misinterpreted
- **Bugs and logic errors**: look for obvious bugs, off-by-one errors, unhandled edge cases, missing error handling
- **Data alignment**: check for mismatches between layers — snake_case vs camelCase, flat vs nested structures, inconsistent field names between API contracts, database schemas, and internal models
- **Over-engineering**: identify unnecessary abstractions, premature generalizations, or files growing too large and needing extraction
- **Style consistency**: flag syntax or patterns that don't match the rest of the codebase or the conventions in `.agents/rules/`

Fix any issues found, then re-run the full test suite to confirm nothing broke.

## Step 6: Report for Verification

Present to the user:

- Summary of what was implemented
- List of files created or modified
- All test results (acceptance + implementation)
- Any deviations from the implementation plan and why
- Any concerns or technical debt introduced

The user will verify the implementation before proceeding to embed-feature.

## Constraints

- `*.acceptance.test.`* files are READ-ONLY. Never modify or delete them.
- Do not modify files outside the feature's scope without explicit user approval.
- Do not modify other feature's test files.
- Do not update `docs/prd.md` or `docs/features/FEATURE-ID.md` -- that happens in embed-feature.
- Do not update `.agents/rules/` or `AGENTS.md` -- that happens in embed-feature.