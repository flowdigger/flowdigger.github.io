# Build First Lineage Slice

!!! warning "Under construction"
    Examples are not yet fully runnable. This page outlines an intended exercise, not verified setup or execution instructions.

## Purpose

Explore one bounded process or report flow with source SQL or transformation code and known table and field names.

## Audience

Developers and technical analysts.

## Inputs

A bounded process, its source artifacts, and known table and field names.

## Steps

1. Select one start table and one target report field.
2. Collect transformation artifacts between source and target.
3. Examine how observations, direct flows, merge and stitch relate to those artifacts in the [pipeline](../adr/0007-explicit-pipeline-phases.md).
4. Review available evidence and document missing links with a domain owner.

## Setup, configuration and troubleshooting

Verified end-to-end setup and troubleshooting instructions are not available yet. The [lineage plugin](https://github.com/flowdigger/gradle-lineage-plugin) documents its current profiles and extension points. Check source coverage and open issues there before relying on an example; avoid assuming a particular command or configuration works.

## Output

A scoped exercise and a list of evidence gaps, not a guaranteed lineage result. End-to-end provenance is not implemented; see [ADR-0011](../adr/0011-preserve-provenance.md).

## Next

Review [lineage quality criteria](validate-lineage-quality.md).
