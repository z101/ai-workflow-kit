# Feature: [Feature Name]

**ID**: [FEATURE-ID]
**Status**: planned | in progress | integrated
**Priority**: [high | medium | low]
**Dependencies**: [list of feature IDs this depends on, or "none"]

## Specification

### Description

[What this feature does from the user's perspective. 2-3 paragraphs.]

### Acceptance Criteria

- [ ] [Specific, testable condition]
- [ ] [Specific, testable condition]
- ...

### Edge Cases

- [Edge case and expected behavior]
- ...

### Error Scenarios

- [Error condition and expected handling]
- ...

### API / Interface Contract

[If applicable: endpoints, function signatures, data models, event contracts. Use code blocks for clarity.]

## Implementation Plan

### Task Breakdown

1. [First task -- what to implement and why this order]
2. [Second task]
3. ...

### Files to Create/Modify

| File | Action | Purpose |
|------|--------|---------|
| | create / modify | |

### Architectural Decisions

- [Decision and rationale, specific to this feature]

## Acceptance Test Manifest

These files form the immutable contract for exec-feature. They MUST NOT be modified during implementation.

| Test File | Covers |
|-----------|--------|
| `tests/features/FEATURE-ID/[name].acceptance.test.*` | [what it tests] |

## Implementation Notes

_This section is updated during embed-feature to reflect what was actually built._

### Deviations from Plan

- [What changed and why]

### Final API Surface

[Actual endpoints, interfaces, or contracts as implemented]

### Decisions Made During Implementation

- [Decision and context]
