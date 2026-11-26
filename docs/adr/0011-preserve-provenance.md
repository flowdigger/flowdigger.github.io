# ADR-0011: Preserve provenance from source evidence to derived knowledge

- **Status:** Proposed
- **Nature:** Retrospective architecture baseline (partly realized)

## Context

A reviewer who sees a lineage relation must be able to ask *why does this relation exist?* and get an answer: which source file and location, which observation, and which rule. Without that link, lineage is an assertion, not evidence.

## Decision

Carry provenance through every phase: each derived fact keeps a reference to the evidence it came from (source artifact and location) and to the rule or phase that produced it. Provenance is part of the canonical result, not a separate log.

## Rationale

- Provenance makes each relation reviewable and auditable ([ADR-0008](0008-derive-from-evidence-without-inference.md)).
- It lets people fix the cause of a wrong relation (source, declaration or rule) instead of patching the result.
- It lets consumer projections expose evidence links where their format allows it ([ADR-0012](0012-separate-canonical-knowledge-from-projections.md)).

## Current realization

Partly realized:

- Parse output keeps token line numbers; observe stylesheets copy them to observations (for example `line` on procedures and views).
- Direct-flow and merge stylesheets carry attributes such as `source`, `line`, `source-schema`, `source-table`, `source-set` and `source-resolution`.
- Stitched lineage does not yet carry file and line references for each relation end to end.
- No mapping to the W3C PROV-O vocabulary exists. Conceptual papers position XIR as complementary to PROV-O; that is not a compatibility claim.

## Consequences

- Positive when complete: every relation can be explained; audits can sample evidence.
- Negative: larger outputs; every rule must pass provenance on.
- Until complete, documentation must state that provenance is preserved up to merge and is partial after stitch.

## Alternatives and earlier approaches

- **Separate provenance log.** Easier to add, but easy to lose when results are copied or projected.
- **PROV-O as the internal model.** Possible later as a projection; not evidenced now.

## Evidence

- [T-SQL observe stylesheet](https://github.com/flowdigger/gradle-lineage-plugin/blob/main/pipeline/2.observe/src/main/xsl/tsql/extract.xsl): `line` attributes.
- [Direct-flow stylesheets](https://github.com/flowdigger/gradle-lineage-plugin/tree/main/pipeline/3.direct-flow/src/main/xsl)
- [stitch-lineage.xsl](https://github.com/flowdigger/gradle-lineage-plugin/blob/main/pipeline/6.stitch/src/main/xsl/stitch-lineage.xsl)
- Documentation: [Evidence model](../core-concepts/evidence-model.md), [XIR W3C positioning note](../XIR_W3C_Positioning_Note.md), [Glossary](../reference/glossary.md).

## Related decisions

- Reader introduction: [Provenance](../learn/system-knowledge/provenance.md).
- [ADR-0002](0002-common-structural-representation.md), [ADR-0008](0008-derive-from-evidence-without-inference.md), [ADR-0012](0012-separate-canonical-knowledge-from-projections.md)
- Historical: H-000, H-020 (see [Historical decision records](history.md))
