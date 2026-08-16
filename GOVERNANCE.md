# LGP Governance

LGP is developed as an open protocol project.

## Principles

1. Protocol semantics are public.
2. No commercial implementation is privileged by the specification.
3. Provider internals are outside protocol scope.
4. Changes should be justified by interoperability or implementation evidence.
5. Core should remain small.
6. Optional capabilities should prefer explicit extensions over Core expansion.
7. Public review should happen before a draft becomes stable.

## Document Lifecycle

```text
Draft -> Review -> Candidate -> Final -> Deprecated
```

The current LGP specification is Draft and may change incompatibly.

## Decision Process

Substantial protocol changes should be proposed through a numbered RFC document or public issue before being merged into a stable specification.

Maintainers may merge editorial changes directly when they do not alter semantics.

## Independence

LFE may implement LGP and N10Y may consume LGP, but neither product owns the protocol contract.

The protocol must remain implementable without either product.
