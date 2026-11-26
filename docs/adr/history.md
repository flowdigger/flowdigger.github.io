# Historical decision records

## Purpose

Summarize the earlier decision records that preceded Flowdigger and show where each one went in the current [ADR set](index.md).

## Background

Flowdigger was extracted from an internal lineage pipeline. That pipeline had its own decision records (here numbered H-000 to H-041). The records themselves are not public. This page gives a neutral summary of each record and its classification, so that readers can see which ideas survive and which ideas were replaced.

Most early records (H-000 to H-034) were drafts without a formal acceptance. A later group (H-035 to H-040) was accepted as the active canon of the internal project. Dates and owners are not reproduced here.

## Classifications

- **Surviving**: the decision still holds and is realized in Flowdigger.
- **Refined**: the core idea holds, in a narrower or changed form.
- **Superseded**: replaced by a later record or contradicted by the implementation.
- **Out of scope**: internal, organisational or tool-specific; not relevant to Flowdigger as open source.
- **Defective**: the record has a structural problem (for example duplicate content or a title that does not match the content).

## Record map

| ID | Topic (neutral summary) | Classification | Current ADR |
|---|---|---|---|
| H-000 | Architecture drivers: completeness, determinism, explicitness, auditability | Refined | [0003](0003-functional-transformations.md), [0008](0008-derive-from-evidence-without-inference.md) |
| H-001 | Repository layout | Out of scope | — |
| H-002 | CI pipeline templates | Out of scope | — |
| H-003 | Java and Gradle with wrapper as build platform | Surviving | [0006](0006-knowledge-reconstruction-as-build.md) |
| H-004 | Python tooling | Out of scope | — |
| H-005 | Saxon as XSLT processor; HE edition; pinned version | Surviving | [0013](0013-saxon-he-baseline.md), [0003](0003-functional-transformations.md) |
| H-006 | Document conversion tooling | Out of scope | — |
| H-007 | ArchiMate modelling with script automation | Refined | [0010](0010-architecture-models-as-evidence.md) |
| H-008 | Schematron and XSpec for validation and tests | Surviving | [0004](0004-shared-xslt-substrate.md), [0005](0005-validate-at-transformation-boundaries.md) |
| H-009 | Architecture model as single source of truth | Superseded (by H-035, H-037) | [0010](0010-architecture-models-as-evidence.md), [0012](0012-separate-canonical-knowledge-from-projections.md) |
| H-010 | Markdown as descriptive projection | Out of scope | — |
| H-011 | Element and relation index | Refined | [0007](0007-explicit-pipeline-phases.md) |
| H-012 | Document-to-XML chain with XSD and Schematron | Superseded; layering idea kept | [0005](0005-validate-at-transformation-boundaries.md) |
| H-013 | Structured metadata inside the architecture model | Superseded | [0010](0010-architecture-models-as-evidence.md) |
| H-014 | File naming convention | Out of scope | — |
| H-015 | Model round-trip (content duplicates H-013) | Defective | — |
| H-016 | Validation scope; fail fast; validation must not fix or infer (title does not match content) | Defective; content refined | [0005](0005-validate-at-transformation-boundaries.md), [0008](0008-derive-from-evidence-without-inference.md) |
| H-017 | Documentation organisation | Out of scope | — |
| H-018 | Metadata first; code-based reconstruction unreliable | Superseded (by H-037; contradicted by implementation) | [0008](0008-derive-from-evidence-without-inference.md) |
| H-019 | ADR quality rules | Refined | [Template](template.md) |
| H-020 | Lineage by design | Refined | [0008](0008-derive-from-evidence-without-inference.md), [0011](0011-preserve-provenance.md) |
| H-021 | Architecture as definition of done | Out of scope | — |
| H-022 | Phased validation enforcement | Refined | [0005](0005-validate-at-transformation-boundaries.md) |
| H-023 | Derive where possible, declare where not, never guess | Surviving | [0008](0008-derive-from-evidence-without-inference.md), [0010](0010-architecture-models-as-evidence.md) |
| H-024 | Component boundaries; validation and derivation do not overlap | Refined | [0007](0007-explicit-pipeline-phases.md) |
| H-025 | No-inference principle; gaps surfaced | Surviving | [0008](0008-derive-from-evidence-without-inference.md) |
| H-026 | Metadata ownership | Out of scope | — |
| H-027 | Systems emit their own metadata | Not realized | [0010](0010-architecture-models-as-evidence.md) |
| H-028 | Metadata governance and versioning | Not realized | — |
| H-029 | XML-based architecture model as canonical representation | Superseded (by H-036) | [0002](0002-common-structural-representation.md) |
| H-030 – H-033 | Versioning, conformance, impact, readiness | Superseded (by H-035 – H-040) | — |
| H-034 | Pipeline produces evidence; governance decides | Refined | [0010](0010-architecture-models-as-evidence.md) |
| H-035 | Near-reality principle | Surviving | [0008](0008-derive-from-evidence-without-inference.md), [0010](0010-architecture-models-as-evidence.md) |
| H-036 | Canonical graph is primary; views are projections | Surviving | [0012](0012-separate-canonical-knowledge-from-projections.md) |
| H-037 | Derive what implementation reveals; model ownership, meaning, intent | Surviving | [0008](0008-derive-from-evidence-without-inference.md), [0010](0010-architecture-models-as-evidence.md) |
| H-038 | Continuous reconciliation loop | Refined | [0007](0007-explicit-pipeline-phases.md), [0010](0010-architecture-models-as-evidence.md) |
| H-039 | Executable fitness functions; a rule that cannot fail a build is a suggestion | Surviving | [0005](0005-validate-at-transformation-boundaries.md) |
| H-040 | Lineage as a derived projection | Surviving | [0012](0012-separate-canonical-knowledge-from-projections.md) |
| H-041 | Candidate backlog of further decisions | Input to this reconstruction | [Reconstruction report](reconstruction-report.md) |

## Main changes of position

- **Source of truth.** Early records made the architecture model the single source of truth. Later records and the implementation treat it as evidence for intent beside implementation evidence. See [ADR-0010](0010-architecture-models-as-evidence.md).
- **Reconstruction from code.** Early records called reconstruction from code unreliable. Flowdigger's parser-based, deterministic derivation shows that what implementation reveals can be derived reliably. See [ADR-0008](0008-derive-from-evidence-without-inference.md).
- **"Infer".** Records used *infer* both for deterministic derivation and for guessing. Current ADRs use *derive* for the first and *inference* only for heuristic guessing.
