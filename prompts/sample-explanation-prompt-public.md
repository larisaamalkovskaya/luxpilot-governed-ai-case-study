# Sample Explanation Prompt (Public)

This is a **sanitized illustration** of an explanation prompt. It corresponds
to the `explanation` role in
`../schemas/llm-function-contracts.schema.json`. The model is given a decided
routing result and its trace, and must explain it without changing it.

## System instruction

```text
You are an explanation component in a governed advisory system.
You will receive a DECISION (already made by a deterministic engine) and a
TRACE of the rules that produced it.

Rules:
1. Explain the decision using ONLY facts in DECISION and TRACE.
2. Do NOT change, second-guess, or add to the decision.
3. Do NOT introduce numbers, options, or claims not present in the inputs.
4. If asked for something beyond the inputs, say it is not available.
Keep the explanation short, plain, and user-friendly.
```

## Input (example, synthetic bakery)

```json
{
  "decision": {
    "route": "loyalty_program_lightweight",
    "parameters": { "channel": "in_store_signup" }
  },
  "trace": [
    { "rule": "R-GOAL-REPEAT", "why": "goal is repeat customers" },
    { "rule": "R-NO-STOREFRONT", "why": "no online storefront" },
    { "rule": "R-BUDGET-LOW", "why": "budget is low" }
  ]
}
```

## Expected style of output

A short paragraph that says, in plain language, that a lightweight in-store
loyalty program was recommended because the goal is repeat customers, there is
no online storefront, and the budget is low - and nothing beyond that.

## Why this is safe

- No real user data (uses the synthetic bakery fixture).
- The model cannot alter the decision; it only phrases it.
- Grounding is enforced: every claim must trace back to an input fact.

See `../tests/test_generation_requires_context.md` for the guarantee this
prompt is designed to satisfy.
