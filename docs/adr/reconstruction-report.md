# ADR reconstruction report

## Purpose

Explain how the [ADR set](index.md) was produced, what changed, and what remains open.

## Method

1. Read all earlier internal decision records (H-000 to H-041) and classified each one ([Historical decision records](history.md)).
2. Checked the implementation in the Flowdigger repositories: `gradle-lineage-plugin`, `gradle-xml-plugin`, `gradle-archi-plugin`, `gradle-ooxml-plugin`, `gradle-antlr-*`, `xir-sax`, `xir-g4`, `example-lineage-plsql`, `github-workflows`.
3. Checked the terminology and phase models on this site.
4. Wrote one ADR per decision that the implementation realizes. A decision that is only partly realized is *Proposed*.
5. Linked each ADR to evidence and to the historical records it replaces.

No dates, motives or evaluated alternatives were added without evidence. Present-day alternatives that were not evaluated are marked as such.

## Records inspected

42 historical records (H-000 to H-041). Classification summary:

| Classification | Count |
|---|---|
| Surviving | 10 |
| Refined | 9 |
| Superseded | 9 (including H-030 to H-033) |
| Out of scope | 9 |
| Defective | 2 |
| Not realized / input only | 3 |

Details per record: [Historical decision records](history.md).

## ADRs created

| ADR | Status | Main historical sources |
|---|---|---|
| [0001](0001-prefer-established-standards.md) Established standards | Accepted | H-005, H-008 |
| [0002](0002-common-structural-representation.md) Common structure | Accepted | H-029 |
| [0003](0003-functional-transformations.md) Functional transformations | Accepted | H-000, H-005 |
| [0004](0004-shared-xslt-substrate.md) Shared XSLT substrate | Accepted | H-008 |
| [0005](0005-validate-at-transformation-boundaries.md) Validate at boundaries | Accepted | H-008, H-012, H-016, H-022, H-039 |
| [0006](0006-knowledge-reconstruction-as-build.md) Reconstruction as build | Accepted | H-003, H-005 |
| [0007](0007-explicit-pipeline-phases.md) Nine phases | Accepted | H-011, H-024, H-038 |
| [0008](0008-derive-from-evidence-without-inference.md) Evidence only | Accepted | H-000, H-018, H-020, H-023, H-025, H-035, H-037 |
| [0009](0009-replaceable-source-adapters.md) Replaceable adapters | Accepted | — (implementation only) |
| [0010](0010-architecture-models-as-evidence.md) Models as evidence | Accepted | H-007, H-009, H-013, H-027, H-034, H-035, H-037, H-038 |
| [0011](0011-preserve-provenance.md) Provenance | Proposed | H-000, H-020 |
| [0012](0012-separate-canonical-knowledge-from-projections.md) Canonical vs projections | Accepted | H-036, H-040 |
| [0013](0013-saxon-he-baseline.md) Saxon-HE baseline | Accepted | H-005 |

## Contradictions and resolutions

| Contradiction | Resolution |
|---|---|
| Architecture model as single source of truth (H-009, H-013, H-029) vs near-reality and derive-versus-model (H-035, H-037) | Later position adopted; matches implementation. [ADR-0010](0010-architecture-models-as-evidence.md) |
| Reconstruction from code is unreliable (H-000, H-018, H-020) vs parser-based derivation in Flowdigger | Implementation wins for what implementation reveals; declarations stay for the rest. [ADR-0008](0008-derive-from-evidence-without-inference.md) |
| "Never infer" (H-025) vs "if it can be inferred, derive it" (H-037) | Terminology fixed: *derive* = deterministic rule over evidence; *inference* = heuristic guess. [ADR-0008](0008-derive-from-evidence-without-inference.md) |
| H-015 duplicates H-013; H-016 title does not match content | Marked defective; H-016 content used in [ADR-0005](0005-validate-at-transformation-boundaries.md). |
| Site pages show 4- or 5-step lifecycles; implementation has nine phases | Short lifecycles kept as simplified views and mapped to the nine phases in [Derivation lifecycle](../core-concepts/derivation-lifecycle.md). |

## Documentation changed

- New section `docs/adr/` with index, 13 ADRs, template, history and this report.
- `mkdocs.yml`: new navigation section *Architecture Decisions*.
- [Derivation lifecycle](../core-concepts/derivation-lifecycle.md): mapping of the five lifecycle steps to the nine pipeline phases.
- [Lineage architecture map](../lineage/index.md), [Evidence model](../core-concepts/evidence-model.md), [Validate lineage quality](../how-to/validate-lineage-quality.md), [Developer path](../role-paths/developer.md), [Reading tracks](../reading-tracks.md), [Home](../index.md): links to the relevant ADRs.
- [Glossary](../reference/glossary.md): terms for observation, direct flow, merge, stitch, projection, processing language, vocabulary, and ADR.

## Implemented vs proposed

- **Implemented:** ADR-0001 to ADR-0010, ADR-0012, ADR-0013.
- **Partly implemented (Proposed):** ADR-0011 provenance. Line references exist up to merge; end-to-end provenance in stitched lineage and a PROV-O mapping do not exist.
- **Known gaps inside accepted ADRs:**
    - `generate-id()` in the RDF projection (ADR-0003, ADR-0012).
    - Not all tasks are cacheable; configuration-cache limitation with OOXML integration (ADR-0006).
    - No dedicated drift report task (ADR-0010).
    - No built-in SAS lineage profile (ADR-0009).
    - Commercial catalog integration is planned only (ADR-0012).

## Open questions

- Should source-specific namespaces in parse output become a documented convention (ADR-0002)?
- Which identifier scheme replaces `generate-id()` in projections?
- Should provenance use PROV-O as a projection, and which terms map?
- Should drift detection between architecture model and derived results become a pipeline phase or a separate task?
- Repository README files do not yet link to these ADRs. This is a follow-up.
