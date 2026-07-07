# Redaction Policy

This repository is a **public case study**. To publish it safely, several
categories of material were removed or replaced with synthetic equivalents.
This document explains what was redacted and why, so readers can trust that
nothing sensitive is present and understand what a full implementation would
additionally contain.

## What was removed

### Production code
No production source is included. The repo documents architecture and
contracts, not the running system. This prevents leaking implementation details
and keeps the focus on the governance design.

### Real customer / user data
No real profiles, inputs, transcripts, or outputs appear anywhere. Every input
in `fixtures/` is synthetic and describes invented entities (a bakery, a
salon).

### Proprietary rulebase
The real decision rulebase is not published. Instead, `toy-rulebase/` contains
a small, obviously illustrative ruleset that demonstrates the *shape* of
deterministic routing without revealing the actual domain logic.

### Credentials and secrets
No API keys, tokens, connection strings, model endpoints, or environment files
are included. None are needed to read or reason about this case study.

### Model weights and vendor specifics
No model weights, fine-tunes, or vendor-specific prompt tuning are included. The
LLM boundary is described only through model-agnostic contracts.

## What was synthesized

- **Fixtures** (`fixtures/`): invented snapshots and their expected results.
- **Toy rulebase** (`toy-rulebase/`): a public stand-in for the real rulebase.
- **Sample audit log** (`audit/sample-routing-execution-log.json`): a fabricated
  but schema-valid execution record.
- **Sample prompt** (`prompts/sample-explanation-prompt-public.md`): a sanitized
  illustration of an explanation prompt.

## What is faithfully represented

- The **architecture** and component boundaries.
- The **normative contract** (see `PUBLIC_SPEC.md`).
- The **schemas** (`schemas/`), which mirror the real data shapes at a
  structural level.
- The **governance tests** (`tests/`), which describe the guarantees the real
  system upholds.

## Principle

When in doubt, this repo prefers **synthetic and structural** over **real and
detailed**. The goal is to make the governance approach fully understandable
without exposing anything that should remain private.
