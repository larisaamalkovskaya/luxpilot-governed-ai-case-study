# Demo Walkthrough

This walkthrough traces a single advisory interaction end to end, using the
synthetic bakery fixture. It shows exactly where the LLM is and is not involved.

## Scenario

A user is deciding among options in the (invented) bakery domain. We follow the
fixture in `fixtures/synthetic-bakery/`.

## Step 1 — Intake

The user provides free-text input. The system may call the LLM to *extract*
candidate variables (extraction under contract). These candidates are **not**
yet usable for routing.

## Step 2 — Confirmation

Each extracted candidate is confirmed. Confirmation may be an explicit user
acknowledgement or a validated deterministic check. After confirmation, the
system assembles an immutable **profile snapshot** — this is
`fixtures/synthetic-bakery/snapshot.json`.

## Step 3 — Deterministic routing

The routing engine evaluates the toy rulebase
(`toy-rulebase/toy-rulebase.json`) against the snapshot. No model is called.
The output is a **routing result**, which must equal
`fixtures/synthetic-bakery/expected-routing-result.json`.

The fired rules and the reasoning path are summarized in
`fixtures/synthetic-bakery/trace-summary.json`.

## Step 4 — Audit

An audit log entry is written describing the snapshot reference, the rulebase
version, the fired rules, and the result. See
`audit/sample-routing-execution-log.json` for the shape of such an entry.

## Step 5 — Explanation (LLM)

Only now does the LLM produce user-facing text. It receives the decided routing
result plus its trace, and generates an explanation. It cannot change the
decision and cannot introduce claims that are not supported by the trace. A
sanitized illustration of such a prompt is in
`prompts/sample-explanation-prompt-public.md`.

## Where the governance boundary sits

- **Model involved:** extraction (Step 1), explanation (Step 5).
- **Model NOT involved:** confirmation gate (Step 2), routing (Step 3), audit
  (Step 4).

The decision is made entirely in Step 3 by deterministic code. Everything the
user reads in Step 5 is downstream of a decision that was already fixed and
logged.

## Try the comparison

See `REPRODUCIBILITY.md` for how to compare the produced result and trace
against the expected fixture files.
