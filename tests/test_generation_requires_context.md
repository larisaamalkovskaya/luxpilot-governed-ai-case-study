# Test: Generation requires context

**Guarantee:** The LLM cannot produce a user-facing recommendation unless it
is supplied with an already-decided routing result and its supporting trace.
Generation is downstream of the decision and grounded in it.

## Given

- The generation function contract from
  `../schemas/llm-function-contracts.schema.json` (role `generation`,
  `requires_decided_context: true`).

## When (A) - missing context

- The generation function is invoked **without** a decided routing result.

## Then (A)

- The call is **refused** before reaching the model; no user-facing
  recommendation is produced.

## When (B) - with context

- The generation function is invoked with
  `fixtures/synthetic-bakery/expected-routing-result.json` and its
  `trace-summary.json`.

## Then (B)

- The generated explanation references only facts present in the result and
  trace (e.g. the fired rules and chosen route).
- The explanation does **not** introduce claims unsupported by the trace, and
  does **not** alter the decision.

## Notes

This keeps the model in a communication role. Even a perfectly fluent
explanation is invalid if it is ungrounded or if it changes the decision.

See `../PUBLIC_SPEC.md` requirement **R7** and
`../prompts/sample-explanation-prompt-public.md`.
