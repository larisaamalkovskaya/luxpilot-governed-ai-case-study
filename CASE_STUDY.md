# LuxPilot Case Study

## Summary

LuxPilot is a governed LLM advisory system for small business decision support. This case study documents the design decisions that keep the system deterministic, auditable, model-agnostic, and suitable for evaluation.

The repository shares a sanitized public version of the architecture, synthetic fixtures, toy examples, public schemas, and demonstration screenshots. It does not disclose production rules, exact gate predicates, prompts, templates, user data, or real execution logs.

## The problem

Advisory and decision-support products are tempting to build entirely on top of an LLM. The model can ask questions, interpret user input, generate advice, and explain itself fluently.

But when the LLM sits on the decision path, three hard problems appear:

1. **Non-determinism.** The same input can produce different recommendations.
2. **Non-reproducibility.** It becomes difficult to replay why a decision was made.
3. **Opaque accountability.** When a recommendation is wrong, it is unclear whether the failure came from the input, the rule logic, the prompt, or the model.

For an advisory product, these are not edge cases. They are the core governance risk.

A fluent answer is not enough. The system must also be feasible, inspectable, replayable, and safe to validate.

## The core design decision

LuxPilot separates **deciding** from **communicating**.

- **Deciding** is handled by a deterministic routing engine.
- **Communicating** is handled by the LLM.

The LLM may extract candidate variables, explain a decision, generate draft assets, or ask adaptive questions. It does not choose the strategic outcome.

This is the central governance boundary of LuxPilot:

```text
confirmed snapshot -> deterministic routing -> structured result
LLM output -> explanation / candidate / draft -> user validation
