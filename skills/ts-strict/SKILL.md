---
name: ts-strict
description: Raises TypeScript strictness in a durable way through trusted domain values, boundary parsing, lint restrictions, and CI ratchets. Use when you want to improve TS safety without relying on one-off cleanups.
---

# TypeScript Strictness

Read these references before making or proposing changes:
- [TypeScript strictness and enforcement](../../references/ts-strict-enforcement.md)
- [trusted domain values](../../references/trusted-domain-values.md)

## Goal

Improve TypeScript safety in a way that stays enforced after the current change lands.

Required outcomes:
- Preserve or enable strict compiler options where practical.
- Use opaque/branded types for invariant-bearing domain values.
- Parse/validate at boundaries, then pass trusted types inward.
- Keep validated values immutable.
- Reduce or ban `any`, unsafe casts, double assertions, and non-null assertions.
- Add or tighten lint/CI ratchets so new unsafe patterns cannot drift back in.
- Prefer gradual migration with a hard rule for new code.
- If you introduce trusted types, ensure they can only be created through validated constructors/parsers.

## Workflow

1. Inspect current `tsconfig`, lint rules, boundary checks, and CI/typecheck commands.
2. Identify the highest-leverage gaps, such as:
   - `any`
   - double assertions
   - non-null assertions masking modeling gaps
   - raw transport/DB values leaking into domain APIs
   - missing trusted types for invariant-bearing values
3. Identify the smallest high-leverage changes that improve strictness without destabilizing the repo.
4. Prefer durable changes over one-off fixes:
   - stricter compiler settings where practical
   - branded/opaque trusted types for important values
   - constructor/parser modules at boundaries
   - lint rules that prevent unsafe reintroduction
   - ratchets that keep the codebase from sliding backward
5. Add or update docs/policy so the rules are explicit.
6. Keep the migration strategy explicit:
   - new code follows the stricter rule immediately
   - touched code improves opportunistically
   - CI ratchets protect progress
7. Finish with:
   - current gaps
   - changes made
   - enforcement added
   - next ratchet step

## Deliverables

- TS/lint/config improvements
- supporting docs/policy updates
- optional starter utilities/examples for trusted types
- a short summary of current gaps, changes made, enforcement added, and the next ratchet step

## Non-goals

- papering over unsafe code with assertions
- introducing strictness that the repo cannot sustain
- treating historical serialized data as already trusted
