# ADR-0003: Use functional, declarative transformations for deterministic knowledge derivation

- **Status:** Accepted
- **Nature:** Retrospective architecture baseline

## Context

Lineage is only useful as evidence when it is reproducible: the same evidence and the same rules must give the same result, and a reviewer must be able to see which rule produced which fact. Derivation logic hidden in general-purpose code with mutable state, shared caches, or order-dependent side effects is hard to review, hard to test in isolation, and can give different results between runs.

## Decision

Express knowledge derivation as functional, declarative transformations from input documents to output documents. Each transformation is a function of its declared inputs and parameters, keeps no hidden state between runs, and can be tested on its own. XSLT 3.0 is the realization.

## Rationale

- **Declarative:** rules state *what* to match and produce (templates, XPath), not a sequence of state changes. Domain experts can read and review them.
- **Functional, side-effect free:** XSLT variables are immutable and templates have no hidden mutable pipeline state. Output depends on input documents and parameters.
- **Composable:** the output of one transformation is a document that the next transformation, a validator, or a test can read. Phases compose by files ([ADR-0007](0007-explicit-pipeline-phases.md)).
- **Independently testable:** each stylesheet can be tested with input/output fixtures (XSpec, TestKit smoke tests).
- **Reproducible:** with explicit sorting and stable identifiers, repeated runs give identical output, which the build can use for up-to-date checks ([ADR-0006](0006-knowledge-reconstruction-as-build.md)).
- **Injectable:** domain-specific interpretation can be supplied as extra or replacement stylesheets at fixed phases without changing orchestration code.

## Current realization

- Phases 2–8 of `gradle-lineage-plugin` are implemented mainly as XSLT 3.0 stylesheets under `pipeline/<phase>/src/main/xsl`.
- Stylesheets read input documents and explicit parameters (for example `table-index-uri`, `configuration-uri`, `application-uri` in `stitch-lineage.xsl`) and use explicit sorting (`xsl:sort`, `sort()`) for stable output order.
- Default assets can be replaced per phase through `lineagePipeline.defaults` and source-set profiles.
- Gradle (Groovy) code registers tasks and wires files; it does not hold derivation rules.

## Consequences

- Positive: derivation rules are reviewable, testable and reproducible; domain rules can be injected per phase.
- Negative: XSLT skills are required; some algorithms (for example graph closure) are less natural in XSLT than in imperative code.
- Gap: `8.export/src/main/xsl/app-to-rdf.xsl` uses `generate-id()` to form RDF identifiers. `generate-id()` is not a guaranteed stable identifier across processors or runs. Stable, content-based identifiers are preferred for projections.
- Gap: some phases also run Groovy or Python helpers. These are task wiring; they must not contain hidden derivation rules.

## Alternatives and earlier approaches

- **Imperative derivation in Java/Groovy.** Possible, but rules become harder to separate from orchestration and harder for non-programmers to review.
- **Graph or rule engines with mutable working memory.** Not evidenced as evaluated. They make order and state part of the result, which works against reproducibility.
- An earlier internal decision record chose XSLT through Saxon mainly for XSLT 2.0/3.0 features. This ADR records the deeper reason that still holds: functional, declarative derivation.

## Evidence

- [Lineage pipeline phases](https://github.com/flowdigger/gradle-lineage-plugin/tree/main/pipeline)
- [stitch-lineage.xsl](https://github.com/flowdigger/gradle-lineage-plugin/blob/main/pipeline/6.stitch/src/main/xsl/stitch-lineage.xsl): parameters, immutable maps, explicit sorting.
- [NARRATIVE-RUNTIME-CONTRACT.md](https://github.com/flowdigger/gradle-lineage-plugin/blob/main/NARRATIVE-RUNTIME-CONTRACT.md): default asset resolution and overrides.
- [app-to-rdf.xsl](https://github.com/flowdigger/gradle-lineage-plugin/blob/main/pipeline/8.export/src/main/xsl/app-to-rdf.xsl): `generate-id()` gap.
- Documentation: [Flowdigger deterministic knowledge derivation](../flowdigger-deterministic-knowledge-derivation.md), sections 4 and 6.

## Related decisions

- [ADR-0004](0004-shared-xslt-substrate.md), [ADR-0006](0006-knowledge-reconstruction-as-build.md), [ADR-0007](0007-explicit-pipeline-phases.md), [ADR-0008](0008-derive-from-evidence-without-inference.md)
- Historical: H-005, H-016 (see [Historical decision records](history.md))
