# Flowdigger

Legacy systems contain architecture, lineage, and metadata knowledge across code, SQL, configuration, build files, and architecture models. Flowdigger derives that knowledge deterministically, so results stay reproducible, traceable, and reviewable.

```text
SOURCE -> SYNTAX -> SEMANTICS -> FLOW -> LINEAGE
```

## Why Flowdigger

- **Deterministic derivation:** same evidence + same rules -> same output.
- **Explicit interpretation:** transformations and validations are versioned code, not hidden logic.
- **Evidence-first trust:** every derived relation can be traced back to source artifacts and rules.

## What makes it different

### Transformation injection

Core pipeline stays generic. Domain-specific interpretation is injected as transformations at stable stages, so private enterprise semantics stay private while orchestration stays reusable.

### Continuous Architecture Reconciliation

ArchiMate captures intended architecture. Flowdigger derives realized structure from implementation evidence. Reconciliation makes drift, conformance, and missing mappings explicit.

## Start now

- **Read concept first:** [Data lineage in legacy systems](core-concepts/data-lineage-basics.md)
- **Build first result:** [Build first lineage slice](how-to/build-first-lineage-slice.md)
- **Validate output quality:** [Validate lineage quality](how-to/validate-lineage-quality.md)
- **Choose guided path:** [Reading tracks](reading-tracks.md)
- **Deep dive architecture:** [Flowdigger deterministic knowledge derivation](flowdigger-deterministic-knowledge-derivation.md)

## Ecosystem at glance

- [Gradle Lineage Plugin](https://github.com/flowdigger/gradle-lineage-plugin): nine-phase lineage orchestration.
- [Gradle XML Plugin](https://github.com/flowdigger/gradle-xml-plugin): XSLT/XPath/Schematron/XSpec execution.
- [Gradle Archi Plugin](https://github.com/flowdigger/gradle-archi-plugin): headless ArchiMate processing.
- [ANTLR plugin family](https://github.com/flowdigger/gradle-antlr-plugin): parser-based syntax extraction across languages.
- [XIR tools](https://github.com/flowdigger/xir-sax): compact XML-compatible representation and processing.
