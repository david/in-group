# in-group

Local pi package for reusable skills around correctness policy and TypeScript strictness.

## Included resources

### Skills
- `/skill:correctness-policy`
- `/skill:ts-strict`

## Project loading

This repo loads the package through `.pi/settings.json`.

Equivalent manual install command:

```bash
pi install -l ./packages/in-group
```

## Design goals

- Treat invariant-bearing domain values as trusted types, not plain primitives
- Create trusted values only through validated constructors/parsers
- Keep validated values immutable
- Make the policy durable through docs, lint, boundaries, and ratchets
