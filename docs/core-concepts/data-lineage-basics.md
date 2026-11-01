# Data Lineage in Legacy Systems

## Purpose

Define core terms with low ambiguity.

## Audience

All roles.

## Definitions

- **Data lineage**: trace of data origin, transformations, and destinations.
- **Legacy system**: older production system with mixed technologies and hidden logic.
- **Evidence**: technical artifact that supports lineage fact. Example: SQL statement, mapping rule, source code.
- **Derived fact**: lineage fact produced from evidence by deterministic logic.

## What fails in many programs

- Teams maintain lineage manually.
- Documentation drifts from implementation.
- Impact analysis depends on individual memory.

## What works better

- Derive facts from real technical evidence.
- Keep derivation repeatable.
- Publish role-specific views from one trusted base.

## Next step

Read [Evidence model](evidence-model.md).

