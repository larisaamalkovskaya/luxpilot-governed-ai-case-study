# Sequence Flow

A temporal view of one advisory interaction, emphasizing that the LLM is called
before and after the decision, but never during it.

```mermaid
sequenceDiagram
    actor User
    participant Intake
    participant LLM
    participant Gate as Confirmation gate
    participant Engine as Routing engine (deterministic)
    participant Audit as Audit log

    User->>Intake: free-text input
    Intake->>LLM: extract candidate variables (contract)
    LLM-->>Intake: candidate variables
    Intake->>Gate: submit candidates
    Gate->>User: confirm variables
    User-->>Gate: confirmations
    Gate->>Engine: confirmed snapshot (immutable)
    Note over Engine: pure function of (snapshot, rulebase_version)\nno LLM, no randomness
    Engine->>Audit: write execution log
    Engine-->>Gate: routing result + trace
    Gate->>LLM: explain result (grounded in trace)
    LLM-->>User: explanation (cannot change result)
```

## Notes

- The **only** inputs to the routing engine are the confirmed snapshot and the
  rulebase version.
- The **audit log** write happens as part of the same deterministic step, so
  every result is logged.
- The final LLM call is **downstream** of the decision and is constrained to
  the trace it is given.

## Replay view

```mermaid
sequenceDiagram
    participant Reviewer
    participant Audit as Audit log
    participant Engine as Routing engine

    Reviewer->>Audit: fetch log entry
    Audit-->>Reviewer: snapshot ref + rulebase version + result
    Reviewer->>Engine: re-run (snapshot, rulebase_version)
    Engine-->>Reviewer: result'
    Note over Reviewer: result' MUST equal logged result
```

See `architecture-diagram.md` for the component view and
`../../audit/replay-contract.md` for the replay contract.
