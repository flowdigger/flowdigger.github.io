# ADR-0012: Separate canonical knowledge from consumer projections

- **Status:** Accepted
- **Nature:** Retrospective architecture baseline

## Context

Lineage results have many consumers: graph viewers, RDF stores, JSON-based tools, catalogs and knowledge graphs. Each consumer wants its own format and its own simplifications. If one consumer format becomes the primary result, the needs of that consumer shape the knowledge, and other consumers receive lossy conversions of a conversion.

## Decision

Keep one canonical result per pipeline run (stitched lineage in Flowdigger XML) and produce every consumer format as a projection from it. Projections can simplify and rename; they must not add relations that the canonical result does not contain. Lineage itself is a projection of derived knowledge, not hand-maintained data.

## Rationale

- New consumers need a new projection, not a change to derivation.
- Validation runs once on the canonical result ([ADR-0005](0005-validate-at-transformation-boundaries.md)).
- Projections are transformations like any other and follow [ADR-0003](0003-functional-transformations.md).

## Current realization

- Phase 6 writes `stitched-lineage.xml` and application artifacts; phase 7 validates them.
- Phase 8 projects them with `stitched-to-graphml.xsl`, `stitched-to-rdf.xsl`, `stitched-to-json.xsl`, `app-to-graphml.xsl` and `app-to-rdf.xsl`.
- Phase 9 packages the projections for distribution.
- `example-lineage-plsql/4-knowledge-graph` loads lineage into Neo4j and emulates a subset of a catalog ingestion API. Integration with a commercial catalog is planned only.

## Consequences

- Positive: one place to validate; consistent projections; consumer formats can change independently.
- Negative: each projection must be maintained and tested.
- Gap: the RDF projection uses `generate-id()` for identifiers; see [ADR-0003](0003-functional-transformations.md).

## Alternatives and earlier approaches

- **One consumer format as the primary result** (for example a graph database or catalog). Not adopted.
- Earlier internal records described a canonical enterprise graph with views as projections, and lineage as a derived projection. This ADR keeps that principle for Flowdigger's lineage result.

## Evidence

- [Export stylesheets](https://github.com/flowdigger/gradle-lineage-plugin/tree/main/pipeline/8.export/src/main/xsl)
- [NARRATIVE-RUNTIME-CONTRACT.md](https://github.com/flowdigger/gradle-lineage-plugin/blob/main/NARRATIVE-RUNTIME-CONTRACT.md), section 1.
- [example-lineage-plsql 4-knowledge-graph](https://github.com/flowdigger/example-lineage-plsql/tree/main/4-knowledge-graph)
- Documentation: [RDF, SKOS and Collibra mapping](../RDF_SKOS_Collibra_Mapping.md).

## Related decisions

- [ADR-0003](0003-functional-transformations.md), [ADR-0007](0007-explicit-pipeline-phases.md), [ADR-0010](0010-architecture-models-as-evidence.md), [ADR-0011](0011-preserve-provenance.md)
- Historical: H-009, H-036, H-040 (see [Historical decision records](history.md))
