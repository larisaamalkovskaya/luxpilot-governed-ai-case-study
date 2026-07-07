# Reproducibility

This repository is designed so that the documented behavior can be reproduced
without any private data. Reproducibility is a governance property here, not an
afterthought.

## What "reproducible" means for LuxPilot

Given:
- a **confirmed profile snapshot** (see `schemas/profile-snapshot.schema.json`),
- a **rulebase version** (see `toy-rulebase/toy-rulebase.json`),

the deterministic routing engine must always produce the **same routing
result** (see `schemas/routing-result.schema.json`). No model calls, no clocks,
no randomness participate in routing.

## Reproduce the documented fixtures

The `fixtures/` directory contains synthetic end-to-end examples for two
invented domains:

- `fixtures/synthetic-bakery/`
- `fixtures/synthetic-salon/`

Each fixture contains:
- `snapshot.json` — a confirmed input snapshot,
- `expected-routing-result.json` — the routing result the engine must produce,
- `trace-summary.json` — a human-readable summary of which rules fired.

To reproduce conceptually:
1. Load `snapshot.json`.
2. Evaluate it against the toy rulebase (`toy-rulebase/toy-rulebase.json`).
3. Compare the produced result to `expected-routing-result.json`. They must
   match exactly.
4. Compare the fired-rules trace to `trace-summary.json`.

Because the fixtures are synthetic and the rulebase is public, anyone can
perform this comparison without access to production systems.

## Determinism checklist

- [x] No LLM call on the routing path.
- [x] No wall-clock or timezone dependence in routing.
- [x] No random seeds.
- [x] Rulebase is versioned and content-addressable.
- [x] Snapshots are immutable once confirmed.

## Replay

Beyond re-running fixtures, any historical decision can be replayed from its
audit log. See `audit/replay-contract.md` for the exact contract and
`audit/sample-routing-execution-log.json` for an example.

## Environment notes

Nothing in this repo requires a specific runtime to *read*. The tests under
`tests/` are written as human-readable specifications so the reproducibility
guarantees can be understood and checked independently of any implementation.
