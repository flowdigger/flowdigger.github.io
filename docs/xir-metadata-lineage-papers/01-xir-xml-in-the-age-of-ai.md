# XIR: XML in the Age of AI

## Abstract

XIR can be understood as XML revisited under a different set of engineering constraints. XML succeeded by combining a strong structural information model with a concrete angle-bracket syntax suited to the Web, SGML continuity, human authoring, validation, streaming and transformation. XIR keeps the useful structural ideas while providing a compact S-expression representation that fits AI consumption and generation well. The practical value does not depend on replacing XML. A SAX gateway makes XML and XIR cheap to bridge, allowing mature XML infrastructure to remain in place.

## 1. The proposition

The simplest description of XIR is:

> XIR is XML in the age of AI.

This is not a claim that XML chose the wrong syntax. XML was optimized for the problems and adoption constraints of the late 1990s. XIR revisits the representation boundary because AI changes the economics of representation.

The underlying structural concerns remain familiar:

- elements and names;
- attributes;
- ordered content;
- text;
- processing instructions and comments;
- schemas and constraints;
- streaming;
- deterministic transformation.

XIR changes the concrete representation, not the need for structure.

## 2. Why representation matters again

For traditional XML, angle brackets had large practical advantages: familiarity through HTML, continuity with SGML, human readability and a rapidly growing tooling ecosystem.

AI introduces different optimization pressures:

- compact token usage;
- regular structural patterns;
- low syntactic noise;
- easy generation;
- easy consumption;
- explicit hierarchy;
- deterministic interchange with existing structured-data systems.

XIR therefore need not compete with XML. It can be another projection of the same structured information.

```text
XML representation
       │
       │ SAX
       ▼
     XIR-SAX
       │
       ▼
XIR representation
       │
       ▼
AI / derivation / transformation
```

## 3. XIR-SAX as the low-cost gateway

The architectural importance of `xir-sax` is that adoption does not require migration of the XML estate.

Existing XML parsers already expose a stable event abstraction:

```text
start element
attributes
content
end element
```

XIR maps naturally onto the same structural sequence. This permits a simple principle:

> Keep XML where XML works. Convert to XIR where XIR is advantageous.

The reverse path can remain available as well:

```text
XML -> SAX -> XIR -> processing -> XIR -> SAX/JAXP -> XML
```

The gateway therefore limits coupling between representation choice and the surrounding platform.

## 4. Beyond XML: grammars as adapters

The same SAX boundary becomes useful for programming languages and DSLs.

An ANTLR G4 parser produces a parse tree. A grammar-aware tree walker can expose that tree as SAX events. Once at the SAX boundary, the downstream pipeline becomes language independent.

```text
source
  ↓
ANTLR G4 parser
  ↓
parse tree
  ↓
language-specific SAX walker
  ↓
SAX event stream
  ↓
XIR
```

The language-specific responsibility is narrow: project the parse tree faithfully into a hierarchical event stream. The rest of the tooling can operate on XIR.

## 5. Schema from grammar

A second derivation starts from the grammar itself. Inspired by the general schema-conversion ideas demonstrated by Trang, a G4 grammar can be analyzed to derive a structural schema for the resulting representation.

```text
                    G4 grammar
                    /        \
                   /          \
              parser         schema
                ↓               ↓
             source             │
                ↓               │
           parse tree           │
                ↓               │
             SAX/XIR <----------┘
```

This gives AI two complementary artifacts:

1. an XIR instance describing a particular program;
2. a schema describing the structures that can occur.

The schema reduces structural inference. It does not prove semantics.

## 6. Semantics and evidence

Grammar rule names can provide useful semantic clues. A grammar containing names such as `procedureCall`, `argumentList`, `tableReference` or `qualifiedName` carries vocabulary chosen by the grammar author.

The evidence layers can be seen as:

```text
tokens             lexical evidence
  ↓
grammar structure  syntactic evidence
  ↓
rule names          weak semantic evidence
  ↓
schema              structural constraints
  ↓
XIR instances       concrete usage evidence
  ↓
AI                  interpretation
```

This distinction is important. XIR does not magically create semantics. It preserves evidence in a form from which semantics can be derived, asserted or reasoned about.

## 7. Raw and semantic representations

A raw grammar projection should not automatically become the enterprise semantic model. Grammars contain implementation details such as precedence rules, helper productions and punctuation.

A robust pipeline therefore separates stages:

```text
source
  ↓
grammar parse tree
  ↓
structural XIR
  ↓
deterministic normalization
  ↓
semantic XIR / derived models
```

This separation preserves evidence while preventing parser design decisions from leaking into every downstream consumer.

## Conclusion

XIR is most useful when treated neither as an XML replacement nor merely as a new syntax. It is a compact structural representation that can sit behind XML, ANTLR grammars, source code and other structured artifacts.

Its practical strength comes from cheap gateways. SAX connects it to the XML ecosystem. G4 tree walkers connect it to programming languages. Grammar-derived schemas give machines explicit structural context. Together these mechanisms make XIR a useful substrate for deterministic derivation and AI-assisted interpretation without discarding the mature ideas that made XML effective.
