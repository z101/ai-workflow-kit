---
name: embed-feature
description: >-
  Integrate a verified feature implementation into the project. Updates
  documentation to reflect what was built, creates cross-feature integration
  tests, runs full test suite, and updates project rules.
  Use after a feature passes user verification in exec-feature.
disable-model-invocation: true
---

# Phase: Embed Feature

Read and follow the instructions in `.agents/phases/embed-feature.md`.

Update `docs/features/FEATURE-ID.md` with implementation reality, then update `docs/prd.md` index.

Create integration tests in `tests/integration/` and E2E tests in `tests/e2e/` as needed.
