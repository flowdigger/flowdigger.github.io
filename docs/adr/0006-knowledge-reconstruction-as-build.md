# ADR-0006: Treat knowledge reconstruction as a reproducible build

- **Status:** Accepted
- **Nature:** Retrospective architecture baseline

## Context

Knowledge reconstruction runs many steps over many inputs: repository sync, parsing, transformations, validations, exports. The steps have dependencies, the inputs change at different rates, and results must be reproducible on developer machines and in CI. Hand-written scripts tend to re-run everything, hide dependencies, and depend on local tool installations.

## Decision

Run knowledge reconstruction as a software build. Model each step as a build task with declared inputs and outputs, compose steps through the task graph, pin tool versions, and run everything through a versioned build tool wrapper. Gradle on Java 21 is the realization.

## Rationale

- Declared inputs and outputs make dependencies explicit and allow incremental, up-to-date checks.
- The task graph gives a reviewable composition of steps; the build script composes, it does not interpret ([ADR-0003](0003-functional-transformations.md)).
- The wrapper and pinned dependencies (for example Saxon-HE) make local and CI runs equal.
- Plugins package reusable machinery; projects only configure it.
- The same build runs transformation tests (TestKit, XSpec) and the pipeline itself.

## Current realization

- `gradle-lineage-plugin` registers the nine phase tasks ([ADR-0007](0007-explicit-pipeline-phases.md)) and registers default assets as task inputs, with a sorted asset index.
- Default phase assets are bundled in the plugin jar and extracted to a content-hash directory, so an asset change produces a new input fingerprint.
- `lineagePipeline.defaults` and source-set profiles let projects override assets without changing plugin code (transformation injection).
- `gradle-xml-plugin`, `gradle-archi-plugin`, `gradle-ooxml-plugin` and the ANTLR plugin family each expose their processing as Gradle tasks.
- Repositories use the Gradle wrapper; CI uses shared GitHub Actions workflows.

## Consequences

- Positive: incremental and reproducible runs; one entry point for developers and CI; reusable plugins.
- Negative: Gradle knowledge is required; build logic needs maintenance.
- Gap: not all tasks are cacheable, and validation tasks are not cacheable by default. The OOXML integration currently prevents configuration-cache reuse in some cases (documented in the lineage plugin README).

## Alternatives and earlier approaches

- **Maven, Ant, or shell scripts with a command-line XSLT processor.** Earlier internal records list these as alternatives that were not selected; reasons given were weaker task graph, weaker incremental support, or poor reproducibility.
- **System-installed build tool instead of a wrapper.** Not selected: version drift between machines.
- **Integration or dataflow platforms such as Apache Camel or Apache NiFi.** These are present-day alternatives named here for comparison only; there is no evidence that they were evaluated. They orchestrate runtime message flows, while Flowdigger needs a reproducible, file-based build.

## Evidence

- [gradle-lineage-plugin README](https://github.com/flowdigger/gradle-lineage-plugin/blob/main/README.md): task graph, profiles, asset overrides, configuration-cache note.
- [NARRATIVE-RUNTIME-CONTRACT.md](https://github.com/flowdigger/gradle-lineage-plugin/blob/main/NARRATIVE-RUNTIME-CONTRACT.md): default asset root and content-hash extraction.
- [gradle-xml-plugin build.gradle.kts](https://github.com/flowdigger/gradle-xml-plugin/blob/main/build.gradle.kts): pinned Saxon-HE.
- [github-workflows](https://github.com/flowdigger/github-workflows): shared CI workflows.

## Related decisions

- [ADR-0003](0003-functional-transformations.md), [ADR-0007](0007-explicit-pipeline-phases.md), [ADR-0009](0009-replaceable-source-adapters.md), [ADR-0013](0013-saxon-he-baseline.md)
- Historical: H-003, H-005 (see [Historical decision records](history.md))
