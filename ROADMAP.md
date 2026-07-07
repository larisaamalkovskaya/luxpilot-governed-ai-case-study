# Roadmap

This roadmap describes the direction of the LuxPilot governance work as
represented in this public case study. It is aspirational and subject to
change; nothing here is a commitment.

## Near term

- **Expand fixtures.** Add more synthetic domains beyond the bakery and salon to
  stress different rulebase shapes and edge cases.
- **Machine-checkable tests.** The tests under `tests/` are currently written as
  human-readable specifications. A future step is to provide runnable checks
  that assert the same guarantees against the toy rulebase.
- **Schema validation examples.** Add worked examples showing each fixture
  validating against its schema in `schemas/`.

## Medium term

- **Rulebase versioning story.** Document how rulebase versions are pinned,
  diffed, and referenced from audit logs so replays remain stable across rule
  changes.
- **Explanation grounding harness.** Formalize how explanations are checked for
  grounding against the routing result and trace.
- **Contract conformance suite.** Provide a reusable checklist/suite for
  validating that a new model still satisfies the model-agnostic contracts.

## Longer term / open questions

- **Partial confirmation flows.** How should the system behave when only some
  variables are confirmed? Today routing rejects unconfirmed inputs; richer
  intermediate states may be worth specifying.
- **Multi-rulebase composition.** Can multiple deterministic rulebases be
  composed while preserving determinism and replayability?
- **Human override auditing.** How to record and replay decisions where a human
  overrides a routing result, without breaking the determinism guarantees of
  the underlying engine.

## Non-goals

- Turning the deterministic core into an LLM-driven component.
- Publishing production code, real data, or the proprietary rulebase (see
  `REDACTION_POLICY.md`).

## Feedback

This is a case study intended to communicate a governance approach. Issues and
discussion about the *design* are welcome; requests for private implementation
details are out of scope.
