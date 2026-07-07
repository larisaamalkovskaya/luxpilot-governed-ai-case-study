# From Beta to Rebuild

This note summarizes the lessons that drove LuxPilot from an early beta into a
governed rebuild. It is written at a design level, without production details.

## The beta

The initial beta placed the LLM close to the decision. The model both
interpreted the user and, in effect, chose the recommendation. It demoed well
and was fast to build.

## What went wrong

Three recurring problems appeared:

1. **Inconsistent recommendations.** Equivalent inputs sometimes produced
   different advice. Users noticed, and it eroded trust.
2. **Unexplainable decisions.** When a recommendation was questioned, there was
   no faithful account of *why* it was produced — the reasoning lived inside an
   opaque generation.
3. **No replay.** Reproducing a past decision for review was not reliably
   possible.

These were not prompt-tuning problems. They were architectural: a
non-deterministic component was on the decision path.

## The rebuild

The rebuild introduced one firm boundary: **the LLM must not decide**.

- Decisions moved into a **deterministic routing engine** driven by a
  **versioned rulebase**.
- The LLM was confined to **extraction under contract**, **explanation**, and
  **grounded generation**.
- Every decision began emitting an **audit log** sufficient for **replay**.
- LLM interactions were redefined as **model-agnostic contracts** so the model
  could be swapped without behavioral drift.

See `ARCHITECTURE.md` and `PUBLIC_SPEC.md` for the resulting design.

## Outcomes

- Equivalent inputs now yield identical results (determinism).
- Every recommendation has a faithful, inspectable rationale (audit + trace).
- Past decisions can be replayed exactly (replay contract).
- The model can be upgraded without changing what the system decides.

## The general lesson

For advisory systems, treat the LLM as a **communication layer**, not a
**decision layer**. Keep the decision deterministic, versioned, and logged, and
let the model do what it is good at: understanding and explaining.
