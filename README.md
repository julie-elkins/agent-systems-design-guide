# Agent Systems Design Guide

Reusable design patterns for building AI agents, evaluations, technical-content
systems, MCP servers, and developer tools that can be inspected before they reach
production.

For the complete family of public design guides and the operational material connected
to them, start at the [AI Design Guides hub](https://github.com/julie-elkins/ai-design-guides).
This repository remains the general pattern guide; the hub is the directory, not a copy
of this content.

This guide sits above the implementation repositories:

- [Agent Assurance Harness](https://github.com/julie-elkins/agent-assurance-harness)
  demonstrates the patterns in executable, synthetic controls.
- [MCP Server Design Guide](https://github.com/julie-elkins/mcp-server-design-guide)
  applies the same discipline specifically to MCP tool contracts.

The central failure this guide designs against is not an obvious crash. It is a
partial, stale, guessed, refused, or unauthorized result being presented as
complete and safe.

## The design question before the code

Before adding an agent, tool, server, workflow, or evaluation, state:

- who calls it;
- what decision it enables;
- what evidence is required before that decision;
- what it must refuse;
- what remains private;
- how another person can run it;
- how a known failure proves the control still works; and
- how the change can be rolled back.

If those answers are not written down, the build is not yet scoped. More code will
not repair an unclear contract.

## Pattern catalog

### 1. Evidence contract

Define the observable facts required before an agent may claim success. Keep task
outcome, evidence completeness, abstention, authorization, and evaluator validity
as separate dimensions.

Why: one composite score can make an unsafe result look successful because the task
answer was plausible.

### 2. Refusal as a result

Make abstention a structured outcome. An agent should be able to say that evidence
is missing, stale, from the wrong source, conflicting, or ambiguous, with a reason
that tells the next operator what to repair.

Why: a blank result is often mistaken for a clean result, while a refusal keeps the
uncertainty visible.

### 3. Metadata-only boundary

Pass identifiers, states, counts, hashes, and bounded locations across the public
or CI boundary. Process source bytes in memory and discard them before reporting.

Why: the control can be demonstrated without turning logs, artifacts, or reports
into a second copy of private or copyrighted material.

### 4. Deliberately degraded negative control

For every positive path, add a known-bad path that removes, misbinds, stales, or
fabricates required evidence. The test passes only when the evaluator catches it.

Why: a green test suite proves that checks ran; a caught negative control proves the
checks can fail for the property they are meant to protect.

### 5. Launch-to-field enablement pack

Represent the path from a changing technical capability to field-ready material as
one bounded pack:

- demo;
- hands-on lab;
- quick-reference guide;
- audience;
- source and freshness contract;
- claim-support state;
- usage or learner feedback; and
- human approval state.

Why: a demo that only raises awareness is different from a lab another technical
person can deliver without the author. The contract makes that difference visible.

### 6. Human approval boundary

Keep automated recommendation separate from approval, editing, and publication.
Untrusted claim proposals may enter evidence evaluation, but they cannot authorize
content changes.

Why: generated content can be useful before it is trustworthy. The workflow must
retain that distinction.

### 7. Shared human/agent handoff

When people and agents work on the same Markdown or other editable artifact, keep the
repository or other canonical source — the authoritative copy — as the place where the
accepted version lives. Treat the shared document as a review surface, and record:

- the source revision or commit the handoff started from;
- who owns each section;
- which sections an agent may edit and which require a person;
- how competing edits are detected and resolved; and
- whether a person approved the result before it returns to the canonical source.

Comments and a visually clean document are not approval. The handoff is complete only when
the reviewed change has been merged back into the canonical source and its revision is
recorded.

Why: one shared file can remove copy-and-paste drift, but without ownership and revision
identity an agent can consume an older human edit or overwrite work that was already
accepted.

What this proves: the workflow made ownership, revision, conflict handling, and approval
visible for one handoff. It does not prove that the shared document is current, that no
unseen edit exists, or that the approved content is correct outside the declared review.

### 8. Provider-neutral adapter

Keep the agent contract independent from the model provider. Provider adapters
translate into the same bounded evidence shape and carry explicit request, token,
time, and spend ceilings when live execution is permitted.

Why: provider changes should not silently change what counts as safe evidence.

### 9. Self-service path

Every reusable workflow needs one command that another person can run, a positive
example, a failure example, and a statement of what the result does not prove.

Why: a system that only works while its author explains it does not scale quality.

### 10. Feedback-to-revision loop

Record safe usage signals and map them to an action such as retain, clarify, revise,
or retire. Keep the signal separate from the content itself.

Why: scale requires knowing which material is working, where people get stuck, and
what should stop consuming maintenance time.

### 11. Solo-maintainer delivery path

When there is no second reviewer, use explicit scope, deterministic fixtures,
negative controls, boundary scans, release evidence, and documented risk acceptance.
Call it solo review; do not imply that automation is another human.

Why: the absence of a reviewer must not become the absence of a release decision.

### 12. Instructor calibration

Use a fixed observer rubric to evaluate technical accuracy, hands-on execution,
live debugging, audience adaptation, and safety behavior. Record stable signals,
not classroom prose, and make missing behaviors produce coaching actions rather
than a passing score.

Why: a train-the-trainer program cannot scale quality if readiness depends on
one person's undocumented memory of what good looked like.

### 13. Transport success is not task success

Record transport status, semantic task outcome, retry count, and bounded usage as
separate observations. A successful request may still return empty context, an
incomplete result, or an outcome that requires escalation. A workflow must not report
success from transport status alone.

Why: dashboards that show only request success can hide the failure that matters to
the person or system waiting for the task to finish.

What this proves: the workflow recorded and evaluated a declared semantic outcome
separately from transport success. It does not prove that the semantic outcome is
correct outside the declared contract.

### 14. Builder--breaker evaluation

Separate the component that proposes or changes a workflow from an independent breaker
that tries to expose a false pass. Keep evaluator validity as its own evidence
dimension, and retain the degraded case that the breaker must catch.

Why: an evaluator that shares the builder's assumptions can approve the same failure
the builder introduced. A second pass is not independent merely because it runs in a
different function.

What this proves: the known degraded case was caught by the independent evaluation
path. It does not prove that the breaker will find every future failure.

### 15. Evidence-to-claim discipline

Treat **epistemology** -- what can be justified from evidence -- as part of the system
design. Separate three states:

- **Observed:** what the trace, test, or evaluation directly recorded;
- **Inferred:** what those observations support within the declared task, fixture,
  population, and revision scope; and
- **Unestablished:** what was not tested or cannot be concluded from the available
  evidence.

A successful task can establish that one declared run reached one declared outcome. It
does not, by itself, establish stable behavior under unseen inputs, broad capability,
or the system's internal state. A blocked tool call can establish that the authorization
boundary acted. It does not establish that the model was aligned or that the input was
safe.

Why: a result becomes misleading when an observation is silently promoted into a claim
with a wider scope than the evidence supports. The remedy is to name the observation,
the inference, the assumptions that connect them, and the boundary beyond which the
claim is not established.

What this proves: the review kept observations, scoped inferences, and unresolved
claims separate. It does not prove hidden state, emergent behavior, or generalization
outside the tested conditions.

### 16. Decision reasoning as a teachable artifact

Make the reasoning behind a consequential design choice visible beside the code. A
teachable decision record is not a diary and it is not a claim that one design is
universally correct. It is a compact path another engineer can follow:

1. the problem and the failure if it is designed badly;
2. the alternatives considered;
3. the selected decision;
4. the official guidance or source evidence considered;
5. the implementation and verification that followed; and
6. the cost, limitation, and boundary of the result.

Use the [decision reasoning template](DESIGN_DECISION_TEMPLATE.md). Link each record to
the relevant implementation, test, runnable example, and authoritative source. Keep
the learner-facing explanation in plain language, and keep implementation-specific
details in the repository that actually owns them.

Why: code shows what was built, but a visible decision trail teaches how to reason about
AI engineering under constraints. It also lets a reviewer distinguish an observed test
result from a broader claim about production reliability.

What this proves: the author made the design assumptions, alternatives, verification,
and limits inspectable. It does not prove that the choice is optimal, that every
official recommendation was followed, or that the result generalizes beyond its tested
scope.

### 17. Agent-readable repository lifecycle

When an agent works across sessions, make the repository answer five operational
questions without relying on chat history:

- **Instructions:** Where does a new session start, and which rules must it read?
- **State:** What is the current task, what is complete, what is blocked, and what
  decision or handoff must survive the session boundary?
- **Verification:** Which runnable command produces evidence before the agent may claim
  completion?
- **Scope:** What is the one bounded unit of work, what does "done" mean, and which
  dependencies or exclusions limit it?
- **Lifecycle:** What must be checked at startup, and what clean handoff must be left
  before the session ends?

Typical artifacts include `AGENTS.md` or `CLAUDE.md`, a progress or feature-state file,
a verification entrypoint, and a session-handoff note. These are examples, not required
filenames: the contract is the visible startup path, state, scope, evidence, and handoff.

Why: an agent that cannot reconstruct the task and its evidence from the repository will
re-explore, guess, or declare work complete from memory. The cost is repeated work and
stale decisions being carried into the next session.

What this proves: a fresh session can locate the task, constraints, verification path,
and next step. The presence of those artifacts is **structural evidence** -- evidence
that the parts exist. It does not prove that the harness improves agent behavior.
That stronger claim needs representative before-and-after sessions or controlled replay
with the same task and explicit outcome measures.

### 18. Evaluation layers and interaction scope

Evaluate an agent at more than one scope:

- **Deterministic checks** use fixed rules for exact results, schemas, tool selection,
  parameters, authorization, state, latency, cost, and evidence completeness.
- **Semantic checks** assess meaning and support, such as groundedness, relevance,
  completeness, and unsupported claims.
- **Behavioral checks** assess task completion, refusal, recovery, safety, repeated-run
  consistency, and user or business outcomes.
- **Trajectory checks** inspect the sequence of planning, tool calls, intermediate
  state, retries, and recovery decisions.

Test multi-turn workflows when state or constraints can change across turns. A final
answer can be plausible even when the agent forgot an earlier restriction, used the
wrong tool, repeated an action, or claimed completion before the state changed.
Conversely, a valid trajectory can still produce a bad result when its source evidence
is incomplete.

Choose the least subjective grader that can answer the question. Validate model-based
graders against human-reviewed cases, measure disagreement, and keep `not_evaluated`
separate from `passed`. Interpret scores with the baseline, sample size, uncertainty,
high-risk slices, repeated-run variation, latency, cost, and user outcomes.

The detailed lifecycle and release reasoning are in
[Agent Evaluation and Release](AGENT_EVALUATION_AND_RELEASE.md). The operational
checklist lives in the [Agent Assurance Harness](https://github.com/julie-elkins/agent-assurance-harness/blob/main/AGENT_EVALUATION_CHECKLIST.md).

## How to use this guide

For a new build:

1. Choose the smallest pattern set that changes the design.
2. Write the evidence contract and the failure that would otherwise pass unnoticed.
3. Build one complete vertical path.
4. Add a degraded control before expanding the feature.
5. Keep private or provider-specific material behind an explicit local boundary.
6. Publish the runnable command and the limitation statement.
7. Record the decision, alternative, reason, cost if wrong, and decision owner.
8. Add the decision to the learning path when another engineer could reasonably choose
   a different design.

For coding-agent work that spans sessions, apply the agent-readable repository lifecycle
before adding memory, automation, or multi-agent coordination. Add only the artifacts
needed to make the next session restartable and the completion claim checkable.
9. For an agent evaluation, run the [Agent Evaluation and Release](AGENT_EVALUATION_AND_RELEASE.md)
   lifecycle and complete the [operational checklist](https://github.com/julie-elkins/agent-assurance-harness/blob/main/AGENT_EVALUATION_CHECKLIST.md).

The guide is intentionally not a framework or a scoring system. It is a set of
design decisions that should remain visible when implementation details change.

## Research design guide

Use the [Research Design Guide for AI Systems](RESEARCH_DESIGN_GUIDE.md) when the
deliverable is more than an implementation: a pilot, evaluation study, working paper,
or public research artifact. It explains how to move from a research gap to a
protocol, negative control, reproducible result, and bounded claim.

The current worked example is the
[agent reauthorization study](https://github.com/julie-elkins/llm-eval-validity/tree/main/research/agent-reauthorization).
The executable pilot remains in the
[Agent Assurance Harness](https://github.com/julie-elkins/agent-assurance-harness).

For a hands-on external curriculum covering repository harnesses, session continuity,
verification, loops, and graph workflows, see
[Learn Harness Engineering](https://github.com/walkinglabs/learn-harness-engineering).
Its structural audit is useful as a checklist, but its score is not behavioral evidence
that an agent became more reliable.

## Scope

This public guide contains no credentials, private course content, unpublished exam
material, provider prompts, production traces, or internal URLs. The examples point
to public synthetic implementations and official public documentation boundaries.

MIT License.
