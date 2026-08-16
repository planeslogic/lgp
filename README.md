# Logical Gate Protocol (LGP)

**An open protocol for logical resolution as a first-class pipeline primitive.**

LGP defines a vendor-neutral contract for projecting source-owned state into logical form and resolving logical conditions back into source-owned coordinates.

```text
Source-owned State
        |
        | coordinate + logical state
        v
   Logical Gate
        |
        | logical condition
        v
   Coordinate Set
        |
        v
Application / Pipeline continues
```

LGP is open by design. It is not tied to a specific database, API gateway, programming language, runtime, storage engine, or Logical Gate implementation.

## Why LGP Exists

LGP emerged from research and engineering work on **LFE (Logical Filter Engine)**.

During that work, a recurring architectural pattern became clear: applications already own their records, payloads, identities, and business state, while logical eligibility can be treated as a separate concern.

The general pattern is simple:

1. a source owns coordinates and authoritative state;
2. a logical representation is associated with those coordinates;
3. a logical condition is evaluated;
4. matching source coordinates are returned;
5. the application continues processing using its own source systems.

LFE was one implementation context in which this pattern became visible. LGP extracts the interoperable architectural contract from that observation and opens it for public evaluation.

LGP does **not** standardize LFE internals.
LGP does **not** require LFE.
LGP does **not** claim ownership of logical filtering as a concept.

The question LGP asks is narrower:

> Should logical resolution be treated as a first-class, interoperable pipeline primitive?

## What Is a Logical Gate?

A **Logical Gate** evaluates logical conditions and returns matching source coordinates.

```text
Logical Condition
       |
       v
  Logical Gate
       |
       v
 Coordinate Set
```

The source remains authoritative for record identity, payload, business state, and lifecycle.

A Logical Gate answers:

> Which source coordinates satisfy this logical condition?

The surrounding application decides what to do with those coordinates.

## LGP Is Not a Database Protocol

A datastore and a Logical Gate answer different questions.

```text
Datastore:
Where is the data and how do I retrieve it?

Logical Gate:
Which source coordinates satisfy this logical condition?
```

A basic Logical Gate provider can be implemented using familiar infrastructure, including a relational database, document database, in-memory evaluator, or application service.

For example, a database-backed provider could evaluate a condition with an ordinary query and return only the matching source identifiers.

LGP intentionally does not prescribe how providers implement logical resolution internally.

## Core Direction

The initial LGP Core surface is intentionally small:

```text
DEFINE
ADD(coordinate, logical_state)
UPDATE(coordinate, logical_state)
DELETE(coordinate)
RESOLVE(condition) -> CoordinateSet
```

See [`specs/LGP-0001-core.md`](specs/LGP-0001-core.md).

## Open Protocol

LGP is intended to be implementable and consumable by anyone.

```text
                 LGP
                  |
       +----------+----------+
       |          |          |
       v          v          v
     LFE       Provider B  Provider C
```

No implementation is privileged by the protocol.

The specification defines observable Logical Gate semantics. It does not discuss or recommend specialized physical implementations.

## Why Open This Now?

The protocol is being published early so the architectural idea can evolve in public.

We want developers, infrastructure engineers, runtime authors, database engineers, API gateway developers, researchers, and other contributors to challenge the model.

Questions include:

- Is Logical Gate a useful standalone architectural primitive?
- What is the smallest interoperable Core contract?
- What should a Coordinate represent?
- Which logical operators belong in Core?
- Which capabilities should remain extensions?
- How should local and remote providers expose equivalent semantics?
- What should conformance mean?
- Where does a Logical Gate belong in different kinds of pipelines?

The protocol should evolve through implementation evidence and interoperability, not assumptions from one implementation.

## Repository Structure

```text
.
├── README.md
├── LICENSE
├── GOVERNANCE.md
├── CONTRIBUTING.md
├── CHANGELOG.md
├── specs/
│   └── LGP-0001-core.md
├── rfcs/
│   └── README.md
├── schemas/
│   └── README.md
└── conformance/
    └── README.md
```

## Status

LGP is currently an **early public draft**.

Nothing in the current draft should be treated as a final standard. Breaking changes are expected while the Core contract is being evaluated.

## License

This repository is released under the Apache License 2.0. See [`LICENSE`](LICENSE).
