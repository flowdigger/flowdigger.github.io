# Validate Lineage Quality

!!! warning "Under construction"
    Examples are not yet fully runnable. These are review criteria, not verified execution instructions.

## Purpose

Check if lineage output is complete, consistent, and actionable.

## Audience

Developers, analysts, and managers.

## Quality checks

1. **Coverage**: key source and target elements appear in lineage graph.
2. **Traceability**: check available source evidence and record missing links. End-to-end provenance is not implemented ([ADR-0011](../adr/0011-preserve-provenance.md)).
3. **Consistency**: repeated derivation run gives same result.
4. **Reviewability**: business user can follow path without hidden jumps.

The pipeline runs Schematron gates after direct-flow derivation and after stitching. See [ADR-0005](../adr/0005-validate-at-transformation-boundaries.md).

## Lightweight acceptance checklist

- All critical fields mapped
- All critical transformations documented
- Evidence links available
- Known gaps listed with owners

## Output

A review checklist and explicit gap list; results depend on available evidence and implementation coverage.

