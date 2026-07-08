# Demo Walkthrough

This walkthrough traces a single LuxPilot advisory interaction end to end.

It connects the public demo screenshots with the sanitized repository artifacts: synthetic fixtures, toy rulebase, schemas, audit example, and prompt hygiene example.

The walkthrough shows where the LLM is involved, where it is explicitly not involved, and how the system keeps the strategic decision deterministic and replayable.

## Visual reference

Public screenshots for this walkthrough are available in [`docs/screenshots`](screenshots/).

The screenshot set follows the public demo flow:

1. landing page and value proposition;
2. business context and offering-line interpretation;
3. focus and challenge selection;
4. routed strategy recommendation;
5. capacity-aware action plan.

All screenshots use synthetic/demo data only. They do not disclose production rules, prompts, templates, real user profiles, or execution logs.

## Scenario

A synthetic small business user wants a feasible marketing recommendation.

The public fixtures use invented examples such as a bakery and a salon. These fixtures are included to demonstrate the governance pattern without disclosing production logic or private data.

Relevant files:

- [`fixtures/synthetic-bakery/snapshot.json`](../fixtures/synthetic-bakery/snapshot.json)
- [`fixtures/synthetic-bakery/expected-routing-result.json`](../fixtures/synthetic-bakery/expected-routing-result.json)
- [`fixtures/synthetic-bakery/trace-summary.json`](../fixtures/synthetic-bakery/trace-summary.json)
- [`toy-rulebase/toy-rulebase.json`](../toy-rulebase/toy-rulebase.json)

## Step 1 — Intake

The user provides business context, either by entering free text or by giving a website link.

The LLM may be used here to extract candidate variables from messy input.

Example candidate variables may include:

- buyer type;
- offering line;
- product or service type;
- geography;
- trust assets;
- platform dependency;
- current marketing challenge.

At this stage, extracted values are candidates only.

They are not yet routing inputs.

## Step 2 — Confirmation

The system shows the interpreted profile back to the user.

The user can confirm, correct, remove, or add information before the recommendation is made.

This is the confirmation-before-routing boundary.

Unconfirmed LLM extraction cannot enter the deterministic routing step.

In the public demo screenshots, this is shown through screens such as:

- offering-line confirmation;
- focus selection;
- challenge selection;
- context and trust-asset confirmation.

## Step 3 — Snapshot

After confirmation, the system assembles a profile snapshot.

The snapshot is the accountable input to routing.

In the public fixture, the snapshot shape is illustrated here:

- [`fixtures/synthetic-bakery/snapshot.json`](../fixtures/synthetic-bakery/snapshot.json)

The production system treats snapshots as immutable routing inputs. If the user changes important information later, the system should create a new snapshot rather than silently mutating the old one.

## Step 4 — Deterministic routing

The routing engine evaluates the confirmed snapshot against a versioned rulebase.

No model is called during routing.

In this public repository, the production rulebase is not disclosed. Instead, the repository includes a toy rulebase to illustrate the representation pattern:

- [`toy-rulebase/toy-rulebase.json`](../toy-rulebase/toy-rulebase.json)

The routing result is illustrated here:

- [`fixtures/synthetic-bakery/expected-routing-result.json`](../fixtures/synthetic-bakery/expected-routing-result.json)

The trace summary is illustrated here:

- [`fixtures/synthetic-bakery/trace-summary.json`](../fixtures/synthetic-bakery/trace-summary.json)

The important point is not the toy rule itself. The important point is the governance pattern:

```text
confirmed snapshot + rulebase version -> deterministic routing result
