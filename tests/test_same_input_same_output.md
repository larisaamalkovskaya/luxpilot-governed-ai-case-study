# Test: Same input, same output

**Guarantee:** Routing is deterministic. The same confirmed snapshot evaluated
against the same rulebase version always produces the identical routing result
and the identical ordered `fired_rules` list.

## Given

- The snapshot `fixtures/synthetic-bakery/snapshot.json` (`snap_bakery001`).
- The rulebase `toy-rulebase/toy-rulebase.json` at version `v1.0.0`.

## When

- The routing engine evaluates the snapshot against the rulebase **N times**
  (N >= 2), in any order, on any host.

## Then

- Every run produces a routing result equal to
  `fixtures/synthetic-bakery/expected-routing-result.json`.
- Every run produces the same `fired_rules` order:
  `["R-GOAL-REPEAT", "R-NO-STOREFRONT", "R-BUDGET-LOW"]`.
- Byte-for-byte, all N results are identical.

## Notes

Determinism is the foundation for auditability and replay. If this test can
fail, then no downstream guarantee (replay, accountability) can be trusted.
Because routing performs no model call, no time lookup, and no randomization,
the only inputs are the snapshot and the rulebase version.

See `../PUBLIC_SPEC.md` requirement **R1**.
