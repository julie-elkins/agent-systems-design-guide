# Orchestrator trust boundary

## Problem

An agent workflow can appear reliable because a model answered well, a retry
eventually returned text, or a local harness recorded completion. That evidence
does not establish that approval, model routing, handoffs, memory, retries, or
external effects were safe and recoverable.

The failure this pattern prevents is a workflow reporting recovery when it
actually repeated an unknown or unauthorized action.

## Decision

Evaluate the orchestrator boundary explicitly. An **orchestrator** is the
workflow component that chooses models, delegates work, applies policy, manages
state, requests approval, retries actions, and joins evidence.

Treat each transition as a separate evidence surface:

| Boundary | Required evidence |
| --- | --- |
| Model routing | Selected model, reason for selection, required capability, and the model or policy revision. |
| Handoff | Source and destination state, evidence transferred, uncertainty, and the next owner. |
| Approval | Approval bound to the exact action, target, scope, and revision, plus mismatch behavior. |
| External action | Stable operation identity, idempotency evidence, and the declared effect scope. |
| Ambiguous result | Authoritative readback or an explicit `unknown`/`not_evaluated` result before retry. |
| Fallback | Capability, permission, privacy, cost, and quality differences between the original and fallback paths. |
| Memory or state | Revision, provenance, freshness, retention, deletion, and rollback behavior. |
| Evaluation | Separate status for task outcome, safety, evidence completeness, evaluator validity, cost, and latency. |

**Idempotency** means that repeating the same operation identity does not
create an additional unintended effect. **Postcondition reconciliation** means
reading the authoritative destination after an uncertain attempt to determine
what state actually exists.

A fallback is a new evaluated configuration, not automatic recovery. Recovery
requires evidence that the previous failure was understood, the resumed action
remained authorized, and the external state was safe to continue.

## Instead of

Treating a successful model response, approval record, local completion flag, or
eventual retry as proof that the workflow recovered safely.

## Why

The component that coordinates models and tools can change the decision even
when the underlying model stays the same. Keeping routing, approval, state, and
external effects visible prevents a fluent handoff or a second model from hiding
the original failure.

## Official guidance considered

- [Human-in-the-loop](https://openai.github.io/openai-agents-python/human_in_the_loop/),
  OpenAI Agents SDK, accessed 2026-11-22. It documents pausing and resuming
  interrupted runs and approval handling across handoffs.
- [Tracing](https://openai.github.io/openai-agents-python/tracing/), OpenAI
  Agents SDK, accessed 2026-11-22. It documents trace evidence for tool calls,
  guardrails, handoffs, and custom events.
- [Configure permissions](https://docs.anthropic.com/pt/docs/claude-code/sdk/sdk-permissions),
  Claude Agent SDK, accessed 2026-11-22. It documents explicit permission
  modes, allow/deny rules, and runtime approval boundaries.
- [Security best practices](https://modelcontextprotocol.io/specification/latest/basic/security_best_practices),
  Model Context Protocol, accessed 2026-11-22. It documents security risks at
  authorization and proxy boundaries.

## Implementation

- [Agent Evaluation and Release](../AGENT_EVALUATION_AND_RELEASE.md)
- [Claude Evaluation Lab, Lesson 25](https://github.com/julie-elkins/claude-evaluation-lab/blob/main/course/lesson-scripts/lesson-25-agent-trajectories-and-recovery.md)
- [Agent Assurance Harness, Developer Workflow Assurance](https://github.com/julie-elkins/agent-assurance-harness/blob/main/docs/DEVELOPER_WORKFLOW_ASSURANCE.md)
- [AI Teaching Strategies, Evaluate the Control Path](https://github.com/julie-elkins/ai-teaching-strategies/blob/main/strategies/evaluate-the-control-path.md)

## Verification

Use one synthetic interrupted-action case:

1. The first model proposes an approved external action.
2. The result is ambiguous after the action boundary.
3. The workflow restarts and selects a fallback model.
4. The evaluator checks approval binding, operation identity, authoritative
   state, fallback differences, and the terminal decision.

The case must distinguish `recovered`, `unknown`, `unsafe_to_retry`, and
`not_evaluated`. A passing text response is not sufficient.

## Costs if wrong

If the boundary is omitted, a workflow may duplicate an external action, carry
stale approval into a new revision, or treat a lower-capability fallback as
equivalent without evidence. If the boundary is applied too broadly, learners
may mistake every model switch for a production incident. The case therefore
requires a declared decision and synthetic, bounded evidence.

## What this does not prove

This pattern does not prove live model quality, exactly-once execution,
production security, provider equivalence, or safe operation against a real
external system. It establishes what evidence a bounded evaluation must inspect
before making those claims.

## Decided by

Julie Elkins and Codex, 2026-11-22.

## Teaching link

Ask learners to annotate the control path rather than only score the final
answer. They should identify who owns the next decision, what evidence is
missing, and why a retry is or is not safe.
