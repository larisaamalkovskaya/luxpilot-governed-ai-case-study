# Evaluation

LuxPilot's evaluation strategy reflects its architecture: the deterministic
core is checked for **correctness and stability**, while the LLM boundary is
checked for **contract compliance and grounding**. These are different kinds of
evaluation and are kept separate.

## 1. Deterministic core evaluation

Because routing is a pure function of `(snapshot, rulebase_version)`, it is
evaluated with fixtures rather than statistical metrics.

- **Golden fixtures.** Each synthetic snapshot has an expected routing result
  (`fixtures/*/expected-routing-result.json`). A run passes only if the
  produced result matches exactly.
- **Stability.** Running the same fixture repeatedly must produce identical
  results (see `tests/test_same_input_same_output.md`).
- **Rejection.** Snapshots containing unconfirmed variables must be rejected
  (see `tests/test_unconfirmed_variables_rejected.md`).

## 2. Governance-boundary evaluation

These evaluations assert the structural guarantees of the system:

- **No LLM on routing path**
  (`tests/test_no_llm_on_routing_path.md`).
- **Audit/replay contract holds**
  (`tests/test_audit_replay_contract.md`).
- **Generation requires context**
  (`tests/test_generation_requires_context.md`): the LLM cannot generate a
  user-facing recommendation unless a decided result and its context are
  supplied.

## 3. LLM boundary evaluation

The LLM is judged only within its sanctioned roles:

- **Extraction under contract.** Does the model return values that conform to
  the function contract schema (`schemas/llm-function-contracts.schema.json`)?
  Non-conforming outputs are rejected before they can become confirmed
  variables.
- **Explanation grounding.** Does the explanation reference only facts present
  in the routing result and its trace? Explanations that introduce unsupported
  claims fail.
- **Model-agnosticism.** Swapping the underlying model must not change routing
  results, only the phrasing of explanations.

## Synthetic evaluation data

All evaluation inputs in this repo are synthetic (see `fixtures/`). This keeps
the evaluation reproducible and shareable while avoiding any exposure of real
user data. The synthetic domains (a bakery and a salon) are deliberately
mundane and invented.

## What we do not claim

This repo does not publish accuracy numbers for a specific production model.
The point of the public evaluation design is the **structure** of the checks:
determinism, rejection, grounding, and contract compliance.
