# From Lineage to Reprojection: Recovering Specifications from Running Enterprises

## Abstract

Deep lineage work eventually reaches beyond diagrams. To explain a lineage edge defensibly, the analysis must return to code, configuration, schemas, architecture and other authoritative artifacts. Once these observations can be connected and transformed deterministically, lineage becomes only one projection of a broader knowledge model. The same machinery can recover evidence-backed specifications from existing applications. Modernization can then be viewed as reprojection: recover the knowledge embodied in an old implementation, distinguish required semantics from accidental implementation choices, and project the retained knowledge onto a new architecture.

## 1. Following lineage back to its cause

A lineage viewer shows relationships. A defensible lineage system must answer where those relationships came from.

Following that question produces a characteristic sequence:

```text
want lineage
   ↓
need relationships
   ↓
need to derive relationships from real artifacts
   ↓
need heterogeneous parsers
   ↓
need a common structural representation
   ↓
need deterministic transformations
   ↓
need provenance
   ↓
need stitching and resolution
   ↓
need querying and visualization
```

At the end of this sequence, the diagram is only the final projection.

## 2. Applications as projections

An application can be understood as one realization of underlying knowledge under a particular set of technological and operational constraints.

```text
                  application knowledge
                    /             \
                   /               \
          legacy projection     modern projection
                 ↓                   ↓
          PL/SQL / Perl          Java / APIs
          batch                  events
          legacy UI              web UI
```

This does not imply that an abstract model fully captures an application. It provides a disciplined way to separate knowledge that must survive modernization from implementation choices that need not.

## 3. Reverse engineering as specification recovery

Traditional development is often represented as:

```text
specification -> implementation
```

Deterministic analysis provides part of the inverse:

```text
implementation -> evidence-backed derived specification
```

For example:

```text
PL/SQL
  ↓
procedure and table relationships
  ↓
data-flow model
  ↓
lineage
```

A derived statement such as "procedure A obtains data from B, transforms it and writes C" can act as a requirement against a replacement implementation.

The result is a loop:

```text
implementation
     ↓ derive
specification
     ↓ reproject
implementation
```

## 4. Modernization as reprojection

Modernization is then not fundamentally code translation.

A more useful definition is:

> Modernization is recovery of an evidence-backed specification from an existing projection, followed by projection of the retained knowledge onto a new architecture.

The recovery stage must distinguish required properties from accidental properties:

```text
old application
      ↓
recover knowledge
      ├─ business semantics       preserve
      ├─ data meaning            preserve
      ├─ regulatory controls     preserve
      ├─ lineage/evidence        preserve
      ├─ accidental architecture discard where safe
      └─ obsolete constraints    discard where justified
      ↓
reproject
      ↓
modern application
```

Human architecture remains essential because deterministic observation can tell us what exists, but not automatically which observed properties should remain requirements.

## 5. Multiple observations of the same system

The broader model is not restricted to code.

```text
                     real system
                         │
       ┌─────────────────┼─────────────────┐
       ↓                 ↓                 ↓
    ArchiMate        configuration        code
       │                 │                 │
       └────────── observations ───────────┘
                         ↓
                  evidence-backed links
                         ↓
                    derived knowledge
                         ↓
       ┌────────────┬────┼─────┬────────────┐
       ↓            ↓          ↓            ↓
    lineage       impact    documentation  modernization
```

ArchiMate, source code and configuration are not competing descriptions. They are projections with different purposes and authority boundaries.

## 6. Lineage as an answer, and an answer as a specification

Lineage is one answer produced by the system. Once the answer is explicit, reproducible and evidence-backed, it can become input to another process.

```text
evidence
   ↓
derive
   ↓
lineage / dependency model
   ↓
verification requirement
   ↓
new implementation
```

This is particularly useful for modernization and regression verification. The derived lineage of a new implementation can be compared against the properties selected from the old implementation.

## Conclusion

Lineage can be the entry point into a more general architecture for recovering enterprise knowledge. Once relationships are derived from authoritative artifacts, lineage ceases to be the final goal. It becomes one projection of evidence-backed knowledge.

This reframes modernization as controlled reprojection rather than code conversion. The legacy application is evidence. Deterministic derivation helps recover a specification. Architecture decides what must survive. A new system becomes another projection of the retained knowledge.
