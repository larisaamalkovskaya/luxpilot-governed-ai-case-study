# Toy Rulebase

This is a **small, public, illustrative rulebase**. It is not the production
rulebase (see `../REDACTION_POLICY.md`). Its purpose is to make the shape of
deterministic routing concrete and to let the fixtures under `../fixtures/` be
reproduced.

## Model

`toy-rulebase.json` is a versioned document containing an ordered list of
rules. Each rule has:

- `id` — a stable rule identifier used in traces and audit logs;
- `when` — a list of conditions (all must hold) over confirmed variables;
- `then` — the effect the rule contributes to the routing result.

## Evaluation semantics

1. Rules are evaluated **in order**.
2. A rule fires if **all** of its `when` conditions match the snapshot.
3. Firing rules accumulate effects: a route candidate, a channel, and a
   variant.
4. Evaluation is a **pure function** of the snapshot and this rulebase
   version. No randomness, no clock, no model.

## Determinism

Given the same snapshot and the same `rulebase_version`, evaluation always
produces the same routing result and the same ordered `fired_rules` list. This
is what the fixtures assert.

## Worked examples

- `../fixtures/synthetic-bakery/` fires `R-GOAL-REPEAT`, `R-NO-STOREFRONT`,
  `R-BUDGET-LOW`.
- `../fixtures/synthetic-salon/` fires `R-GOAL-NOSHOW`, `R-HAS-STOREFRONT`,
  `R-BUDGET-MEDIUM`.

See `../PUBLIC_SPEC.md` for the normative requirements the rulebase must
satisfy.
