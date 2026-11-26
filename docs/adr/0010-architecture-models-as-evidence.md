# ADR-0010: Use architecture models as binding evidence for intent, not as the sole source of truth

- **Status:** Accepted
- **Nature:** Retrospective architecture baseline

## Context

Architecture models describe what implementation alone does not reveal: ownership, business meaning, application boundaries and intended dependencies. Implementation describes what really runs: tables, procedures, calls and data movement. Both drift apart over time.

If the model is treated as the only truth, derived facts that disagree with it are discarded and the model slowly becomes fiction. If the model is ignored, lineage loses the intent and boundaries that people need to interpret it.

## Decision

Treat the architecture model as binding evidence for what only people can declare (ownership, semantics, intent, cross-component connections), and derive from implementation what implementation reveals. Neither source is the sole truth. Disagreement between them is a reconciliation finding, not something to resolve silently.

Rule of thumb: *derive what implementation reveals; model what only people know.*

## Rationale

- Each kind of knowledge comes from the source that can best provide it.
- The model stays close to reality, because derived facts show where it drifts.
- Explicit declarations in the model give stitching the evidence it needs to connect components without guessing ([ADR-0008](0008-derive-from-evidence-without-inference.md)).
- The pipeline produces evidence; people and governance processes decide on the findings.

## Current realization

- `gradle-archi-plugin` runs Archi headless as a Gradle task and exports model content for pipeline use.
- `example-lineage-plsql/1-architecture` contains an ArchiMate model (`test-lineage.archimate`), `archi2model.xsl`, and Schematron checks.
- Phase 6 uses `archi2model.xsl` and architecture or configuration declarations to stitch across components; unresolved endpoints stay visible.
- Drift detection between model and derived results is described in [Continuous Architecture Reconciliation](../continuous-architecture-reconcilliation.md). A dedicated drift report task is not yet evidenced in the implementation.

## Consequences

- Positive: models keep their value for intent; implementation facts keep their authority for what runs; drift becomes visible.
- Negative: two sources must be maintained and compared; reconciliation needs an owner.
- Documentation must not describe the architecture model as the single source of truth.

## Alternatives and earlier approaches

- **Model as single source of truth; derived artifacts disposable.** Earlier internal records took this position. Later internal records replaced it with a near-reality principle and a derive-versus-model rule. This ADR follows the later position, which matches the implementation.
- **Implementation only.** Loses ownership, meaning and boundaries.
- **Structured metadata embedded in the model for all lineage.** Earlier internal records proposed this. It is not used: implementation-level lineage is derived, not declared in the model.

## Evidence

- [gradle-archi-plugin ARCHITECTURE.md](https://github.com/flowdigger/gradle-archi-plugin/blob/main/ARCHITECTURE.md)
- [example-lineage-plsql 1-architecture](https://github.com/flowdigger/example-lineage-plsql/tree/main/1-architecture)
- [NARRATIVE-RUNTIME-CONTRACT.md](https://github.com/flowdigger/gradle-lineage-plugin/blob/main/NARRATIVE-RUNTIME-CONTRACT.md): declaration-based stitching.
- Documentation: [ArchiMate as the architectural bootstrap](../flowdigger-deterministic-knowledge-derivation.md#9-archimate-as-the-architectural-bootstrap), [Continuous Architecture Reconciliation](../continuous-architecture-reconcilliation.md).

## Related decisions

- [ADR-0007](0007-explicit-pipeline-phases.md), [ADR-0008](0008-derive-from-evidence-without-inference.md), [ADR-0012](0012-separate-canonical-knowledge-from-projections.md)
- Historical: H-007, H-009, H-013, H-023, H-027, H-034, H-035, H-037, H-038 (see [Historical decision records](history.md))
