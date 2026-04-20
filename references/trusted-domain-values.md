# Trusted Domain Values

Use this reference when a project wants durable guarantees that important domain values are valid.

## Core rule

Values with business invariants should be represented by opaque/branded types, created only through validated constructors/parsers, and treated as immutable.

Practical guarantee:

> If a value typechecks as a trusted domain value, it must have passed the approved constructor/parser for this process and must not have been mutated since.

## Trusted vs untrusted

Every value should be treated as one of:

1. **Untrusted/raw** — request bodies, DB rows, event payload JSON, env vars, third-party API data
2. **Trusted/validated** — branded domain values created through approved constructors/parsers
3. **Serialized** — values written back to JSON, DB rows, event payloads, or network formats

Brands do not survive persistence. Serialized data becomes raw again until reparsed.

## Constructor/parser rule

Each invariant-bearing concept should own a small module that exports:

- the branded/opaque type
- the constructor/parser
- optional `isX` / `assertX`
- optional serializer/normalizer helpers

Callers should not construct trusted values directly and should not depend on raw schemas.

Using Zod inside the constructor/parser is encouraged. The durable rule is still: callers go through the exported constructor/parser.

## Immutability rule

Validated values must be immutable.

- Prefer `Readonly` value objects and leaf primitives.
- Updates should create new trusted values.
- Do not validate once and then mutate the object freely.

## Enforcement rule

For durability, adopt the strongest combination the project can sustain:

1. written contributor docs / ADR
2. a standard branded-type module pattern
3. boundary parsing
4. lint rules against unsafe assertions
5. architectural boundaries that keep raw transport/DB shapes out of domain code
6. explicit `unsafe*` escape hatches, isolated and reviewable
7. CI ratchets / review checklists / templates

## Unsafe-cast rule

The policy loses value if code can freely write:

- `as SomeBrand`
- `as unknown as SomeType`

Restrict these to narrow, explicit locations such as:

- constructor/parser modules
- carefully marked framework boundary adapters
- intentionally named `unsafe*` helpers
- tests, only when justified

## Good candidates for trusted types

High-value starting targets:

- domain IDs
- normalized strings like email or phone
- money, units, percentages, quantities
- geometry and coordinates
- time windows/date ranges
- external identifiers
- small value objects with real invariants

Avoid branding giant mutable data blobs or incidental DTOs.

## Event-sourced / persistence-heavy systems

Historical data may not satisfy current invariants.

- Keep replay compatibility exact.
- Re-establish trusted values at boundaries.
- Do not pretend old serialized data was always valid.
- Use migrations, quarantine, or explicit historical handling when invariants changed over time.

## Rollout guidance

- Make this the default for **new code**.
- Apply it gradually to touched code.
- Start with the most expensive bug classes first.
- Prefer a hard rule for new code plus a ratchet for legacy code.
