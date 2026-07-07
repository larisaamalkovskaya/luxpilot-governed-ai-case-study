# Replay Contract

This document defines what it means to **replay** a LuxPilot routing decision
and what a correct replay must produce.

## Inputs to a replay

A replay uses only:

1. an **audit log entry** conforming to
   `../schemas/audit-log.schema.json` (see
   `sample-routing-execution-log.json`);
2. the **profile snapshot** referenced by the entry's `snapshot_id`;
3. the **rulebase** at the entry's `rulebase_version`.

No other input is permitted. In particular, no LLM and no current-time value
may participate.

## Procedure

1. Load the snapshot referenced by `snapshot_id`.
2. Load the rulebase at `rulebase_version`.
3. Evaluate the rulebase against the snapshot with the deterministic engine.
4. Compute the content digest of the produced routing result.

## Success criteria

A replay is correct if and only if:

- the produced result's digest equals the entry's `result_digest`; and
- the produced `fired_rules` equal the entry's `fired_rules`, in order.

## Failure handling

- If the referenced `rulebase_version` is unavailable, replay MUST fail and
  MUST NOT substitute another version.
- If the digest does not match, replay MUST report a mismatch rather than
  accept the new result.

## Digest definition

`result_digest` is `sha256:` followed by the SHA-256 hex digest of the routing
result serialized in its canonical form (stable key ordering, no insignificant
whitespace). The digest in `sample-routing-execution-log.json` is illustrative.

## Guarantees this enables

- **Accountability:** any past decision can be reconstructed exactly.
- **Tamper-evidence:** a changed rulebase or snapshot yields a different
  digest, so silent drift is detectable.

See `../PUBLIC_SPEC.md` requirements **R4** and **R5**, and
`../tests/test_audit_replay_contract.md`.
