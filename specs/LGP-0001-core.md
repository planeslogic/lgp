# LGP-0001 - Logical Gate Protocol Core

- **Status:** Draft
- **Version:** 0.1
- **Category:** Core Protocol

## Abstract

The Logical Gate Protocol (LGP) defines a vendor-neutral interface for associating source-owned coordinates with logical state and resolving logical conditions back into source-owned coordinates.

LGP defines observable protocol semantics only. It does not define provider internals, storage architecture, execution algorithms, or deployment topology.

## 1. Terminology

### Source

The authoritative owner of application records and state.

The Source owns record identity, coordinates, payload, business state, and lifecycle.

### Coordinate

A stable identifier owned by the Source and supplied to the Logical Gate.

A provider MUST preserve the supplied coordinate as the reference returned by resolution operations.

A provider MUST NOT silently substitute a provider-owned identity for a Source coordinate.

### Logical State

A logical representation associated with a Source coordinate.

Logical State is not required to contain the Source payload.

### Logical Condition

An expression evaluated against Logical State.

### Coordinate Set

The set of Source coordinates satisfying a Logical Condition.

### Logical Gate

A provider implementing the LGP contract.

### Consumer

A system using a Logical Gate.

## 2. Architectural Model

```text
Source
  |
  | coordinate + logical state
  v
Logical Gate
  |
  | resolve(condition)
  v
Coordinate Set
  |
  v
Consumer continues processing
```

LGP separates logical resolution from source execution.

A Logical Gate determines which coordinates satisfy a logical condition.
The Consumer determines what happens next.

## 3. Core Operations

### 3.1 DEFINE

Defines logical dimensions or schema information required by a provider.

```text
DEFINE(definition)
```

Exact schema semantics remain open in Draft 0.1.

### 3.2 ADD

Associates logical state with a Source coordinate.

```text
ADD(coordinate, logical_state)
```

### 3.3 UPDATE

Changes logical state associated with an existing Source coordinate.

```text
UPDATE(coordinate, logical_state)
```

### 3.4 DELETE

Removes the Logical Gate representation associated with a Source coordinate.

```text
DELETE(coordinate)
```

DELETE MUST NOT imply deletion of the authoritative Source record.

### 3.5 RESOLVE

Evaluates a logical condition and returns matching Source coordinates.

```text
RESOLVE(condition) -> CoordinateSet
```

The returned values MUST preserve Source coordinate identity.

## 4. Mutation and Read Classes

The following operations are mutations:

```text
DEFINE
ADD
UPDATE
DELETE
```

The following operation is a read:

```text
RESOLVE
```

Future extensions MUST identify whether an operation mutates Logical Gate state.

## 5. Condition Model

Draft 0.1 defines a conceptual expression tree:

```text
Condition :=
    Predicate
  | AND(Condition...)
  | OR(Condition...)
  | NOT(Condition)
```

A Predicate conceptually contains:

```text
field
operator
value
```

The minimum operator set is intentionally not frozen in Draft 0.1.

Providers MUST advertise the operators they support before Consumers depend on provider-specific operators.

## 6. Resolution Semantics

A successful RESOLVE operation returns a Coordinate Set representing Source coordinates that satisfy the supplied Logical Condition according to the provider's declared semantics.

Coordinate Set uses set semantics.

A provider MUST NOT intentionally return duplicate coordinates.

Core LGP does not assign ranking or priority semantics to result ordering.

Consumers MUST NOT infer ranking from Coordinate Set order unless an extension explicitly defines it.

## 7. Provider Independence

LGP MUST NOT require a specific:

- database;
- programming language;
- network transport;
- runtime;
- storage engine;
- deployment model;
- internal logical representation.

A provider MAY execute in-process or out-of-process, locally or remotely, provided observable Core semantics are preserved.

A basic provider can be implemented using conventional application or database technology.

## 8. Transport Independence

LGP Core defines semantics independently from transport.

Transport bindings are separate protocol documents.

Potential future bindings include HTTP, RPC, IPC, and language-native bindings.

Draft 0.1 does not define a canonical wire representation.

## 9. Capability Discovery

A provider MUST expose enough metadata for a Consumer to discover:

- supported LGP version;
- supported Core operations;
- supported operators;
- supported optional extensions.

The exact discovery representation is not frozen in Draft 0.1.

## 10. Datastore Independence

LGP is not a general datastore protocol.

A datastore answers how application state is persisted and retrieved.
A Logical Gate answers which Source coordinates satisfy a logical condition.

A Consumer MAY combine both roles in a pipeline:

```text
Logical Condition
      |
      v
Logical Gate
      |
      v
Coordinate Set
      |
      v
Datastore / Source
```

## 11. Error Semantics

Core error classes SHOULD distinguish at least:

```text
INVALID_REQUEST
INVALID_COORDINATE
INVALID_LOGICAL_STATE
INVALID_CONDITION
UNSUPPORTED_OPERATION
UNSUPPORTED_OPERATOR
NOT_FOUND
CONFLICT
UNAUTHORIZED
PROVIDER_UNAVAILABLE
INTERNAL_ERROR
```

Transport bindings MAY map these to transport-specific error representations.

## 12. Fail-Closed Requirement

Malformed logical conditions MUST fail closed.

A provider MUST NOT silently reinterpret an invalid condition as a less restrictive valid condition.

## 13. Extensions

Optional capabilities MUST be discoverable before use.

An extension MUST NOT silently redefine Core semantics.

Extension naming and registry governance remain open for a later draft.

## 14. Versioning

Breaking semantic changes require an explicit major protocol version transition.

Backward-compatible capability additions SHOULD use version negotiation or explicit extensions.

## 15. Security Considerations

Logical Gate results may participate in routing, policy, resource eligibility, or other security-sensitive decisions.

Providers MUST validate externally supplied logical state and conditions.

Remote bindings MUST separately define transport authentication, authorization, integrity, confidentiality, and replay considerations.

LGP Core does not define an authentication mechanism.

## 16. Privacy Considerations

Consumers SHOULD project only logical state required for resolution.

Providers that persist Logical State SHOULD document retention and deletion behavior.

## 17. Conformance Direction

An LGP Core Provider is expected to:

- accept Source-owned coordinates;
- preserve coordinate identity;
- support the frozen Core operation set for its declared protocol version;
- return Coordinate Sets;
- expose capability information;
- fail closed on malformed conditions;
- avoid requiring ownership of Source payload.

An LGP Core Consumer is expected to:

- treat results as Source coordinates;
- negotiate optional capabilities before use;
- avoid relying on provider internals;
- avoid inferring undefined ordering semantics;
- handle Core errors deterministically.

Formal conformance vectors will be defined separately.

## 18. Non-Goals

LGP Core does not standardize:

- payload storage;
- general database querying;
- traffic routing;
- load balancing;
- authentication;
- authorization;
- rate limiting;
- workflow execution;
- event buses;
- business rules;
- result ranking;
- provider implementation algorithms;
- commercial licensing;
- deployment architecture.

## 19. Research Origin and Protocol Independence

LGP emerged from architectural observations made while researching and developing LFE.

That origin does not make LGP an LFE-specific protocol.

LGP intentionally separates the public Logical Gate contract from any particular implementation's internal design.

The protocol defines observable behavior only.

## 20. Open Questions for Draft 0.2

The following are intentionally unresolved:

- canonical Coordinate representation;
- canonical logical scalar types;
- minimum Core operator set;
- DEFINE/schema semantics;
- canonical wire representation;
- batch mutation semantics;
- transaction boundaries;
- result streaming;
- extension naming;
- discovery representation;
- formal conformance vectors.

These should be resolved through public review and interoperability evidence rather than prematurely expanded Core scope.
