# Test: Unconfirmed variables are rejected

**Guarantee:** Routing consumes only confirmed variables. Any snapshot that
contains an unconfirmed variable must be rejected before routing runs.

## Given

- A snapshot derived from `fixtures/synthetic-bakery/snapshot.json`, but with
  one variable altered so that `confirmed: false` (for example, an LLM-
  extracted `budget_tier` that the user has not yet acknowledged).

## When

- The snapshot is submitted to the routing engine.

## Then

- The engine **rejects** the snapshot and does **not** produce a routing
  result.
- The rejection identifies the offending variable(s).
- No audit log entry for a *decision* is written (a rejection may be logged,
  but no routing result exists).

## Also

- A variable whose `source` is only an LLM extraction (never confirmed by
  `user` or `deterministic-validation`) must be treated as unconfirmed.

## Notes

This prevents model guesses from silently entering decisions. The confirmation
gate is the boundary between untrusted candidates and trusted routing inputs.

See `../PUBLIC_SPEC.md` requirement **R3** and
`../schemas/profile-snapshot.schema.json` (`confirmed` must be `true`).
