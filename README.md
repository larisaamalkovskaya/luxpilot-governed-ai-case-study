# LuxPilot — Governed AI Advisory System (Public Case Study)

> Public case study of **LuxPilot**: a governed LLM advisory system with
> deterministic routing, audit/replay design, model-agnostic LLM contracts,
> and synthetic evaluation fixtures.

This repository is a **public, sanitized case study**. It does not contain the
production codebase, credentials, customer data, or proprietary rulebases.
Everything here is either documentation, a public specification, a JSON schema,
a synthetic fixture, or a toy example created specifically for illustration.

## What LuxPilot is

LuxPilot is a decision-support / advisory system that helps users make
structured decisions in a narrow domain. The design goal is **governed AI**:
the parts of the system that make decisions are deterministic, inspectable, and
reproducible, while the LLM is confined to bounded, auditable roles
(explanation, extraction under contract, natural-language generation from an
already-decided result).

## Design principles

- **Deterministic routing.** Given the same confirmed input snapshot and the
  same rulebase version, routing produces the same result. No randomness, no
  model calls on the routing path.
- **LLM off the decision path.** The LLM never decides routing. It explains,
  extracts, or renders text from a decision the deterministic engine already
  made.
- **Model-agnostic LLM contracts.** LLM interactions are defined by JSON
  function contracts, so the underlying model can be swapped without changing
  system behavior.
- **Audit and replay.** Every routing execution emits a structured log that is
  sufficient to replay the decision and get a byte-identical result.
- **Confirmed variables only.** Routing consumes only variables that have been
  explicitly confirmed. Unconfirmed or model-guessed variables are rejected.
- **Reproducibility.** Synthetic fixtures and a toy rulebase let anyone
  reproduce the documented behavior without private data.

## Repository map

| Path | Purpose |
| --- | --- |
| `CASE_STUDY.md` | Narrative case study: problem, approach, outcomes. |
| `ARCHITECTURE.md` | System architecture and component boundaries. |
| `REPRODUCIBILITY.md` | How to reproduce documented behavior. |
| `EVALS.md` | Evaluation approach and synthetic evaluation design. |
| `PUBLIC_SPEC.md` | Public specification of the governed contract. |
| `REDACTION_POLICY.md` | What was removed/sanitized and why. |
| `ROADMAP.md` | Direction and open questions. |
| `docs/` | Walkthroughs and diagrams. |
| `schemas/` | JSON schemas for snapshots, routing results, contracts, logs. |
| `fixtures/` | Synthetic input/output fixtures (bakery, salon). |
| `toy-rulebase/` | A small, public, illustrative rulebase. |
| `tests/` | Human-readable specifications of governance tests. |
| `prompts/` | Public, sanitized sample prompts. |
| `audit/` | Sample execution log and replay contract. |

## Status

This is a documentation-first case study. See `ROADMAP.md` for what is planned
and `REDACTION_POLICY.md` for what is intentionally omitted.

## License

Released under the MIT License. See `LICENSE`.
