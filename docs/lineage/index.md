# Lineage Architecture Map

## Purpose

Show how Flowdigger derives lineage from real system evidence.

## Audience

Developers, business analysts, managers, and learners.

## Core idea

Do not maintain lineage by hand when system facts can be derived from code, configuration, and runtime signals.

## End-to-end flow

```mermaid
flowchart TD
    A[Collect evidence from legacy system] --> B[Normalize to intermediate representation]
    B --> C[Derive lineage facts]
    C --> D[Validate quality and completeness]
    D --> E[Publish views for teams and governance]
```

## Use this section

- Need architecture detail: read [Universal Semantic Lineage Architecture](../Universal_Semantic_Lineage_Architecture.md).
- Need reconciliation detail: read [Continuous Architecture Reconciliation](../continuous-architecture-reconcilliation.md).
- Need lifecycle detail: read [Derivation lifecycle](../core-concepts/derivation-lifecycle.md).

## Related

- [Migration map](migration-map.md)
- [Evidence model](../core-concepts/evidence-model.md)
