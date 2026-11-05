# Stateful memory evaluation

Use this pattern when an agent reads or writes durable memory, session
summaries, user preferences, or background-consolidated state.

**Stateful memory** means information that can affect a later run. A memory
writer is therefore part of the system under test even when it runs in the
background.

## The design question

Do not ask only whether the memory workflow completed. Ask whether the
candidate memory change improves the declared future decision while preserving
authorization, privacy, provenance, deletion, and rollback.

Write down:

- the task and future decision;
- the baseline memory condition;
- the candidate memory condition;
- which prior events may be read;
- which information may be written;
- who may read, correct, or delete it;
- the evidence that proves a later task benefited;
- the high-risk failure that must block adoption; and
- the expiry, rollback, and deletion behavior.

## Baseline and treatment

Compare a stable baseline with one candidate treatment:

| Condition | Meaning |
| --- | --- |
| Baseline | The approved memory snapshot, or no newly consolidated memory |
| Treatment | The same workflow with the candidate memory writer or schema |

Keep the model, prompt, tools, policy, task set, and environment constant
unless the decision explicitly concerns one of them. Separate cases used to
design the memory workflow from fresh and held-out cases used for the adoption
decision.

The memory writer's successful completion is not future-task evidence. The
later task supplies the evidence.

## Four evidence surfaces

Evaluate four surfaces separately:

1. **Write correctness** — eligible, supported, useful information was stored.
2. **Read correctness** — the later agent retrieved relevant memory and ignored
   irrelevant memory.
3. **Task outcome** — the declared task became more reliable, complete, or
   appropriately escalated.
4. **State safety** — authorization, tenant isolation, privacy, retention,
   provenance, expiry, deletion, and rollback remained correct.

Useful metrics are decision-specific. Examples include memory retrieval
precision and recall, stale-memory rate, contradiction rate, unsupported-memory
rate, task completion, latency, token cost, storage cost, and human-review
rate. Keep these findings separate; efficiency cannot compensate for a safety
failure.

## Negative controls

For every positive memory path, add a deliberately degraded case:

- an expired source is accepted;
- memory crosses a tenant or user boundary;
- a current policy conflicts with stored memory;
- an unsupported personal detail is written;
- relevant memory is not retrieved;
- irrelevant memory changes the plan;
- deletion leaves a derived copy active; or
- aggregate improvement hides a high-risk held-out regression.

The evaluation is not ready for release use until it catches the failure it was
designed to catch.

## Human approval boundary

The evaluator may recommend retain, revise, block, or roll back. It must not
silently publish a memory schema, approve retention of sensitive data, or
delete records. Those are human-owned decisions unless a separately approved
policy explicitly delegates them.

## What this proves

This pattern proves that one declared memory change was compared against a
baseline with explicit safety and future-task evidence. It does not prove that
memory improves every task, that a provider cookbook transfers to production,
or that one successful experiment remains valid after a model, policy, schema,
data, or retrieval change.

## Related implementation

The public [Agent Assurance Harness](https://github.com/julie-elkins/agent-assurance-harness)
contains a bounded synthetic memory-policy lane. The public course's
[memory-consolidation evaluation extension](https://github.com/julie-elkins/claude-evaluation-lab/blob/main/course/MEMORY_CONSOLIDATION_EVALUATION.md)
provides the learner-facing walkthrough.
