# Validate Lineage Quality

## Purpose

Check if lineage output is complete, consistent, and actionable.

## Audience

Developers, analysts, and managers.

## Quality checks

1. **Coverage**: key source and target elements appear in lineage graph.
2. **Traceability**: each derived fact links to source evidence.
3. **Consistency**: repeated derivation run gives same result.
4. **Reviewability**: business user can follow path without hidden jumps.

## Lightweight acceptance checklist

- All critical fields mapped
- All critical transformations documented
- Evidence links available
- Known gaps listed with owners

## Output

Clear pass/fail result with explicit gap list.

