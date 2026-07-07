# Architecture

LuxPilot is organized around one hard boundary: the **deterministic decision
core** is separated from the **LLM boundary**. This document describes the
components and how they interact.

## Components

### 1. Intake and confirmation
Collects user input and turns it into a **confirmed profile snapshot**. A
variable is only "confirmed" once the user (or a validated deterministic step)
has explicitly set it. The LLM may *propose* extracted values, but proposals
are not confirmed until validated. See `schemas/profile-snapshot.schema.json`.

### 2. Deterministic routing engine
Takes a confirmed snapshot plus a versioned rulebase and produces a
**routing result**. This engine:
- makes no network calls,
- calls no model,
- uses no randomness,
- is a pure function of `(snapshot, rulebase_version)`.

See `schemas/routing-result.schema.json` and `toy-rulebase/`.

### 3. LLM boundary (model-agnostic)
The LLM is invoked only through typed **function contracts**. It has three
sanctioned roles:
- **Extraction under contract:** turn free text into candidate variables
  (which then must be confirmed).
- **Explanation:** describe an already-computed routing result.
- **Generation:** produce user-facing text from a decided result and context.

The contracts are model-agnostic — see
`schemas/llm-function-contracts.schema.json`. Swapping the model must not change
routing.

### 4. Audit log and replay
Every routing execution writes a structured log capturing inputs, rulebase
version, evaluated rules, and the result. The log is sufficient to replay the
decision deterministically. See `schemas/audit-log.schema.json`,
`audit/replay-contract.md`, and `audit/sample-routing-execution-log.json`.

## Data flow (happy path)

1. User provides input; intake builds a candidate snapshot.
2. Variables are confirmed; the snapshot becomes immutable.
3. The routing engine evaluates the rulebase against the snapshot.
4. A routing result is produced and an audit log entry is written.
5. The LLM generates an explanation from the result (never altering it).

## Invariants

- **No LLM on the routing path.** Routing cannot depend on a model call.
- **Confirmed-only inputs.** Routing rejects unconfirmed variables.
- **Deterministic outputs.** `(snapshot, rulebase_version)` fully determines the
  result.
- **Replayable.** The audit log reproduces the result byte-for-byte.

## Boundaries and non-goals

LuxPilot is not a general chatbot. The LLM is deliberately constrained. If a
task would require the model to make the decision, that task is out of scope
for the governed path and must be redesigned as a deterministic rule or an
explicit user confirmation.
