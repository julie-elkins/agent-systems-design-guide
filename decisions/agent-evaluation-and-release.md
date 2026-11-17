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

- [Codex evaluation guidance](https://developers.openai.com/codex/use-cases/ai-app-evals), OpenAI, accessed 2026-11-05.
- [Promptfoo](https://github.com/promptfoo/promptfoo), maintained evaluation-runner reference, accessed 2026-11-05.
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

## Trust-gated autonomy and evidence custody

### Problem

An agent can appear effective while the evaluator is uncalibrated, the harness
context is changing, or the executor can rewrite the record used to justify its
own autonomy. A public incident index can also be mistaken for telemetry-backed
evidence.

### Decision

Teach autonomy as trust-gated: expand permissions only after evaluator validity,
authorization ownership, and evidence custody are explicit. Evaluate the complete
harness—model, tools, permissions, policies, state, and evaluators—rather than the
model alone. Name who holds the authoritative evidence and whether the executor can
rewrite or delete it.

### Instead of

Use a composite trust score, treat a model score as sufficient, or create a public
attack-harness index that implies live coverage from reported incidents.

### Why

The costly failure is not only an incorrect answer. It is an agent receiving more
authority because a weak evaluator, stale record, or non-independent evidence source
made the workflow look safe. A named gate makes the expansion decision reviewable and
keeps public claims within the evidence actually collected.

### Implementation

- [Agent evaluation and release guide](../AGENT_EVALUATION_AND_RELEASE.md)
- [Evaluation research checklist](../EVALUATION_RESEARCH_CHECKLIST.md)
- [Agent Assurance Harness checklist](https://github.com/julie-elkins/agent-assurance-harness/blob/main/AGENT_EVALUATION_CHECKLIST.md)

### Verification

Check that the evaluation includes a calibrated evaluator, an independent false-pass
control, explicit authorization ownership, an evidence holder, and a declared
executor-mutability boundary. Keep a public incident report separate from
telemetry-backed evidence.

### Costs if wrong

The added gate may slow autonomy expansion and require more review evidence. That cost
is intentional: the alternative is granting consequential authority on evidence the
executor or evaluator can silently invalidate.

### What this does not prove

These documentation patterns do not prove that an evaluator is valid for every task,
that a public incident index is complete, or that a real system's evidence store is
immutable. Those claims require separate system-specific evidence.

### Decided by

Julie Elkins, 2026-11-17.

### Teaching link

Use the `🧭 Take this forward` callout to ask learners to name the decision,
authorization owner, independent evidence holder, false-pass check, and claim
boundary before expanding an agent's autonomy.
