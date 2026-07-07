# Test: No LLM on the routing path

**Guarantee:** The routing engine never invokes an LLM (or any non-
deterministic service), and the LLM cannot influence which routing result is
selected.

## Given

- A confirmed snapshot (e.g. `fixtures/synthetic-salon/snapshot.json`).
- The routing engine configured to run against `toy-rulebase` `v1.0.0`.
- All model/network endpoints made unavailable (or observed).

## When

- The snapshot is routed.

## Then

- Routing completes successfully **without** any model or network call.
- With endpoints observed, **zero** LLM calls are recorded during routing.
- The produced result equals
  `fixtures/synthetic-salon/expected-routing-result.json`.
- The audit entry records `llm_involved_in_routing: false`.

## Adversarial variant

- If a model endpoint is present but returns arbitrary/garbage responses, the
  routing result **must be unchanged**, proving the model has no influence on
  the decision path.

## Notes

This is the central governance boundary. It is what lets the system claim
determinism and clean accountability: a wrong decision is attributable to the
input or the rules, never to model variance.

See `../PUBLIC_SPEC.md` requirement **R2** and
`../schemas/audit-log.schema.json` (`llm_involved_in_routing`).
