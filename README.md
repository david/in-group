# in-group

`in-group` is a public pi package with reusable skills for introducing and enforcing trusted domain values, stronger TypeScript safety, and correctness-oriented engineering policy.

## Included skills

- `/skill:correctness-policy`
  - codify a durable correctness policy around trusted values, constructors/parsers, immutability, lint, boundaries, and ratchets
- `/skill:ts-strict`
  - improve TypeScript strictness in a durable way through trusted types, boundary parsing, lint restrictions, and CI ratchets
- `/skill:trusted-types-starter`
  - add one concrete starter trusted-type module and wire it into a real boundary and call path

## Install

### From GitHub

```bash
pi install git:github.com/david/in-group
```

Project-local install:

```bash
pi install -l git:github.com/david/in-group
```

### From a local checkout

```bash
pi install -l ./packages/in-group
```

Or via `.pi/settings.json`:

```json
{
  "packages": ["../packages/in-group"]
}
```

## What these skills are for

These skills are built around a few durable rules:

- invariant-bearing domain values should not stay plain primitives
- trusted values should be created only through validated constructors/parsers
- trusted values should be immutable
- raw, trusted, and serialized forms should stay distinct
- correctness should be enforced through tooling and boundaries, not only style guidance

## Good use cases

- introduce branded/opaque domain IDs
- add a trusted value object for money, units, or geometry
- write an ADR or contributor policy for trusted values
- tighten TypeScript/lint/CI rules so unsafe patterns stop regressing
- create one starter pattern a team can copy across the codebase

## References

- `references/trusted-domain-values.md`
- `references/ts-strict-enforcement.md`
- `references/trusted-types-starter.md`

## License

MIT
