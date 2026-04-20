---
name: trusted-types-starter
description: Scaffolds or introduces trusted domain value patterns using opaque/branded immutable types, validated constructors/parsers, and boundary parsing. Use when you want a concrete starting point for one domain concept or a small starter kit in a codebase.
---

# Trusted Types Starter

Read these references before making or proposing changes:
- [trusted domain values](../../references/trusted-domain-values.md)
- [TypeScript strictness and enforcement](../../references/ts-strict-enforcement.md)
- [trusted types starter pattern](../../references/trusted-types-starter.md)

## Goal

Introduce one small but durable slice of the trusted-domain-values approach so the repo has a concrete pattern to copy.

## Best-fit use cases

- a domain ID should stop being a plain string
- an invariant-bearing value should stop being a plain primitive or mutable object
- a team wants a starter module pattern before rolling the policy out further
- a project needs one end-to-end example of boundary parsing -> trusted value -> internal API usage

## Workflow

1. Identify one high-value target:
   - domain ID
   - normalized string
   - money/unit value
   - geometry/value object
   - external identifier
2. Inspect the current boundary where the raw value enters the system.
3. Introduce a focused module that exports:
   - the trusted opaque/branded type
   - the constructor/parser
   - optional `isX` / `assertX` / serializer helpers
4. Keep the trusted value immutable.
5. Update one or more internal APIs to accept the trusted type instead of the raw primitive.
6. Add tests that prove:
   - invalid raw values are rejected
   - normalized valid values are accepted
   - callers can only obtain the trusted type through the constructor/parser path
7. If appropriate, add the smallest supporting doc or lint policy so the pattern is durable.

## Requirements

- Prefer Zod inside the constructor/parser for shape parsing and normalization.
- Keep callers dependent on the exported constructor/parser, not the raw schema.
- Do not rely on unchecked casts outside tightly controlled modules.
- Do not pretend brands survive persistence; re-establish them at boundaries.
- Prefer one excellent example over a broad partial migration.

## Deliverables

- one starter trusted-type module
- one updated call path using it
- tests for valid/invalid behavior
- brief notes on how to copy the pattern elsewhere in the repo

## Non-goals

- branding every primitive in one pass
- mutable trusted objects
- fake guarantees through `as unknown as`
