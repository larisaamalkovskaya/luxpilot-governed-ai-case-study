# Prompts

This folder holds **public, sanitized** sample prompts that illustrate how the
LLM is used within its sanctioned roles. They are not the production prompts
(see `../REDACTION_POLICY.md`).

## Principles reflected in these prompts

- **Grounding.** The LLM is asked to explain a decision using only the supplied
  result and trace. It must not invent facts or change the decision.
- **Role confinement.** Prompts correspond to the roles in
  `../schemas/llm-function-contracts.schema.json` (extraction, explanation,
  generation).
- **Model-agnosticism.** Prompts avoid vendor-specific tricks so any capable
  model can fulfill the contract.

## Contents

- `sample-explanation-prompt-public.md` - an explanation prompt that turns a
  decided routing result into a grounded, user-facing explanation.

## What is not here

No production prompts, no internal instructions, no secrets, and no real user
content. Everything references the synthetic fixtures under `../fixtures/`.
