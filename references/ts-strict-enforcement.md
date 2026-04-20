# TypeScript Strictness and Enforcement

Use this reference when a project wants TypeScript safety to be durable rather than aspirational.

## Goal

Make the type system, lint rules, and project boundaries work together so unsafe patterns are hard to introduce and easy to detect.

## Recommended stack

1. strict compiler settings where practical
2. trusted domain values for invariant-bearing concepts
3. parsing/validation at boundaries
4. lint rules against unsafe casts and `any`
5. CI ratchets so regressions fail automatically
6. templates/examples so the right pattern is cheap

## Boundary rule

Parse and validate at edges:

- HTTP / CLI inputs
- env vars
- DB row to domain mapping
- event payload to domain mapping
- third-party integrations

Use trusted values internally. Serialize back to plain values only at egress.

## Anti-patterns to restrict

- `any`
- `as unknown as T`
- direct assertions into branded types
- non-null assertions used as a substitute for modeling
- passing raw DB or transport shapes into domain logic

## Durable adoption plan

1. document the policy
2. add or tighten lints
3. ratchet CI against regressions
4. require new code to follow the policy
5. migrate touched legacy code gradually

## Review questions

- Does this API accept a plain primitive where a trusted type should exist?
- Is validation happening at the right boundary?
- Can this trusted value be mutated after validation?
- Could this guarantee be bypassed with an unchecked cast?
- Is the enforcement in docs only, or also in tooling?
