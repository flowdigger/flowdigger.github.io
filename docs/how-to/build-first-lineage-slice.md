# Build First Lineage Slice

## Purpose

Produce first useful lineage result from one legacy flow.

## Audience

Developers and technical analysts.

## Inputs

- One bounded process or report flow
- Source SQL or transformation code
- Table and field names

## Steps

1. Select one start table and one target report field.
2. Collect transformation artifacts between source and target.
3. Normalize artifacts into one intermediate representation.
4. Derive lineage links at table and field level.
5. Record provenance for each derived link.
6. Publish one lineage view and review with domain owner.

## Output

One validated lineage slice with traceable provenance.

## Next

Run [Validate lineage quality](validate-lineage-quality.md).

