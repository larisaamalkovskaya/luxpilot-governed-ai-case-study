# Tests

These tests are written as **human-readable specifications**. Each describes a
governance guarantee, the setup, the action, and the expected outcome, so the
guarantees can be understood and checked independently of any implementation.

## Test index

| Test | Guarantee |
| --- | --- |
| `test_same_input_same_output.md` | Determinism of routing. |
| `test_unconfirmed_variables_rejected.md` | Confirmed-only inputs. |
| `test_no_llm_on_routing_path.md` | No model on the decision path. |
| `test_audit_replay_contract.md` | Replayability from the audit log. |
| `test_generation_requires_context.md` | Grounded, decision-downstream generation. |

## Format

Each test file follows the same structure:

- **Guarantee** — the property under test.
- **Given** — the setup / preconditions.
- **When** — the action taken.
- **Then** — the expected, checkable outcome.
- **Notes** — why the guarantee matters.

## Relationship to fixtures

Several tests reference the synthetic fixtures in `../fixtures/` and the toy
rulebase in `../toy-rulebase/`, so they can be reproduced without private data.
See `../EVALS.md` for how these fit into the overall evaluation strategy.
