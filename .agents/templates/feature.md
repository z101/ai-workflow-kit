# Feature: [Feature Name]

**ID**: [FEATURE-ID]
**Type**: [New Capability | Enhancement | Refactor | Bug Fix]
**Complexity**: [Low | Medium | High]
**Status**: planned | in progress | integrated
**Priority**: [high | medium | low]
**Dependencies**: [list of feature IDs this depends on, or "none"]

## Specification

### Description

[What this feature does from the user's perspective. 2-3 paragraphs.]

### Acceptance Criteria

- [Specific, testable condition]
- [Specific, testable condition]
- ...

### Edge Cases

- [Edge case and expected behavior]
- ...

### Error Scenarios

- [Error condition and expected handling]
- ...

### API / Interface Contract

[If applicable: endpoints, function signatures, data models, event contracts. Use code blocks for clarity.]

## Context References

### Mandatory Reading

*Files the exec agent must read before implementing. Include line ranges where relevant.*

- `path/to/file` (lines X-Y) -- Why: [relevance to this feature]
- ...

### Relevant Documentation

- [Library/framework docs](URL#section) -- Why: [what this covers]
- ...

### Patterns to Follow

*Codebase patterns this feature should mirror.*

- [Pattern name] -- see `path/to/file` (lines X-Y)
- ...

### Anti-Patterns to Avoid

- [What to avoid and why]
- ...

## Implementation Plan

### Task Breakdown

Use action keywords: **CREATE**, **UPDATE**, **ADD**, **REFACTOR**.

1. [ACTION] [First task -- what to implement and why this order]
2. [ACTION] [Second task]
3. ...

### Files to Create/Modify


| File | Action          | Purpose |
| ---- | --------------- | ------- |
|      | CREATE / UPDATE |         |


### Architectural Decisions

- [Decision and rationale, specific to this feature]

## Acceptance Test Manifest

These files form the immutable contract for exec-feature. They MUST NOT be modified during implementation.


| Test File                                            | Covers          |
| ---------------------------------------------------- | --------------- |
| `tests/features/FEATURE-ID/[name].acceptance.test.`* | [what it tests] |


## Implementation Notes

*This section is updated during embed-feature to reflect what was actually built.*

### Deviations from Plan

- [What changed and why]

### Final API Surface

[Actual endpoints, interfaces, or contracts as implemented]

### Decisions Made During Implementation

- [Decision and context]