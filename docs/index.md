# Flowdigger

Flowdigger reconstructs system knowledge from technical evidence such as code, SQL, configuration and architecture models. Deterministic transformations turn that evidence into lineage and other views that teams can review.

!!! warning "Examples are under construction"
    The examples are not yet fully runnable. This site describes the approach and existing components; it does not yet provide a complete, verified end-to-end tutorial.

The lineage pipeline has implemented phases, but coverage varies by source. End-to-end provenance and a PROV-O mapping are not implemented. See [implementation status](lineage/index.md#implementation-status).

- [Start here](start-here/index.md) for value, maturity and reader paths.
- [Learn from the LinkedIn series](learn/system-knowledge/index.md) or explore [core concepts](core-concepts/data-lineage-basics.md).
- [Explore unfinished examples](how-to/build-first-lineage-slice.md) and [quality checks](how-to/validate-lineage-quality.md).
- [Understand the architecture](lineage/index.md) and its [decisions](adr/index.md).
- [Find terms](reference/glossary.md), [community contact](community/index.md), and [research](flowdigger-deterministic-knowledge-derivation.md).

## Why Flowdigger

Explicit rules and technical evidence support repeatable, reviewable knowledge derivation.

## What makes it different

### Transformation injection

The pipeline supports domain-specific transformations at defined stages; see [deterministic knowledge derivation](flowdigger-deterministic-knowledge-derivation.md).

### Continuous Architecture Reconciliation

Architecture models can be compared with implementation evidence; see [ADR-0010](adr/0010-architecture-models-as-evidence.md).

## Start now

Choose a [reader path](start-here/index.md#choose-your-path).

## Ecosystem at glance

- [Gradle Lineage Plugin](https://github.com/flowdigger/gradle-lineage-plugin): pipeline orchestration.
- [Gradle XML Plugin](https://github.com/flowdigger/gradle-xml-plugin): XSLT, XPath, Schematron and XSpec.
- [Gradle Archi Plugin](https://github.com/flowdigger/gradle-archi-plugin): ArchiMate processing.
- [ANTLR plugin family](https://github.com/flowdigger/gradle-antlr-plugin): parser-based extraction.
- [XIR tools](https://github.com/flowdigger/xir-sax): XML-compatible representation and processing.
