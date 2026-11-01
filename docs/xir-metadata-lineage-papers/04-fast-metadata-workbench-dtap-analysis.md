# A Fast Metadata Workbench for DTAP Analysis

## Abstract

A lightweight service that implements the useful subset of the Collibra REST contract over Spring Boot and Neo4j can serve a different purpose from the governed enterprise catalog. It can receive generated metadata immediately from build pipelines, support consumer and metadata-pipeline testing, compare lineage across multiple development, test, acceptance and production environments, and provide a graph foundation for impact and root-cause investigation. The aim is not to clone Collibra. It is to create a fast operational projection of evidence-backed metadata while retaining Collibra compatibility at the boundary.

## 1. Product boundary

The proposed architecture is:

```text
Authoritative artifacts
  ├─ ArchiMate
  ├─ configuration
  ├─ PL/SQL
  ├─ Java
  ├─ Perl
  └─ database definitions
          ↓
     XIR extraction
          ↓
 deterministic derivation
          ↓
 metadata / lineage fragments
          ↓
 resolution and stitching
          ↓
        Neo4j
          ↓
 Spring Boot service
          ↓
 Collibra-compatible REST interface
```

Responsibilities stay separate:

1. XIR represents observations.
2. XSLT derives metadata.
3. Neo4j stores and queries connected assertions.
4. Spring Boot exposes a consumption contract.
5. Collibra remains a possible governed publication target.

## 2. Do not emulate all of Collibra

The emulator should implement only the API surface required by real consumers and tests. Full platform emulation would create a permanent compatibility burden.

A practical initial surface can include:

- assets and asset types;
- relations and relation types;
- attributes;
- communities and domains where consumers require them;
- search;
- lineage retrieval;
- bulk metadata publication and deletion.

Consumer-driven contract tests should define compatibility.

## 3. Immediate metadata availability

The workbench can receive metadata during the same build or deployment that changes the application.

```text
commit
  ↓
build
  ↓
derive metadata
  ↓
publish to workbench
  ↓
query immediately
```

This creates a fast feedback loop before or independently of publication to the governed enterprise platform.

## 4. End-to-end metadata testing

A test can start with source and assert against the published graph.

Given:

```text
procedure A calls B
procedure B reads TABLE_X
```

Expected knowledge:

```text
A -> CALLS -> B
B -> READS -> TABLE_X
A -> TRANSITIVELY_DEPENDS_ON -> TABLE_X
```

This exercises the complete path:

```text
source
-> parser
-> XIR
-> XSLT
-> fragment
-> stitcher
-> Neo4j
-> REST API
```

The workbench therefore becomes valuable even if no human ever opens its user interface.

## 5. DTAP is not one D, one T and one A

A realistic landscape can contain multiple development, test and acceptance environments converging on production.

```text
D1  D2  D3
 \   |   /
  T1   T2
   \   /
   A1 A2
     |
     P
```

Each deployed environment can yield its own observed lineage projection.

Lineage becomes a function such as:

```text
lineage(
  application,
  environment,
  source revision,
  configuration revision,
  derivation version
)
```

Environment alone is insufficient. A difference between acceptance and production may be expected if their revisions differ.

## 6. Horizontal and promotion drift

### Horizontal drift

Compare equivalent environment instances:

```text
D1 <-> D2
T1 <-> T2
A1 <-> A2
```

This can reveal branch differences, experimental deployments, configuration drift and inconsistent dependencies.

### Promotion drift

For artifacts intended to move together:

```text
D2 -> T1 -> A1 -> P
```

compare the derived knowledge at each stage.

Differences can be classified as:

- added relationship;
- removed relationship;
- changed target;
- changed transformation;
- changed component binding;
- changed table or column dependency.

The comparison must first establish which revisions were expected to be equivalent.

## 7. Identity and observations

Do not model D1, A1 and P as unrelated copies of the enterprise graph. Separate logical identity from observed state.

```text
              logical Procedure A
              /        |        \
             /         |         \
        D1 observation A1 observation P observation
             |         |         |
          revision   revision   revision
```

This supports questions such as:

- Which production dependencies were absent in acceptance?
- Which D environment currently resembles P?
- What lineage changed between revisions X and Y?
- Is a difference caused by code, configuration or environment binding?
- What downstream impact is introduced by a proposed promotion?

## 8. Impact analysis

Impact analysis is naturally graph-oriented. Starting from a changed entity, traverse dependency relationships and retain the paths that justify the result.

An impact result should include:

- changed entity;
- affected entity;
- dependency path;
- application component;
- source evidence;
- source revision;
- derivation version.

The important output is not merely a list of affected assets. It is an explainable path from the change to the impact.

## 9. Root-cause analysis

Static lineage identifies possible causal paths, not necessarily the cause of a runtime failure.

A stronger model combines:

```text
static dependency knowledge
          +
runtime event evidence
          +
change history
          ↓
root-cause candidates
```

The graph should distinguish:

- directly observed fact;
- deterministically derived dependency;
- runtime correlation;
- root-cause hypothesis;
- confirmed cause.

This prevents an inference from silently becoming a fact.

## 10. Three graph layers

A useful separation is:

```text
1. Evidence graph
   What was directly observed?

2. Derived knowledge graph
   What was calculated from those observations?

3. Published governance view
   What should consumers see through the Collibra-compatible interface?
```

For example:

```text
Evidence:
procedure B contains SELECT from TABLE_X

Derived:
B READS TABLE_X

Published:
Business Preprocessor uses Customer Data Store
```

These statements have different origins and should not be conflated.

## 11. Main failure modes

### Internal model becomes Collibra-shaped

The Collibra REST model must remain an adapter. The canonical internal model should express the knowledge needed by the derivation architecture.

### Neo4j becomes the source of truth

Neo4j is a queryable projection. Derived regions should be rebuildable from evidence rather than repaired manually.

### Provenance disappears during stitching

A stitched edge must retain the fragments and evidence that justify it.

### Environment data is compared without revision context

A lineage difference is not drift until the compared observations are expected to represent equivalent versions.

### Static analysis is called root-cause analysis

Static dependencies provide candidate paths. Runtime evidence is needed to identify which path was active in an incident.

## Conclusion

The useful product is not a miniature Collibra. It is a fast metadata workbench with a Collibra-compatible boundary.

Its value comes from immediate publication of build-produced metadata, reproducible graph construction, contract testing, DTAP lineage comparison, explainable impact analysis and evidence-aware root-cause investigation.

This creates a complementary relationship:

```text
engineering evidence -> fast workbench -> governed publication
```

The workbench optimizes for speed, reproducibility and analysis. Collibra can remain the governed enterprise consumption layer.
