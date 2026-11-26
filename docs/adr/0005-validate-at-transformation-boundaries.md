# ADR-0005: Validate derived knowledge at meaningful transformation boundaries

- **Status:** Accepted
- **Nature:** Retrospective architecture baseline

## Context

Each derivation step can produce wrong or incomplete results: a parse can miss constructs, an observe rule can match too much, and a stitch can connect the wrong endpoints. If errors travel to the end of the pipeline, it is hard to find the step that caused them. A rule that can never fail the build has no effect.

## Decision

Validate intermediate and final results at the boundaries where their contract changes: after direct-flow derivation and after lineage stitching. Express rules as executable, versioned artifacts (XSD for structure, Schematron for rules, XSpec for transformation behaviour) that can fail the build. Allow staged enforcement so that new rules can be introduced before they become blocking.

## Rationale

- A failure at a boundary points to the phase that produced the defect.
- Executable rules are reviewable and versioned with the code they protect.
- Structural validation (XSD) and rule validation (Schematron) answer different questions; using both keeps each simple.
- Validation must only report. It must not fix, infer or complete data, because that would hide derivation defects ([ADR-0008](0008-derive-from-evidence-without-inference.md)).
- Staged enforcement (opt-in gates, explicit skip switch) lets teams adopt rules gradually without making every new rule a blocking change.

## Current realization

- Phase 4 `validateDirectFlows` runs `4.validate-direct-flow/src/main/schematron/direct-flow-validation.sch` on direct flows.
- Phase 7 `validateLineage` runs `7.validate-lineage/src/main/schematron/validate-lineage.sch` on stitched lineage.
- `verifyCallAppSqlInputs` fails fast before stitching when expected SQL index artifacts are missing.
- An opt-in Java coverage gate fails on zero operational flows when enabled with `-PlineageFailOnZeroOperationalFlow=true`.
- `skipLineageValidations` disables the validation phases explicitly.
- Same-schema SQL definitions that conflict fail; ambiguous unqualified definitions fail.
- `gradle-xml-plugin` provides `XsdTask`, `SchematronTask` (SVRL output) and `XSpecTask`. Lineage stylesheets have XSpec/Schematron smoke tests.

## Consequences

- Positive: defects are found near their cause; rules can block releases; rule changes are visible in review.
- Negative: rules must be maintained with the transformations; validation tasks add build time.
- Validation tasks are not cacheable by default in `gradle-xml-plugin`, so they run again when invoked.
- The explicit skip switch must be visible in build output, so that skipped validation is not mistaken for passed validation.

## Alternatives and earlier approaches

- **Validate only the final result.** Simpler, but defects lose their location.
- **XSD only.** Earlier internal records rejected XSD alone for business rules: it cannot express co-occurrence and cross-document rules.
- Earlier internal records described phased enforcement levels (advisory, warning, enforced) and the principle that a rule that cannot fail a build is only a suggestion. Both survive here in the form of opt-in gates and Schematron gates.

## Evidence

- [direct-flow-validation.sch](https://github.com/flowdigger/gradle-lineage-plugin/blob/main/pipeline/4.validate-direct-flow/src/main/schematron/direct-flow-validation.sch)
- [validate-lineage.sch](https://github.com/flowdigger/gradle-lineage-plugin/blob/main/pipeline/7.validate-lineage/src/main/schematron/validate-lineage.sch)
- [NARRATIVE-RUNTIME-CONTRACT.md](https://github.com/flowdigger/gradle-lineage-plugin/blob/main/NARRATIVE-RUNTIME-CONTRACT.md), section "Validation/fail-fast gates".
- [XsdTask.java](https://github.com/flowdigger/gradle-xml-plugin/blob/main/src/main/java/org/flowdigger/gradle/xml/XsdTask.java), [SchematronTask.java](https://github.com/flowdigger/gradle-xml-plugin/blob/main/src/main/java/org/flowdigger/gradle/xml/SchematronTask.java)
- Documentation: [Validate lineage quality](../how-to/validate-lineage-quality.md).

## Related decisions

- [ADR-0004](0004-shared-xslt-substrate.md), [ADR-0007](0007-explicit-pipeline-phases.md), [ADR-0008](0008-derive-from-evidence-without-inference.md)
- Historical: H-008, H-012, H-016, H-022, H-039 (see [Historical decision records](history.md))
