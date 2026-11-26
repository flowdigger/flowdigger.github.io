# ADR-0001: Prefer established standards before Flowdigger-specific semantics

- **Status:** Accepted
- **Nature:** Retrospective architecture baseline

## Context

Flowdigger reads evidence from many technologies and produces knowledge for many consumers. Each new format, rule language, or result model that Flowdigger invents is one more thing that users must learn, tools must support, and maintainers must keep stable.

Mature standards already exist for most of the processing and exchange that Flowdigger needs: W3C XML, XPath, XSLT, XQuery and XML Schema; ISO Schematron and its report format SVRL; W3C RDF; GraphML; and The Open Group ArchiMate for architecture models.

## Decision

Use an established standard, processing language, or semantic vocabulary when one fits the need. Introduce Flowdigger-specific constructs only where no established standard covers the need, and keep those constructs small and explicitly namespaced.

## Rationale

- Standards carry their own specifications, test suites, tools, and community knowledge. Flowdigger reuses that investment instead of re-creating it.
- Consumers can process outputs (SVRL, RDF, GraphML) with general tooling, without Flowdigger code.
- Rules written in a standard language (for example Schematron) remain readable and portable even if Flowdigger itself changes.
- Flowdigger-specific constructs that remain (XIR syntax, SEL annotations, lineage element names) become clearly identifiable as Flowdigger's own contribution.

## Current realization

| Need | Standard used | Where |
|---|---|---|
| Transformation | XSLT 3.0, XPath 3.1 | lineage pipeline assets, `gradle-xml-plugin` XSLT task |
| Query | XQuery | `gradle-xml-plugin` XQuery task |
| Structural validation | W3C XML Schema (XSD) | `gradle-xml-plugin` XSD task |
| Rule validation and report | ISO Schematron, SVRL | `gradle-xml-plugin` Schematron task; lineage phases 4 and 7 |
| Transformation tests | XSpec | `gradle-xml-plugin` XSpec task |
| Architecture model | ArchiMate (Archi tool format) | `gradle-archi-plugin`, `example-lineage-plsql/1-architecture` |
| Graph exchange | GraphML | lineage phase 8 |
| Semantic exchange | RDF | lineage phase 8 |

Flowdigger-specific constructs: XIR text syntax ([ADR-0002](0002-common-structural-representation.md)), SEL annotations in the `urn:flowdigger:sel` namespace on top of Schematron ([ADR-0004](0004-shared-xslt-substrate.md)), and lineage result elements.

Terminology: in this documentation, *vocabulary* means an established semantic or domain vocabulary (for example RDF-based vocabularies, ArchiMate concepts). XSLT, XQuery, Schematron and SEL are *processing languages*, not vocabularies.

## Consequences

- Positive: lower learning cost for users with XML or semantic-web background; outputs open to general tooling; rules stay portable.
- Negative: Flowdigger inherits the limits of the chosen standards (for example XSD expressiveness) and must sometimes combine several standards to cover one need.
- Gap: SKOS and PROV-O appear in conceptual papers, but no implemented mapping exists yet. See [ADR-0011](0011-preserve-provenance.md) and [ADR-0012](0012-separate-canonical-knowledge-from-projections.md).

## Alternatives and earlier approaches

- **Own rule language or own result format.** Not adopted. Schematron and SVRL already provide rule-based validation with structured results.
- **General-purpose code for transformation and validation.** Possible, but the result is less declarative and less portable. See [ADR-0003](0003-functional-transformations.md).
- Earlier internal decision records already chose Saxon, Schematron and XSpec. They did so for tool reasons; this ADR states the general principle behind those choices.

## Evidence

- [gradle-xml-plugin README](https://github.com/flowdigger/gradle-xml-plugin/blob/main/README.md): XSLT, XQuery, XSD, Schematron and XSpec tasks.
- [SchematronTask.java](https://github.com/flowdigger/gradle-xml-plugin/blob/main/src/main/java/org/flowdigger/gradle/xml/SchematronTask.java): Schematron compiled to XSLT, SVRL output.
- [schematron-sel-language.md](https://github.com/flowdigger/gradle-xml-plugin/blob/main/schematron-sel-language.md): SEL as annotations on standard Schematron.
- [Lineage export stylesheets](https://github.com/flowdigger/gradle-lineage-plugin/tree/main/pipeline/8.export/src/main/xsl): GraphML, RDF and JSON exports.
- [example-lineage-plsql architecture module](https://github.com/flowdigger/example-lineage-plsql/tree/main/1-architecture): ArchiMate model as input.

## Related decisions

- [ADR-0002](0002-common-structural-representation.md), [ADR-0004](0004-shared-xslt-substrate.md), [ADR-0012](0012-separate-canonical-knowledge-from-projections.md)
- Historical: H-005, H-008 (see [Historical decision records](history.md))
