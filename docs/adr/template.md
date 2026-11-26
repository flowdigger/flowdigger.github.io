# ADR template

Copy this template to `docs/adr/NNNN-short-decision-title.md`, use the next free number, and add the record to the [ADR index](index.md) and to the navigation in `mkdocs.yml`.

## Writing rules

- The title states the decision, not a product. Name products in **Current realization**.
- Do not invent dates, motives or rejected alternatives. If something is not evidenced, say so.
- Link every claim in **Current realization** to evidence in a Flowdigger repository or on this site.
- Use *Proposed* when the decision is only partly realized, and list the gap.
- Name present-day alternatives that were not evaluated as such.
- Use *processing language* for XSLT, XQuery, Schematron and SEL. Use *vocabulary* only for semantic or domain vocabularies.

## Template

```markdown
# ADR-NNNN: <Decision stated as an imperative>

- **Status:** Proposed | Accepted | Superseded by ADR-NNNN
- **Nature:** <Retrospective architecture baseline | New decision>

## Context

<Problem and forces. What makes a decision necessary?>

## Decision

<The decision in one or two paragraphs.>

## Rationale

<Why this decision. Bullet points.>

## Current realization

<How the implementation realizes the decision today. Repositories, tasks, files.>

## Consequences

<Positive, negative, and known gaps.>

## Alternatives and earlier approaches

<Alternatives with evidence of evaluation; earlier approaches; present-day alternatives marked as such.>

## Evidence

<Links to code, tests, and documentation.>

## Related decisions

<Links to other ADRs and historical records.>
```
