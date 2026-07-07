# Public Specification

This is a public, implementation-independent specification of the **governed
contract** that LuxPilot upholds. It defines *what must be true*, not *how it is
coded*.

## Terms

- **Variable** — a named input value used by routing.
- **Confirmed variable** — a variable whose value has been explicitly set by the
  user or by a validated deterministic step. Only confirmed variables are
  eligible for routing.
- **Profile snapshot** — an immutable set of confirmed variables at a point in
  time. Schema: `schemas/profile-snapshot.schema.json`.
- **Rulebase** — a versioned, deterministic set of rules. Illustrated by
  `toy-rulebase/toy-rulebase.json`.
- **Routing result** — the deterministic output of evaluating a rulebase against
  a snapshot. Schema: `schemas/routing-result.schema.json`.
- **Audit log entry** — a structured record of one routing execution. Schema:
  `schemas/audit-log.schema.json`.

## Normative requirements

Requirements use RFC-2119-style keywords.

### R1 — Determinism
The routing engine **MUST** be a pure function of the pair
`(profile_snapshot, rulebase_version)`. Two evaluations with equal inputs
**MUST** produce equal routing results.

### R2 — No model on the routing path
Routing **MUST NOT** invoke any LLM or other non-deterministic service. The LLM
**MUST NOT** influence the selection of a routing result.

### R3 — Confirmed-only inputs
Routing **MUST** reject any snapshot that contains unconfirmed variables. It
**MUST NOT** infer or guess values on the routing path.

### R4 — Auditability
Every routing execution **MUST** emit an audit log entry conforming to
`schemas/audit-log.schema.json`, capturing the snapshot reference, the rulebase
version, the evaluated/fired rules, and the result.

### R5 — Replayability
Given an audit log entry and the referenced rulebase version, replaying the
decision **MUST** reproduce the identical routing result. See
`audit/replay-contract.md`.

### R6 — Model-agnostic LLM contracts
All LLM interactions **MUST** occur through the function contracts described by
`schemas/llm-function-contracts.schema.json`. Replacing the model **MUST NOT**
change routing results.

### R7 — Grounded generation
The LLM **MUST NOT** produce a user-facing recommendation unless supplied with a
decided routing result and its supporting context. Generation **MUST NOT**
introduce claims unsupported by that context.

## Conformance

An implementation conforms to this specification if it satisfies R1–R7 and can
reproduce the fixtures under `fixtures/` against the rulebase in
`toy-rulebase/`.
