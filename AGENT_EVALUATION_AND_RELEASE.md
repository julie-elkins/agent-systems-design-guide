# Agent evaluation and release

This guide describes how to turn an agent capability into evidence strong enough
to support a release decision.

An **agent evaluation** is a structured test of whether an agent meets a declared
requirement. An **agent** is a system that can plan and take multiple steps, often
through tools. A **release decision** is the human-owned choice to release, hold,
roll back, or continue investigating a revision.

The central question is:

> What decision are we making, what evidence supports it, and what does that evidence
> not prove?

## The evaluation lifecycle

Use one visible path rather than a collection of unrelated scores:

```text
Requirements and failure cost
    -> test cases and expected outcomes
    -> trace capture
    -> component checks
    -> trajectory checks
    -> end-to-end checks
    -> grader validation
    -> result analysis
    -> release decision
    -> monitoring and new test cases
```

The [Agent Assurance Harness](https://github.com/julie-elkins/agent-assurance-harness)
contains the operational checklist and synthetic controls. The
[LLM Eval Validity](https://github.com/julie-elkins/llm-eval-validity) repository
focuses on whether the measurement itself supports the decision.

The design choice to keep the reasoning here and the operational controls in the
harness is recorded in the [evaluation and release decision record](decisions/agent-evaluation-and-release.md).

## 1. Define the decision before the score

Start with the user task and the cost of being wrong.

Record:

- the task or decision the agent is meant to support;
- the actions and sources the agent is allowed to use;
- what must be true before the task counts as complete;
- which actions require confirmation;
- what the agent must do when evidence is missing, stale, or conflicting;
- the failure that would cause the most harm or rework; and
- who owns the release decision.

Separate these outcomes:

- `passed`: the required evidence was observed and the declared rule was met;
- `failed`: the required evidence was observed and the rule was not met;
- `blocked`: a required control or authorization prevented a safe decision;
- `not_evaluated`: the required evidence was missing or invalid; and
- `not_applicable`: the check does not apply to the declared workflow.

Do not convert `not_evaluated` into `passed`. A missing trace is not a clean trace.

## 2. Build and maintain the test set

A **golden dataset** is a versioned set of representative cases with reviewed expected
outcomes, acceptable actions, or acceptable answer ranges. “Golden” means that the
case has a documented review and provenance; it does not mean that it is permanently
perfect or complete.

The set should include:

- normal user tasks;
- boundary and missing-information cases;
- stale, conflicting, or unauthorized sources;
- expected refusals and clarification requests;
- tool errors and recovery paths;
- high-consequence actions;
- multi-turn tasks whose state changes across turns;
- known historical failures; and
- deliberately degraded negative controls.

Keep calibration cases used to tune a grader separate from held-out cases used to
measure it. Version the cases, expected outcomes, policy assumptions, and source
revision together. When a real failure is reviewed, preserve the safe evidence needed
to classify it and convert the failure into a regression case when appropriate.

Do not copy private prompts, raw traces, credentials, or protected content into a
public test set. Use identifiers, bounded metadata, synthetic fixtures, or local-only
artifacts behind an explicit boundary.

## 3. Capture the trace needed for the decision

A **trace** is the structured record of the agent run. The required trace fields depend
on the decision, but commonly include:

- task and scenario identifier;
- system, model, prompt, policy, tool, data, and evaluator revisions;
- conversation turn and state identifiers;
- retrieved-source identifiers and freshness state;
- tool names, bounded arguments, results, and errors;
- intermediate decisions and retries;
- final result and semantic task outcome;
- latency, cost, timeout, and stop-condition state; and
- evaluator status, evidence references, and human-review state.

Retain only what the decision requires. Public or shared artifacts should prefer
identifier-only or metadata-only evidence. A trace can prove what was observed in one
declared run; it does not automatically prove hidden state, generalization, or live
production quality.

## 4. Evaluate at four complementary scopes

The first three are the main evaluation layers. Trajectory is called out separately
because an agent's path can fail even when its final text looks acceptable.

### Deterministic checks

**Deterministic checks** use fixed rules with the same expected result for the same
observed input. Use them for:

- exact or numeric correctness;
- required fields and schema validity;
- tool selection and parameter correctness;
- authorization and confirmation requirements;
- state changes in a controlled environment;
- latency, timeout, and cost ceilings; and
- whether required evidence exists.

These checks are strong for declared facts and boundaries. They do not establish
quality that has not been represented in a rule.

### Semantic checks

**Semantic checks** judge meaning and evidentiary support rather than exact string
identity. Use them for:

- groundedness in approved sources;
- relevance to the user's request;
- completeness and omission of required information;
- unsupported or fabricated claims;
- appropriate uncertainty; and
- clarity when clarity affects the decision.

Model-based grading can scale these checks, but a model judge is another measurement
system, not ground truth. Validate it against human-reviewed examples, inspect
disagreements, measure false passes and false failures, and define when it must
abstain.

### Behavioral checks

**Behavioral checks** evaluate what the agent does across an interaction and environment.
Use them for:

- end-to-end task completion;
- appropriate clarification and refusal;
- recovery after tool or source failure;
- safety against prompt injection and unauthorized requests;
- consistency across repeated runs; and
- user or business outcomes such as rework, escalation, and time to useful result.

Behavioral success must remain separate from operational convenience. A faster answer
that takes an unauthorized action is not an improvement.

### Trajectory checks

A **trajectory** is the sequence of planning steps, tool calls, arguments, intermediate
state changes, retries, and recovery decisions that lead to the result.

Check whether:

- the agent used the permitted source and tool;
- the call order respected dependencies;
- arguments were complete and authorized;
- intermediate evidence supported the next step;
- the agent recovered or stopped when a tool failed;
- confirmation occurred before a high-risk action; and
- the final answer accurately represented what happened.

A correct final answer can hide a bad trajectory. A correct trajectory can still produce
a bad final answer when the source data is incomplete. Keep both facts visible.

## 5. Test multi-turn and stateful behavior

Single-turn cases are insufficient when the agent must remember constraints, maintain
state, or act over time.

A realistic multi-turn case should declare:

- the initial user goal;
- facts or constraints introduced later;
- state that must persist;
- state that must not persist;
- allowed and disallowed actions at each turn;
- expected clarification or confirmation points;
- the terminal success or refusal condition; and
- what evidence proves that the terminal state was reached.

Evaluate both the individual turns and the conversation-level outcome. A conversation
can fail even when every isolated response looks reasonable because the agent forgot
an earlier restriction, repeated an action, used stale context, or claimed completion
before the state change was confirmed.

## 6. Select and validate the grader

Use the least subjective grader that can answer the question:

| Grader | Best use | Main limitation |
| --- | --- | --- |
| Code-based | Exact facts, schemas, tool parameters, state, counts, and thresholds | Cannot judge an unrepresented semantic property |
| Model-based | Groundedness, relevance, completeness, and other rubric-shaped qualities | Can be biased, inconsistent, or overconfident |
| Human | Ambiguous, high-risk, novel, or subjective cases | Costs time and may disagree without calibration |

Use human-reviewed calibration cases to measure a model-based grader. Record agreement,
disagreement categories, false passes, false failures, and the conditions under which
the grader must return `not_evaluated`.

Do not let the same unexamined assumption define the agent, the judge, and the release
threshold. An independent breaker—a check designed to expose a false pass—should
challenge at least the highest-risk positive path.

## 7. Interpret metrics as evidence, not verdicts

For each reported result, inspect:

- the baseline and comparison revision;
- sample size and uncertainty;
- failure categories and high-risk slices;
- repeated-run variation;
- latency, cost, and timeout changes;
- false passes and false failures;
- evaluator agreement and calibration state; and
- whether one metric improved while a higher-priority metric regressed.

The metric tells the reviewer where to look. The trace, expected state, and comparison
with the baseline explain what happened. A small score movement is not automatically a
real improvement; it must exceed the declared evaluation variation or be described as
inconclusive.

## 8. Make the release rule explicit

Before inspecting the result, write the release contract:

- blocking checks and thresholds;
- checks that are informative but not release blockers;
- required evidence completeness;
- acceptable variance and uncertainty;
- high-risk slices that cannot be averaged away;
- owner and escalation path;
- rollback or containment condition; and
- what happens when the evaluator itself is unavailable or inconclusive.

Keep `passed`, `failed`, `blocked`, and `not_evaluated` separate in reports. Human
approval may accept a documented residual risk where policy permits, but approval must
not rewrite missing evidence as a pass.

## 9. Close the production feedback loop

Use this path after release:

```text
Real failure
    -> preserve safe evidence
    -> review the failure category
    -> add or revise a test case
    -> rerun the relevant baseline
    -> make a release or remediation decision
    -> monitor the changed behavior
```

For an internal sales agent, include outcomes beyond model quality:

- time to useful answer;
- user rework;
- escalation and clarification rate;
- decision completion;
- incorrect-action rate;
- stale-information rate;
- repeated-run reliability; and
- cost and latency within the workflow's usable range.

These are product evidence, not substitutes for tool correctness, authorization, or
grounding checks.

## What this guide proves and does not prove

Following this lifecycle makes the decision contract, evidence, evaluator limits, and
release boundary visible. It does not prove live model quality, production safety,
generalization to unseen users, or business impact. Those claims require evidence
matched to their own scope.

## Related material

- [Agent Assurance Harness evaluation checklist](https://github.com/julie-elkins/agent-assurance-harness/blob/main/AGENT_EVALUATION_CHECKLIST.md)
- [LLM Eval Validity](https://github.com/julie-elkins/llm-eval-validity)
- [OpenAI Evals](https://github.com/openai/evals)
- [Anthropic trustworthy agents](https://www.anthropic.com/research/trustworthy-agents)
