# Choosing an evaluation layer

AI evaluation is not one professional practice. The right method depends on the
decision the evidence must support.

An **evaluation layer** is the scope at which a system is measured: a research
study, an application or agent workflow, a production release, or behavior in
real use. These layers can compose, but they should not be treated as
interchangeable. A research result is not automatically a release decision, and
a passing release gate is not a measurement of broad model capability.

## The layers at a glance

| Layer | Primary question | Typical evidence | Decision it may support | What it does not prove |
| --- | --- | --- | --- | --- |
| Research study of model behavior | Does the study measure the declared capability or behavior with enough validity and precision? | Hypothesis, protocol, versioned cases, reference decisions, repeated runs, uncertainty, and limitations | A bounded research conclusion or a decision about the next experiment | Production safety, broad generalization, or product impact |
| Application or agent evaluation | Does this application or agent complete the declared task within its tools, policies, and failure boundaries? | Task outcomes, traces, tool calls, authorization, refusals, recovery, grader calibration, and negative controls | A diagnostic, comparison, or scoped engineering decision | Live production reliability or quality outside the tested workflow |
| Production assurance and release | Is the exact revision supported by the evidence required before release or continued operation? | Revision binding, evidence completeness, blocking controls, release thresholds, human ownership, rollback, and reauthorization | Release, hold, rollback, or continued-operation decision | General model quality, complete security approval, or absence of future incidents |
| Monitoring and feedback | Has behavior or its consequences changed under real use? | Safe operational signals, drift, incidents, user outcomes, reviewed failures, and new regression cases | Investigation, remediation, re-evaluation, or a change to the release boundary | A substitute for pre-release validity or controlled testing |

## Choose the layer from the decision

Before selecting a tool or benchmark, write the decision in one sentence:

> We need to decide whether ______________________________.

Then answer:

1. **What is the unit?** A response, tool call, trajectory, session, complete
   task, release artifact, or population-level outcome?
2. **What is the failure cost?** What would make the decision materially wrong,
   unsafe, or expensive to reverse?
3. **What evidence is required?** Include the reference, revision, environment,
   evaluator, and human-review state needed for that decision.
4. **What must remain separate?** Do not collapse correctness, safety,
   authorization, evidence completeness, latency, cost, and user outcome when
   they have different consequences.
5. **What remains unestablished?** State what the evaluation did not test or
   cannot conclude from its evidence.

The same system may use more than one layer. For example, a release workflow may
depend on a research study for evaluator validity, an agent evaluation for task
and trajectory behavior, and monitoring for drift after release. Each result
retains its own claim boundary.

## Research study of model behavior

A research study asks whether a declared construct or behavior was measured
appropriately. **Construct validity** means that the evaluation measures the
capability or quality it claims to measure. The study should make its hypothesis,
protocol, case construction, reference standard, sampling, uncertainty, and
limitations visible.

Use this layer when the primary output is a bounded research observation or when
the next decision is about the measurement itself. A larger case count cannot
repair an unclear construct, a contaminated case set, an invalid scoring rule,
or an evaluator that has not been calibrated.

The [Evaluation Research Checklist](EVALUATION_RESEARCH_CHECKLIST.md) and
[Research Design Guide](RESEARCH_DESIGN_GUIDE.md) are the canonical patterns
for this layer.

## Application or agent evaluation

An application or agent evaluation asks whether a complete workflow behaves
within a declared task and boundary. It examines more than the final answer:
tools, permissions, intermediate state, retries, refusals, recovery, and the
safe terminal state may all matter.

The **system under test** is the complete workflow that supports the decision:
model, harness or scaffolding (the prompts, tools, permissions, and
orchestration around the model), repository and environment, evaluator, task
outcome, trace completeness, retry behavior, and side effects. A harness change
can change the measured result without changing the model, so these sources of
variation should be recorded and interpreted separately.

Use this layer when the decision concerns a workflow, agent, prompt, tool
configuration, policy, or candidate implementation. Keep deterministic checks,
semantic grading, behavioral outcomes, and trajectory evidence separate.

The evaluation may support a comparison or engineering decision, but it should
not be promoted to a production claim without the additional release evidence
required by the next layer.

For repeated coding-agent runs, report **Pass@1**, meaning success on one
attempt; **Pass@k**, meaning success on at least one of `k` attempts; **Pass^k**,
meaning success on every one of `k` attempts; and **flip rate**, meaning the
share of tasks whose outcome changes across attempts. Pass@k answers whether a
retry budget can find a success. Pass^k answers whether the workflow is
predictable without choosing the best run.

Use a preservation-aware rule: an aggregate score increase is not an
improvement when previously covered tasks regress and the regression is hidden,
unexplained, or outside the accepted decision rule. Inspect failed attempts for
unintended repository changes before calling a retry recovered, preserve
newly exposed failures as regression cases, and keep any accepted tradeoff
human-owned.

## Production assurance and release

Production assurance asks whether the exact revision under review has the
evidence required for a release, hold, rollback, or continued-operation
decision.

**Revision binding** means connecting the evidence to the exact model, prompt,
tool, policy, data, evaluator, harness, environment, and repository revisions
that were tested. **Reauthorization** means an explicit decision that existing
evidence still supports use after a relevant change.

This layer requires a release contract written before results are interpreted,
blocking and informative checks kept separate, missing evidence kept visible,
and a named human owner for the decision. A green score without evidence
completeness is not a release pass.

The [Agent Assurance Harness checklist](https://github.com/julie-elkins/agent-assurance-harness/blob/main/AGENT_EVALUATION_CHECKLIST.md)
is the operational companion for this layer.

## Monitoring and feedback

Monitoring asks whether behavior, inputs, dependencies, or consequences changed
after release. It should preserve safe metadata, distinguish operational signals
from model-quality claims, and turn reviewed failures into new cases or
re-evaluation work.

Monitoring is not a substitute for pre-release evaluation. A quiet dashboard
may mean that no signal was collected, not that no failure occurred. A useful
monitoring path therefore states its coverage, freshness, missing-data behavior,
and escalation boundary.

## Do not compare unlike layers as one score

The most common category error is comparing a research workspace, an agent
evaluation suite, and a release-assurance harness as though they were competing
versions of the same product.

Before comparing tools, normalize:

- the decision being supported;
- the unit being measured;
- the evidence retained;
- the evaluator and reference standard;
- the change and version boundary;
- the human decision boundary; and
- the claim the result is allowed to make.

The strongest system is not the one with the longest feature list. It is the one
whose evidence matches the decision and whose limitations remain visible.

## Portfolio map

- [Agent Evaluation and Release](AGENT_EVALUATION_AND_RELEASE.md) explains the
  workflow and release reasoning.
- [Agent Assurance Harness](https://github.com/julie-elkins/agent-assurance-harness)
  demonstrates synthetic application evaluation and production-assurance
  controls.
- [LLM Eval Validity](https://github.com/julie-elkins/llm-eval-validity)
  contains research studies of measurement validity and reliability.
- [Agent Evaluation Learning Path](https://github.com/julie-elkins/agent-evaluation-learning-path)
  provides the learner route across the layers.

## What this guide does not prove

This classification helps choose an evaluation scope. It does not prove model
quality, production safety, security approval, business impact, or generalization
to unseen users. Those claims require evidence matched to their own decision and
risk boundary.
