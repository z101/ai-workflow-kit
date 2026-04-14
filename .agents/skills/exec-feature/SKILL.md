---
name: exec-feature
description: >-
  Implement a planned feature by writing code that passes all acceptance
  tests. Works iteratively until all tests pass. Acceptance tests are
  immutable and must not be modified.
  Use when implementing a feature that has been planned with plan-feature.
disable-model-invocation: true
---

# Phase: Execute Feature

The user will provide a Feature ID (e.g., `user-auth`) identifying which feature to implement.

Read and follow the instructions in `.agents/phases/exec-feature.md`.

Read the feature document from `docs/features/FEATURE-ID.md` for the specification and implementation plan.

Acceptance tests in `tests/features/FEATURE-ID/*.acceptance.test.*` are READ-ONLY. Never modify or delete them.
