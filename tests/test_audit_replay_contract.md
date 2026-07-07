# Test: Audit / replay contract

**Guarantee:** Every routing execution writes an audit log entry that is
sufficient to replay the decision and reproduce a byte-identical result.

## Given

- A completed routing execution and its audit entry (see
  `../audit/sample-routing-execution-log.json`).
- Access to the referenced rulebase version.

## When

- The decision is replayed using only the audit entry: its `snapshot_id`
  reference, its `rulebase_version`, and the rulebase content.

## Then

- Replay produces a routing result whose content digest equals the entry's
  `result_digest`.
- The replayed `fired_rules` equal the logged `fired_rules`, in the same order.
- The entry validates against `../schemas/audit-log.schema.json`.
- The entry declares `replayable: true` and `llm_involved_in_routing: false`.

## Negative case

- If the rulebase version referenced by the entry is unavailable, replay must
  **fail loudly** rather than silently substitute a different version.

## Notes

Replay turns "we logged something" into "we can prove what happened". The
digest check is what makes the guarantee objective. See
`../audit/replay-contract.md` for the exact contract and
`../PUBLIC_SPEC.md` requirements **R4** and **R5**.
