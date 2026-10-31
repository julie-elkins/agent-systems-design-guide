# Design decision: make evaluation scope and release evidence explicit

## Problem

An agent can produce a plausible final answer while using the wrong tool, losing
multi-turn state, skipping confirmation, or relying on an invalid evaluator. A single
score can hide those failures and mislead a release decision.

## Decision

Teach one evaluation lifecycle with explicit deterministic, semantic, behavioral,
multi-turn, and trajectory checks. Keep the operational checklist in the Agent
Assurance Harness and the design reasoning in this repository.

## Instead of

Use one end-to-end score, create separate technology-specific evaluation guides, or
duplicate the checklist in every repository.

## Why

Different checks answer different questions. Fixed-rule checks are strong for schemas,
tool calls, state, and thresholds. Meaning-based checks require grader validation.
Behavioral and trajectory checks expose failures that final-answer grading misses.
Keeping the practical checklist beside runnable evidence makes the release path
usable without turning the design guide into an implementation repository.

## Official guidance considered

- [OpenAI Evals](https://github.com/openai/evals), OpenAI, accessed 2026-10-30.
- [Trustworthy agents](https://www.anthropic.com/research/trustworthy-agents), Anthropic, accessed 2026-10-30.

## Implementation

- [Agent evaluation and release guide](../AGENT_EVALUATION_AND_RELEASE.md)
- [Agent Assurance Harness checklist](https://github.com/julie-elkins/agent-assurance-harness/blob/main/AGENT_EVALUATION_CHECKLIST.md)
- [LLM Eval Validity](https://github.com/julie-elkins/llm-eval-validity)

## Verification

Review the lifecycle, confirm the checklist requires an evidence artifact and decision
rule for each control, and run the linked harness checks and synthetic negative controls.

## Costs if wrong

The guidance may be mistaken for a universal scoring formula or production approval.
The documents therefore require human ownership, preserve `not_evaluated` as distinct
from `passed`, and state what the evidence does not prove.

## What this does not prove

The guide and checklist do not prove live model quality, production safety, business
impact, or generalization beyond the declared workflow, data, revisions, and test set.

## Decided by

Julie Elkins, 2026-10-30.

## Teaching link

Start with the lifecycle, choose one high-risk workflow, run the checklist, and use a
known negative control to verify that the evaluation can catch a false pass.

## Evaluator layers and controlled comparisons

### Decision

Teach evaluator layers explicitly: the agent-as-a-judge may inspect a run's trace and
result, while the harness-as-a-judge checks trace completeness and declared controls.
Require controlled comparisons to report correctness, safety, tool-call correctness,
latency, cost, and user outcomes separately.

### Instead of

Collapse the run into one automated score, treat the judge as ground truth, or let a
preferred A/B result redefine the release rule after the comparison is visible.

### Why

Final-answer quality can hide an unsafe or incorrect trajectory. Separate dimensions
make those failures visible, while calibration and abstention keep an automated judge
from becoming an unreviewed authority.

### Implementation

- [Agent evaluation and release guide](../AGENT_EVALUATION_AND_RELEASE.md)
- [Agent Assurance Harness checklist](https://github.com/julie-elkins/agent-assurance-harness/blob/main/AGENT_EVALUATION_CHECKLIST.md)

### Verification

Confirm the guide names deterministic trace checks, calibrated semantic grading,
abstention, separate comparison dimensions, blocking thresholds, and a rerun policy.
Confirm the harness checklist has evidence rows for evaluator layers and controlled
variant comparisons.

### Costs if wrong

The guidance could add review work or be mistaken for a requirement to use another
model. It therefore leaves deterministic checks first, keeps human ownership explicit,
and permits a result to remain `not_evaluated`.

### What this does not prove

These controls do not prove production quality, safety, or business impact. They make
the evidence and unresolved uncertainty inspectable for the declared workflow.

### Decided by

Julie Elkins, 2026-10-31.
