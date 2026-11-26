# Glossary and Acronyms

## Purpose

Keep terminology consistent across pages.

## Terms

- **Data lineage**: trace of data origin, transformations, and destinations.
- **Provenance**: record that links derived fact to source evidence.
- **Derivation**: deterministic process that creates lineage facts from evidence.
- **Legacy system**: older system with mixed technologies and undocumented behavior.
- **Observation**: fact extracted from parsed source, configuration, document, or model (pipeline phase 2).
- **Direct flow**: local source → operation → target relation derived from observations (phase 3).
- **Merge**: combination of validated results of the same kind, with indexes; adds no new relations (phase 5).
- **Stitch**: connection of local flows across components, only through explicit declarations (phase 6).
- **Canonical result**: validated stitched lineage from which all consumer formats are produced.
- **Projection**: consumer representation (GraphML, RDF, JSON) produced from the canonical result.
- **Inference**: in Flowdigger, heuristic guessing without sufficient evidence. Flowdigger derives; it does not infer.
- **Processing language**: language that executes transformation, validation, or extraction (XSLT, XQuery, Schematron, SEL).
- **Vocabulary**: established semantic or domain vocabulary (for example RDF-based vocabularies, ArchiMate concepts). Not a processing language.

## Acronyms

- **ADR**: Architecture Decision Record. See [Architecture Decision Records](../adr/index.md).
- **ASD-STE100**: Aerospace and Defence Industries Association of Europe - Simplified Technical English.
- **DTAP**: Development, Test, Acceptance, Production.
- **XIR**: eXtensible Intermediate Representation.
- **SQL**: Structured Query Language.
- **RDF**: Resource Description Framework.
- **SKOS**: Simple Knowledge Organization System.
- **W3C**: World Wide Web Consortium.

