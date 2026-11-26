# ADR-0004: Share one XSLT execution substrate for transformation, validation and extraction

- **Status:** Accepted
- **Nature:** Retrospective architecture baseline

## Context

Flowdigger needs three different kinds of processing: transformation (derive new documents), validation (assert rules and report violations), and evidence extraction (select and group facts from structured input). Separate engines for each kind would mean separate runtimes, separate configuration, and separate test approaches.

## Decision

Run transformation, rule validation and evidence extraction on one functional execution substrate: XSLT 3.0 on Saxon. Schematron is compiled to XSLT. SEL (Schematron Evidence Language) and SHACL-SEL are compiled to XSLT as well. Keep the *responsibilities* separate even though the *execution mechanism* is shared.

## Rationale

- One processor, one configuration and one test framework (XSpec) cover all three kinds of processing.
- All three inherit the properties of [ADR-0003](0003-functional-transformations.md): declarative, side-effect free, reproducible.
- Schematron gives a rule-oriented authoring form for assertions; SEL gives a rule-oriented authoring form for evidence selection. Authors use the form that fits their responsibility.
- Validation stays validation: a Schematron schema does not derive knowledge, even though it executes as XSLT. Extraction stays extraction: SEL selects evidence, it does not assert validity.

## Current realization

- `gradle-xml-plugin` `SchematronTask` compiles Schematron to XSLT with the bundled SchXslt transpiler and produces SVRL.
- `SchematronSelCompileTask` and `SchematronSelExtractTask` compile Schematron rules with SEL annotations (`urn:flowdigger:sel`) to XSLT and run them to extract evidence.
- `ShaclSelCompileTask` compiles SHACL-style shapes with SEL annotations to the same substrate.
- XSLT, XQuery and XSpec tasks run on the same Saxon processor.
- Lineage phases 4 and 7 use Schematron rules; phases 2–8 use XSLT.

## Consequences

- Positive: one runtime and one test approach; rule authors can choose the most suitable declarative form.
- Negative: everything depends on the XSLT processor; processor limits apply to all three kinds of processing ([ADR-0013](0013-saxon-he-baseline.md)).
- SEL and SHACL-SEL are Flowdigger-specific *processing languages* layered on standards. They are not vocabularies ([ADR-0001](0001-prefer-established-standards.md)).

## Alternatives and earlier approaches

- **Separate engines** (for example a dedicated SHACL engine on RDF, or a Java rule engine). Not adopted for the core pipeline; they would add a second runtime and a second data model.
- Earlier internal decision records already used Schematron compiled to XSLT, with SVRL output and XSpec tests. This ADR keeps that and extends the same substrate to evidence extraction.

## Evidence

- [SchematronTask.java](https://github.com/flowdigger/gradle-xml-plugin/blob/main/src/main/java/org/flowdigger/gradle/xml/SchematronTask.java)
- [SchematronSelCompileTask.java](https://github.com/flowdigger/gradle-xml-plugin/blob/main/src/main/java/org/flowdigger/gradle/xml/SchematronSelCompileTask.java), [SchematronSelExtractTask.java](https://github.com/flowdigger/gradle-xml-plugin/blob/main/src/main/java/org/flowdigger/gradle/xml/SchematronSelExtractTask.java), [ShaclSelCompileTask.java](https://github.com/flowdigger/gradle-xml-plugin/blob/main/src/main/java/org/flowdigger/gradle/xml/ShaclSelCompileTask.java)
- [schematron-sel-language.md](https://github.com/flowdigger/gradle-xml-plugin/blob/main/schematron-sel-language.md)
- Documentation: [SEL Derivation Language](../SEL-Derivation-Language.md).

## Related decisions

- [ADR-0003](0003-functional-transformations.md), [ADR-0005](0005-validate-at-transformation-boundaries.md), [ADR-0013](0013-saxon-he-baseline.md)
- Historical: H-008 (see [Historical decision records](history.md))
