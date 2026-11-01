# Papers: XIR, Metadata Production, Reprojection and Lineage Analysis

This collection consolidates the architectural ideas developed in the discussion into four papers for a technical enterprise audience: engineers, solution and enterprise architects, metadata and lineage specialists, and technically oriented managers.

## Papers

1. **[01-xir-xml-in-the-age-of-ai.md](01-xir-xml-in-the-age-of-ai.md)**  
   Positions XIR as XML revisited for AI-era constraints. Covers XIR-SAX, ANTLR/G4 tree walking, grammar-derived schemas, structural evidence and the boundary between syntax and semantics.

2. **[02-metadata-as-a-build-product.md](02-metadata-as-a-build-product.md)**  
   Describes metadata production as part of CI/CD. It separates evidence authority from publication authority, explains why deterministic functional derivation matters, and shows how architecture, configuration and code can form one coherent evidence chain.

3. **[03-from-lineage-to-reprojection.md](03-from-lineage-to-reprojection.md)**  
   Develops the idea that lineage is one derived projection of application knowledge. It treats applications as projections, reverse engineering as specification recovery, and modernization as controlled reprojection rather than code translation.

4. **[04-fast-metadata-workbench-dtap-analysis.md](04-fast-metadata-workbench-dtap-analysis.md)**
   Defines a Spring Boot/Neo4j metadata workbench behind a Collibra-compatible REST boundary. It covers rapid metadata publication, contract testing, multiple D/T/A environments, lineage drift, impact analysis and evidence-aware root-cause investigation.

## Shared principles

- Keep authority close to the evidence that originates a fact.
- Derive technical knowledge instead of manually maintaining it where possible.
- Treat generated metadata as a reproducible build product.
- Preserve provenance for derived assertions.
- Keep evidence, derived knowledge and published governance views distinct.
- Treat lineage as one projection, not the complete knowledge model.
- Use stable adapters such as SAX and REST contracts to avoid unnecessary migration and coupling.
