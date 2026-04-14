---
name: plan-feature
description: >-
  Plan the implementation of a specific feature. Generates a detailed
  feature document with specification and implementation plan, creates
  immutable acceptance tests as a contract for execution.
  Use when planning a new feature before implementation.
disable-model-invocation: true
---

# Phase: Plan Feature

Read and follow the instructions in `.agents/phases/plan-feature.md`.

Use `.agents/templates/feature.md` as the document structure template for generating `docs/features/FEATURE-ID.md`.

Acceptance tests go in `tests/features/FEATURE-ID/` with the `*.acceptance.test.*` naming convention.
