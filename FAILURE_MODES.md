# Failure Modes

## 1. LLM shadow-routing

The LLM implicitly changes the recommendation during explanation or generation.

Mitigation: explanation receives trace summaries only; generation receives routed plan context only.

## 2. Unconfirmed extraction enters routing

The model extracts a variable and the system treats it as confirmed.

Mitigation: routing rejects unconfirmed variables.

## 3. Capacity-overload recommendation

The recommendation is strategically plausible but exceeds the owner’s capacity.

Mitigation: feasibility checks and low-burden plan constraints.

## 4. Prompt leakage of rule logic

Production rule content is embedded in prompts.

Mitigation: prompts do not contain production rule predicates or full gate logic.

## 5. Non-replayable recommendation

A future run cannot reproduce why the system recommended something.

Mitigation: snapshot hash + rule-base version + engine build -> output hash.
