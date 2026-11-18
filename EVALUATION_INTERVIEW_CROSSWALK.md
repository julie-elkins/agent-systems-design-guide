# Evaluation Interview Questions → Design Decisions

This crosswalk helps a learner turn common evaluation-interview questions into
observable design choices. It is a reasoning aid, not a list of approved answers.

| Interview question | Design decision to make | Evidence to inspect |
|---|---|---|
| What decision does the evaluation support? | Name the release, screening, rollback, or research decision before choosing a metric. | Decision owner, threshold, failure cost, and limitation. |
| What is the unit of analysis? | Choose the case, turn, trajectory, user, task family, or repository that can vary independently. | Sampling plan and dependence assumption. |
| Are the observations paired or clustered? | Use paired comparisons for matched cases and cluster methods for correlated groups. | Pairing key, cluster key, and uncertainty method. |
| What does the confidence interval change? | Judge the decision against the bound, not only the point estimate. | Interval, target tolerance, and resolvable difference. |
| How do you validate an LLM judge? | Compare it with independent human raters and measure repeatability. | Human-human ceiling, judge-human agreement, repeatability range, and abstention. |
| When is a judge suitable for release? | Require stricter evidence than for screening or triage. | Declared release rule, bias probes, negative controls, and adjudication record. |
| Did the RAG system fail in retrieval or generation? | Separate retrieval recall, context sufficiency, grounding, and citation correctness. | Ranked documents, assembled context, claims, and supporting identifiers. |
| What happens after interruption or handoff? | Preserve the task contract and evidence contract across recovery. | Recovery state, terminal outcome, trajectory artifact, and claim-support record. |
| What does the benchmark not prove? | Write the boundary before publishing the result. | Limitation statement and excluded claims. |

## How to use this

For each question, write one sentence for the decision, one for the rejected
alternative, and one for the verification. Then connect the result to the
[AI Evaluation Engineer Learning Path](https://github.com/julie-elkins/agent-evaluation-learning-path/blob/main/EVAL_ENGINEER_LEARNING_PATH.md).

The design guide remains provider-neutral. Provider documentation or a model
feature may change the implementation, but it should not silently change the
evidence required for the decision.

## What this proves

The crosswalk makes the reasoning behind an evaluation design visible and gives
learners a way to practice answering interview questions with evidence.

## What this does not prove

Answering a design question correctly does not prove that an implementation is
correct, that a model is reliable, or that a production decision is safe.
