# ADR-0013: Use Saxon-HE as baseline processor with an optional schema-aware path

- **Status:** Accepted
- **Nature:** Retrospective architecture baseline

## Context

[ADR-0003](0003-functional-transformations.md) and [ADR-0004](0004-shared-xslt-substrate.md) require an XSLT 3.0 processor. Flowdigger is open source and must run without commercial licences. Some features, such as schema-aware processing, exist only in commercial processor editions.

## Decision

Use Saxon-HE, pinned to a fixed version, as the baseline processor for all XSLT, XQuery, XPath, Schematron and XSpec execution. Do not require schema-aware features. Where a schema-aware edition is present, use it as an option, with an automatic fallback to standard JAXP validation.

## Rationale

- Saxon-HE implements XSLT 3.0 and XPath 3.1 and is open source.
- A pinned version gives the same behaviour on every machine ([ADR-0006](0006-knowledge-reconstruction-as-build.md)).
- Optional schema-aware use lets organisations with a licence benefit without making it a dependency.

## Current realization

- `gradle-xml-plugin` depends on `net.sf.saxon:Saxon-HE` (version pinned in `build.gradle.kts`); the lineage plugin tests use the same line.
- `XsdTask` supports engines `AUTO`, `SAXON` and `JAXP`. `AUTO` uses Saxon schema validation only if a schema-aware processor is available, otherwise JAXP. `SAXON` without a schema-aware edition fails with a clear message.

## Consequences

- Positive: no licence needed; reproducible runs; optional upgrade path.
- Negative: no schema-aware XSLT and no streaming in the baseline; very large inputs must be handled by partitioning.

## Alternatives and earlier approaches

- **Xalan.** Rejected in earlier internal records: XSLT 1.0 only.
- **Saxon-PE/EE as a requirement.** Considered in earlier internal records and not selected because of licensing; mentioned as a possible future option for streaming and schema-aware processing.

## Evidence

- [gradle-xml-plugin build.gradle.kts](https://github.com/flowdigger/gradle-xml-plugin/blob/main/build.gradle.kts)
- [XsdTask.java](https://github.com/flowdigger/gradle-xml-plugin/blob/main/src/main/java/org/flowdigger/gradle/xml/XsdTask.java)
- [XsdEngine.java](https://github.com/flowdigger/gradle-xml-plugin/blob/main/src/main/java/org/flowdigger/gradle/xml/validation/XsdEngine.java)

## Related decisions

- [ADR-0003](0003-functional-transformations.md), [ADR-0004](0004-shared-xslt-substrate.md), [ADR-0006](0006-knowledge-reconstruction-as-build.md)
- Historical: H-005 (see [Historical decision records](history.md))
