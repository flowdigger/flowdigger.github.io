# ADR-0007: Separate the lineage lifecycle into nine single-responsibility phases

- **Status:** Accepted
- **Nature:** Retrospective architecture baseline

## Context

A lineage pipeline that parses, interprets, connects, validates and exports in one step is hard to test and hard to extend. Different source technologies need different parsing and observation, but connecting and exporting results should work the same way for all sources.

Flowdigger's phases were not designed up front. They became visible when reusable machinery was extracted from a working lineage pipeline.

## Decision

Organize lineage derivation in nine phases. Each phase has one responsibility, a fixed output location, and a contract that the next phase can rely on. Phases 1–5 are source-specific and selected by profile; phases 6–9 are uniform for all sources.

| # | Phase | Task | Responsibility |
|---|---|---|---|
| 1 | parse | `parseLineageSources` | convert source text into structured syntax |
| 2 | observe | `extractLineageObservations` | extract facts from syntax, configuration, documents and models |
| 3 | direct flow | `deriveDirectFlows` | derive local source → operation → target flows |
| 4 | validate direct flow | `validateDirectFlows` | check local flow contracts |
| 5 | merge | `mergeDirectFlows` | combine validated local results and build indexes |
| 6 | stitch | `stitchLineage` | connect local flows across components into application lineage |
| 7 | validate lineage | `validateLineage` | check the integrated result |
| 8 | export | `exportLineageArtifacts` | create consumer representations |
| 9 | publish | `publishLineage` | package and distribute artifacts |

## Rationale

- One responsibility per phase makes each phase testable with fixtures and replaceable per profile.
- Validation phases sit directly after the phases whose contracts they check ([ADR-0005](0005-validate-at-transformation-boundaries.md)).
- **Merge is not stitch.** Merge combines results of the same kind and builds indexes; it adds no new relations. Stitch adds cross-component relations, and only from explicit declarations ([ADR-0008](0008-derive-from-evidence-without-inference.md)). Keeping them apart keeps new relations reviewable.
- Export is separate from stitch so that consumer formats never change canonical results ([ADR-0012](0012-separate-canonical-knowledge-from-projections.md)).

## Current realization

- Phases live under `gradle-lineage-plugin/pipeline/<n>.<phase>` with a registrar class per phase and outputs under `<buildRoot>/<n>.<phase>/`.
- Profiles `java`, `plsql`, `tsql`, `config` and custom profiles configure phases 1–5; phases 6–9 have separate override points.
- Observe output is organized by kind (`sql`, `java`, `configuration`, `ooxml`).
- Merge writes `direct-flows.merged.xml` and SQL indexes; stitch writes `stitched-lineage.xml` and application artifacts.

## Consequences

- Positive: clear contracts; source-specific work is isolated in phases 1–5; new sources do not change phases 6–9.
- Negative: more intermediate files and tasks; phase contracts must be documented and kept stable.
- Older documentation pages describe shorter lifecycles (for example Extract → Normalize → Derive → Validate → Publish). They are simplified views of these nine phases, mapped in [Derivation lifecycle](../core-concepts/derivation-lifecycle.md).

## Alternatives and earlier approaches

- **One monolithic transformation.** Simpler at first; not testable per responsibility.
- **Fewer, broader phases** (for example a combined resolve step). Used in earlier conceptual papers; the implementation separates merge, stitch and their validation.
- Earlier internal records described a reconciliation loop (capture intent, derive, merge, validate, publish, detect drift). The nine phases realize the derive-to-publish part of that loop.

## Evidence

- [NARRATIVE-RUNTIME-CONTRACT.md](https://github.com/flowdigger/gradle-lineage-plugin/blob/main/NARRATIVE-RUNTIME-CONTRACT.md), section 1 and section 3.
- [Pipeline directory](https://github.com/flowdigger/gradle-lineage-plugin/tree/main/pipeline)
- [gradle-lineage-plugin README](https://github.com/flowdigger/gradle-lineage-plugin/blob/main/README.md): profiles and stage overrides.
- Documentation: [Flowdigger deterministic knowledge derivation](../flowdigger-deterministic-knowledge-derivation.md), section 2.

## Related decisions

- [ADR-0003](0003-functional-transformations.md), [ADR-0005](0005-validate-at-transformation-boundaries.md), [ADR-0008](0008-derive-from-evidence-without-inference.md), [ADR-0009](0009-replaceable-source-adapters.md), [ADR-0012](0012-separate-canonical-knowledge-from-projections.md)
- Historical: H-011, H-024, H-038 (see [Historical decision records](history.md))
