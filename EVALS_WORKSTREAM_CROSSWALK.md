# Evaluation workstream crosswalk

This page maps a common evaluation-team shape to the public artifacts in this
portfolio. A **workstream** is a distinct operational lane with its own
evidence, owner, and decision boundary.

The LinkedIn post that prompted this page describes an “Anthropic-style AI
Evals Team” with ten workstreams. This crosswalk treats that post as a useful
capability map, not as an official Anthropic specification or validated
research framework.

## Capability map

| Workstream | What it does | Current public evidence | Boundary |
| --- | --- | --- | --- |
| Failure Lab | Finds representative, boundary, and known failure cases. | [Agent Assurance Harness](https://github.com/julie-elkins/agent-assurance-harness) synthetic scenario packs, negative controls, and failure-to-regression workflow. | Synthetic cases demonstrate control behavior; they do not estimate live production prevalence. |
| Trace Review | Reconstructs what happened from a bounded run record. | Harness trace contracts, metadata-only reports, monitorability workbench, and the course trace-to-evaluator skill. | Public artifacts intentionally exclude raw prompts, production traces, and private payloads. |
| Rubric Studio | Defines what “good,” “unsafe,” incomplete, or review-required means. | [LLM Eval Validity](https://github.com/julie-elkins/llm-eval-validity), the [evaluation research checklist](EVALUATION_RESEARCH_CHECKLIST.md), and Claude Evaluation Lab lessons on rubrics and judge calibration. | A rubric is a measurement contract, not proof that the construct is complete or correct. |
| Label Operations | Runs reviewer sampling, calibration, disagreement review, adjudication, and label provenance. | The research paper’s human-review phase, reviewer forms, inter-rater guidance, and course lessons 5–9. | There is not yet a hosted annotation queue or production labeling service; that is an operating-model extension, not a missing research result. |
| Judge Builder | Builds and validates model-based evaluators. | Research judge-validation reports, layered evaluator patterns, structured-output checks, and the harness evaluator checklist. | An LLM-as-a-judge (one language model grading another model’s output) remains a calibrated measurement component, not ground truth. |
| Tie-Break and Adjudication | Resolves disagreement without hiding uncertainty. | Human-review, disagreement, adjudication, and explicit `not_evaluated` or `review_required` states across the paper, harness, and course. | Adjudication decisions still require an accountable human owner for consequential use. |
| Red Team | Searches for unsafe, unauthorized, misleading, or exploitable behavior. | Harness security and incident-response labs, synthetic prompt-injection cases, and attack-to-regression guidance. | Public red-team examples are bounded and synthetic; they are not a claim of complete threat coverage. |
| Regression Gate | Stops known failures from returning after a change. | Harness replay, Golden Trace, adapter regression, contract-drift, and release-gate workflows. | A passing gate covers declared checks and revisions only; it does not prove general safety. |
| Agent Arena | Compares flat and multi-agent architectures under matched conditions. | Planned research direction documented in the response-quality study and related design decisions. | The comparison must hold cases, tools, permissions, budget, and stopping rules constant before it can support an architecture claim. |
| Production Watch | Monitors evaluator health, drift, feedback, and new failure cases. | Harness monitorability, drift, telemetry, incident-response, and trace-to-regression materials; learner course Labs 6–8. | The public path is offline or synthetic and does not claim to operate a live production watch service. |

## If this were operated as a team

The portfolio also shows the operating model that would sit behind these
lanes:

| Role | Primary responsibility | Evidence they own |
| --- | --- | --- |
| Evaluation lead | Defines the decision, claim boundary, failure cost, and release contract. | Evaluation plan, thresholds, uncertainty, and release disposition. |
| Rubric and label-operations lead | Maintains the rubric, reviewer calibration, sampling, disagreement routing, and adjudication record. | Versioned rubric, reviewer agreement, label provenance, and unresolved queue. |
| Judge-validity lead | Tests judge bias, repeatability, false passes, false failures, and abstention. | Calibration report, bias probes, disagreement analysis, and judge limitations. |
| Failure and red-team lead | Converts important failures into bounded regression cases. | Failure taxonomy, synthetic controls, threat cases, and regression links. |
| Evaluation-platform lead | Maintains traces, revision binding, cost and latency ceilings, and reproducible execution. | Run manifest, trace contract, evaluator version, and resource report. |
| Release or product owner | Decides whether evidence supports release, hold, rollback, or more review. | Human disposition, owner, follow-up date, and rollback or containment path. |

This is why the repositories and paper are still valuable. They are the
inspectable artifacts of the operating model: they show how the team would
define work, preserve evidence, challenge evaluators, and make bounded release
decisions. They are not a substitute for staffing or ownership.

## What this crosswalk does not claim

- It does not claim that the LinkedIn post describes an official Anthropic
  organization or production implementation.
- It does not claim that every workstream is a deployed service.
- It does not turn synthetic demonstrations into live production evidence.
- It does not change the frozen provider results in the response-quality study.
- It does not imply that an evaluation team can replace product, security,
  privacy, incident-response, or release ownership.
