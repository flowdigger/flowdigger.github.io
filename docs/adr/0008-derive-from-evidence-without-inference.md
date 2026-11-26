# ADR-0008: Derive knowledge only from evidence, keep gaps visible, keep AI assistive

- **Status:** Accepted
- **Nature:** Retrospective architecture baseline

## Context

Lineage tools often fill gaps with heuristics: name similarity, guessed joins, or statistical or AI-generated links. The result looks complete, but a reviewer cannot tell which relations are supported by evidence and which are guesses. For audit and impact analysis, an unknown relation that is shown as unknown is more useful than a guessed relation that is shown as fact.

## Decision

Create a relation only when evidence supports it: parsed source, configuration, documents, or explicit declarations in architecture models. Make the derivation deterministic. When evidence is missing or ambiguous, keep the gap visible (diagnostic, unresolved endpoint, or failed validation) instead of filling it. AI can assist people who write rules, adapters and declarations; AI output is not used as authoritative evidence in the pipeline run.

Terminology: *derive* means a deterministic rule over evidence. *Inference* in this ADR means heuristic guessing without sufficient evidence.

## Rationale

- Each relation can be traced to evidence and a rule ([ADR-0011](0011-preserve-provenance.md)).
- Visible gaps show where declarations or adapters are missing. They are work items, not hidden defects.
- Deterministic derivation gives the same result for the same evidence ([ADR-0003](0003-functional-transformations.md)).
- AI makes writing rules and adapters cheaper, but it does not change who is accountable for the result.

## Current realization

- Calls, table references and direct flows across SQL schemas are omitted and diagnosed instead of guessed.
- Stitching connects domains only through explicit architecture or configuration declarations; unresolved endpoints remain visible.
- Ambiguous unqualified SQL definitions fail; conflicting declarations raise an error in `stitch-lineage.xsl`.
- Java links that cannot be resolved precisely are marked as approximate or unresolved.
- No AI or LLM call exists in the runtime code of the pipeline plugins.

## Consequences

- Positive: results are explainable and auditable; gaps drive targeted improvement.
- Negative: first results can look less complete than results of heuristic tools; explicit declarations take effort.
- Approximate Java links are a known weak point: they must stay marked as approximate in outputs.

## Alternatives and earlier approaches

- **Heuristic or AI-based link completion in the pipeline.** Not adopted.
- **Model-only lineage (declare everything by hand).** Earlier internal records preferred declared metadata and called code-based reconstruction unreliable. The parser-based derivation in Flowdigger contradicts that position for what implementation reveals; declarations remain necessary for what it does not reveal ([ADR-0010](0010-architecture-models-as-evidence.md)).
- Earlier internal records stated both "never infer" and "if it can be inferred, derive it". The terminology above resolves this apparent conflict.

## Evidence

- [NARRATIVE-RUNTIME-CONTRACT.md](https://github.com/flowdigger/gradle-lineage-plugin/blob/main/NARRATIVE-RUNTIME-CONTRACT.md), sections 2 and 3.
- [stitch-lineage.xsl](https://github.com/flowdigger/gradle-lineage-plugin/blob/main/pipeline/6.stitch/src/main/xsl/stitch-lineage.xsl): declaration-based stitching, conflict error.
- Documentation: [AI changes the economics, not the authority](../flowdigger-deterministic-knowledge-derivation.md#13-ai-changes-the-economics-not-the-authority), [Evidence model](../core-concepts/evidence-model.md).

## Related decisions

- [ADR-0003](0003-functional-transformations.md), [ADR-0005](0005-validate-at-transformation-boundaries.md), [ADR-0010](0010-architecture-models-as-evidence.md), [ADR-0011](0011-preserve-provenance.md)
- Historical: H-000, H-018, H-020, H-023, H-025, H-035, H-037 (see [Historical decision records](history.md))
