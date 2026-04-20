---
name: correctness-policy
description: Establishes or upgrades a codebase correctness policy around trusted domain values, opaque/branded types, validated constructors, immutability, and durable enforcement. Use when you want to codify these rules in docs, config, templates, or ratchets.
---

# Correctness Policy

Read these references before making or proposing changes:
- [trusted domain values](../../references/trusted-domain-values.md)
- [TypeScript strictness and enforcement](../../references/ts-strict-enforcement.md)

## Goal

Turn the trusted-domain-values pattern into durable project policy rather than an informal preference.

Required policy points:
- Treat invariant-bearing domain values as opaque/branded immutable types.
- Separate untrusted/raw, trusted/validated, and serialized forms.
- Require constructors/parsers to own validation. Using Zod inside a parser/builder is encouraged, but callers should depend on the exported constructor/parser rather than the schema directly.
- Prevent direct construction of trusted types outside approved modules, except for rare explicit `unsafe*` escape hatches.
- Prefer readonly / immutable validated values; updates should create new validated values.
- Ban or severely restrict unsafe casts, especially `as unknown as` and assertions into branded types.
- Apply the policy gradually, but make it the default for new code.
- Prefer high-value starting targets such as domain IDs, normalized strings, money/units, geometry/value objects, and external identifiers.
- Keep historical and serialized data replay-safe; branded guarantees must be re-established at boundaries rather than assumed across persistence.

## Workflow

1. Inspect the repo's architecture, contribution docs, TS/lint setup, and any existing safety policy.
2. Classify the current state:
   - docs only
   - docs plus conventions
   - partial enforcement
   - ratcheted enforcement
3. Implement or propose the smallest durable next step. Prefer:
   - contributor docs or ADRs
   - a standard branded/opaque-type utility or module pattern
   - lint rules against unsafe construction or unsafe casts
   - boundary parsing guidance
   - CI ratchets, templates, and review checklists
4. Keep the policy explicit:
   - raw/untrusted vs trusted vs serialized values
   - constructors/parsers own validation
   - trusted values are readonly / immutable
   - trusted types cannot be created outside approved modules except for explicit `unsafe*` escape hatches
5. Update project docs/config where needed.
6. For event-sourced or persistence-heavy systems, document that brands do not survive persistence and must be re-established at boundaries.
7. End with:
   - what changed
   - how the policy is enforced
   - known exceptions
   - what remains as gradual follow-up work

## Deliverables

- updated docs/ADR or contributor guidance
- config/lint/ratchet changes if in scope
- optional starter utilities, templates, or examples
- a short summary of what changed, how enforcement works, and what follows next

## Non-goals

- fake safety through unchecked casts
- big-bang rewrites of the whole codebase
- mutable trusted value objects
