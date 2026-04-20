# Trusted Types Starter Pattern

Use this reference when introducing trusted domain values into a codebase.

## Goal

Make invalid states hard to express by:

1. keeping raw values at boundaries
2. validating once in a constructor/parser
3. returning a trusted opaque/branded type
4. keeping trusted values immutable
5. preventing arbitrary construction elsewhere

## Module pattern

Each invariant-bearing concept should live in a focused module.

Exports:
- the trusted type
- the constructor/parser
- optional `isX` / `assertX`
- optional serializer helpers

Do not export implementation details that let callers forge the trusted value.

## Example: primitive-like trusted type

```ts
import { z } from "zod";

type Brand<T, Name extends string> = T & { readonly __brand: Name };

export type Email = Brand<string, "Email">;

const EmailSchema = z
  .string()
  .trim()
  .toLowerCase()
  .email();

export function parseEmail(input: unknown): Email {
  return EmailSchema.parse(input) as Email;
}
```

## Example: structured trusted type

```ts
import { z } from "zod";

type Brand<T, Name extends string> = T & { readonly __brand: Name };

const RawMoneySchema = z.object({
  currency: z.literal("USD"),
  cents: z.number().int().nonnegative(),
});

type RawMoney = z.infer<typeof RawMoneySchema>;
export type Money = Readonly<Brand<RawMoney, "Money">>;

export function parseMoney(input: unknown): Money {
  return RawMoneySchema.parse(input) as Money;
}
```

## Example: contextual parser

Use a parser/builder when validity depends on context.

```ts
import { z } from "zod";

const RectSchema = z.object({
  x: z.number(),
  y: z.number(),
  width: z.number().positive(),
  height: z.number().positive(),
});

type Brand<T, Name extends string> = T & { readonly __brand: Name };
export type ValidLocationRect = Readonly<Brand<z.infer<typeof RectSchema>, "ValidLocationRect">>;

export function parseValidLocationRect(
  input: unknown,
  minimums: { minWidth: number; minHeight: number },
): ValidLocationRect {
  const rect = RectSchema.parse(input);
  if (rect.width < minimums.minWidth || rect.height < minimums.minHeight) {
    throw new Error("dimensions_too_small");
  }
  return rect as ValidLocationRect;
}
```

## Rules

- Use Zod for shape parsing and normalization.
- Put domain-specific invariant checks in the constructor/parser.
- Keep trusted values readonly.
- Re-parse after persistence; brands do not survive DB/JSON/event storage.
- Restrict `as Brand` and `as unknown as T` to tightly controlled modules.

## Good first targets

- IDs (`UserId`, `OrgId`, `PacketSessionId`)
- normalized strings (`Email`, `PhoneNumber`, `Slug`)
- money/units/percentages
- geometry and small value objects
- external IDs

## Rollout checklist

- Add one trusted type module for a high-value concept.
- Change one domain API to accept the trusted type.
- Add docs explaining raw vs trusted vs serialized.
- Add lint restrictions against unsafe casts.
- Add a ratchet so new code follows the rule.
