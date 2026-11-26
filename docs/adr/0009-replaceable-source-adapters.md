# ADR-0009: Keep source adapters replaceable and source-specific

- **Status:** Accepted
- **Nature:** Retrospective architecture baseline

## Context

Enterprise systems use many languages and dialects: PL/SQL, T-SQL, SAS, Java, configuration files, OOXML documents and architecture models. New sources appear over time. If parsing and observation for every source sit inside the core pipeline, each new source changes core code.

## Decision

Isolate source-specific parsing and observation in adapters that the core pipeline selects by configuration. The core pipeline knows phases and contracts, not grammars. Adapters produce the common structure of [ADR-0002](0002-common-structural-representation.md).

## Rationale

- New sources are added by adding an adapter and a profile, without changes to phases 6–9.
- Grammar upgrades stay local to one adapter.
- Projects can replace a default adapter or its observe rules with domain-specific versions (transformation injection) while keeping private semantics private.

## Current realization

- Parser adapters: `gradle-antlr-plugin` (generic ANTLR → XML) and language plugins `gradle-antlr-plsql-plugin`, `gradle-antlr-tsql-plugin`, `gradle-antlr-sas-plugin`, `gradle-antlr-java-plugin`.
- Source sets select an adapter with `parserPlugin` (for example `org.flowdigger.antlr.plsql`); the final segment selects the profile unless `profile` is set.
- Non-code evidence adapters: `gradle-ooxml-plugin` (OOXML documents), `gradle-archi-plugin` (ArchiMate models), and the `config` profile for configuration files.
- `xir-g4` derives structural models from grammars for adapter development.

## Consequences

- Positive: independent release of adapters; small core; local grammar changes.
- Negative: more repositories to maintain; adapter output contracts must stay compatible with observe rules.
- SAS has a parser adapter, but no built-in lineage profile is evidenced yet.

## Alternatives and earlier approaches

- **One parser library inside the core.** Simpler to start, but every grammar change becomes a core release.
- **Regular-expression scanners.** Fast to write, but not reliable enough for deterministic derivation.

## Evidence

- [gradle-lineage-plugin README](https://github.com/flowdigger/gradle-lineage-plugin/blob/main/README.md): `parserPlugin`, profiles.
- [gradle-antlr-plugin](https://github.com/flowdigger/gradle-antlr-plugin), [gradle-antlr-plsql-plugin](https://github.com/flowdigger/gradle-antlr-plsql-plugin), [gradle-antlr-tsql-plugin](https://github.com/flowdigger/gradle-antlr-tsql-plugin), [gradle-antlr-sas-plugin](https://github.com/flowdigger/gradle-antlr-sas-plugin), [gradle-antlr-java-plugin](https://github.com/flowdigger/gradle-antlr-java-plugin)
- [gradle-ooxml-plugin](https://github.com/flowdigger/gradle-ooxml-plugin), [gradle-archi-plugin](https://github.com/flowdigger/gradle-archi-plugin)
- [xir-g4](https://github.com/flowdigger/xir-g4)

## Related decisions

- [ADR-0002](0002-common-structural-representation.md), [ADR-0006](0006-knowledge-reconstruction-as-build.md), [ADR-0007](0007-explicit-pipeline-phases.md)
