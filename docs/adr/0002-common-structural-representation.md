# ADR-0002: Represent heterogeneous evidence in a common structure that keeps source-specific names

- **Status:** Accepted
- **Nature:** Retrospective architecture baseline

## Context

Flowdigger reads PL/SQL, T-SQL, SAS, Java, configuration, OOXML documents, and architecture models. Each source has its own grammar, names, and meaning. Downstream steps (observation, derivation, validation) must process all of them with the same tools.

Two extremes exist. A separate tool chain per source keeps meaning intact but duplicates all machinery. One universal semantic model lets one tool chain process everything, but it forces early interpretation and loses source-specific meaning.

## Decision

Represent parsed evidence in one common *structural* representation (XML/XDM, with XIR as compact text syntax), and keep source-specific names, types and namespaces inside that structure. Do not map evidence into one universal ontology at parse time. Semantic interpretation happens later, in explicit observe and derive transformations.

## Rationale

- One structural model lets all phases use the same processing languages (XPath, XSLT, Schematron) for all sources.
- Keeping grammar rule names and token types as data keeps the evidence faithful to its source. Interpretation stays explicit and reviewable in later transformations ([ADR-0003](0003-functional-transformations.md)).
- A new source language needs a new adapter and new observe rules, not a change to a shared ontology ([ADR-0009](0009-replaceable-source-adapters.md)).
- XIR is a syntax for the XML data model, not a new data model. Any XIR document can be read as XML and processed with standard tools ([ADR-0001](0001-prefer-established-standards.md)).

## Current realization

- `gradle-antlr-plugin` converts ANTLR parse trees into a generic XML tree. Rule nodes keep the grammar rule name and token nodes keep the token type and source line (for example `r[@name='func_proc_name_schema']`, `t[@type='IDENTIFIER'][@line]`), as consumed by the observe stylesheets.
- `xir-sax` parses and serializes XIR text as SAX events, so XIR and XML are interchangeable for pipeline tools.
- `xir-g4` derives structural models and AST classes from ANTLR grammars and renders them as XIR.
- Observe stylesheets per source kind (`2.observe/src/main/xsl/<kind>`) turn source-specific structure into observations.

## Consequences

- Positive: one tool chain for all sources; parse output stays close to source; interpretation stays testable.
- Negative: observe rules depend on grammar rule names, so a grammar change can require rule changes. Generic trees are verbose.
- XIR is a structural representation. Documentation must not describe XIR as a universal ontology or as the semantic model of the enterprise.
- Open question: a documented convention for source-specific namespaces in parse output is not yet visible in the implementation.

## Alternatives and earlier approaches

- **Universal semantic model at parse time.** Not adopted, for the reasons in Context.
- **Per-language typed AST classes as the exchange format.** `xir-g4` can generate them, but the pipeline exchanges XML/XIR so that XSLT and Schematron can process all sources.
- Earlier internal decision records named an XML-based architecture model as the *canonical representation* of the system. That idea is narrowed here: XML is the common structure for evidence; it is not a claim that one model holds all truth. See [ADR-0010](0010-architecture-models-as-evidence.md).

## Evidence

- [DynamicAntlrXmlAstConverter.java](https://github.com/flowdigger/gradle-antlr-plugin/blob/main/src/main/java/org/flowdigger/gradle/antlr/DynamicAntlrXmlAstConverter.java)
- [xir-sax README](https://github.com/flowdigger/xir-sax/blob/main/README.md) and [XIR reference implementation spec](https://github.com/flowdigger/xir-sax/blob/main/XIR-Reference-Implementation-Spec.md)
- [xir-g4 README](https://github.com/flowdigger/xir-g4/blob/master/README.md)
- [T-SQL observe stylesheet](https://github.com/flowdigger/gradle-lineage-plugin/blob/main/pipeline/2.observe/src/main/xsl/tsql/extract.xsl): uses grammar rule names and token lines.
- Documentation: [XIR Structural Model](../XIR_Structural_Model_Paper.md).

## Related decisions

- [ADR-0001](0001-prefer-established-standards.md), [ADR-0003](0003-functional-transformations.md), [ADR-0009](0009-replaceable-source-adapters.md), [ADR-0011](0011-preserve-provenance.md)
- Historical: H-029 (see [Historical decision records](history.md))
