# LuxPilot Case Study

## Summary

LuxPilot is a governed LLM advisory system. This case study documents the
design decisions that keep the system deterministic, auditable, and
model-agnostic, and it shares synthetic artifacts so the behavior can be
inspected without any private data.

## The problem

Advisory / decision-support products are tempting to build entirely on top of
an LLM. But an LLM on the decision path creates three hard problems:

1. **Non-determinism.** The same input can produce different recommendations.
2. **Non-reproducibility.** You cannot reliably replay why a decision was made.
3. **Opaque accountability.** When a recommendation is wrong, it is unclear
   whether the cause was the input, the rules, or the model.

For an advisory product, these are not edge cases — they are the core risk.

## The approach

LuxPilot separates *deciding* from *communicating*.

- **Deciding** is done by a deterministic routing engine that consumes a
  conCASE_STUDY.mdfirmed input snapshot and a versioned rulebase. Same snapshot plus same
  rulebase version always yields the same routing result.
- **Communicating** is done by the LLM: it explains results, extracts
  structured variables under contract, and generates natural language from a
  decision that has already been made.

The LLM never chooses the outcome. This is the central governance boundary.

## What this bought us

- **Determinism:** identical inputs yield identical routing results.
- **Replayability:** every decision emits a log sufficient to reproduce it.
- **Model-agnosticism:** the model can be swapped behind JSON function
  contracts without changing decisions.
- **Clear accountability:** input, rules, and model are separated, so failures
  can be attributed to the right layer.

## What is in this repo

This public case study includes the architecture, a public specification, JSON
schemas, synthetic fixtures for two invented domains (a bakery and a salon), a
toy rulebase, human-readable governance tests, and a sample audit log with a
replay contract. See `README.md` for the full map.

## What is intentionally not here

No production code, no real customer data, no proprietary rulebase, no
credentials, no model weights. See `REDACTION_POLICY.md`.
