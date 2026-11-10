# Design decision: make evaluator questions explicit and independently reviewable

## Problem

A single judge prompt can turn several different decisions into one opaque
score. That makes partial correctness hard to represent, lets a blocking tool
failure disappear inside a good final answer, and can charge the same failure
twice when criteria overlap.

## Decision

Teach and require a question-level evaluator contract for rubric-based
evaluation. Each question has a stable ID, named construct, appropriate
question type, evidence reference, weight, blocking rule, and revision. Reports
retain the per-question answer, score, evidence, and contribution to the
aggregate. Calibration is reported per question against independent
human-reviewed cases, not inferred from the aggregate score.

## Instead of

Using one unstructured judge prompt, adopting a provider-specific evaluator
engine as the source of authority, or reporting only one aggregate score.

## Why

Explicit questions make score changes inspectable and give the reviewer a place
to represent partial correctness, hard-fail tool-faithfulness errors, and
uncertainty about the evaluator itself. The overlap audit prevents one defect
from being counted twice without a documented policy reason.

## Official guidance considered

No external source is treated as an approval authority for this pattern. The
decision is a provider-neutral design rule supported by the linked course,
synthetic lab, and existing evaluation-layer guidance.

## Implementation

- [Evaluation research checklist](../EVALUATION_RESEARCH_CHECKLIST.md)
- [Claude Evaluation Lab question-level lesson](https://github.com/julie-elkins/claude-evaluation-lab/blob/main/course/lesson-scripts/lesson-13-llm-as-a-judge.md)
- [Claude Evaluation Lab automated-evaluator lab](https://github.com/julie-elkins/claude-evaluation-lab/tree/main/labs/03-automated-evaluators)

## Verification

Run the public synthetic Lab 3 and confirm that it reports per-question
results, a partial-correctness score, a blocking tool-faithfulness failure, an
overlap warning, and an explicit calibration boundary. Review the checklist
for the corresponding question-contract, calibration, overlap, and revision
controls.

## Costs if wrong

The contract adds design and review work, and a question set can become too
fine-grained to maintain. Keeping the lab synthetic, requiring an overlap
audit, and retaining the aggregate only as a summary limits that cost while
preserving the evidence needed to challenge a score.

## What this does not prove

An explicit question contract does not prove judge accuracy, human agreement,
production safety, or generalization beyond the declared cases, evidence,
revisions, and decision policy.

## Decided by

Julie Elkins, 2026-11-10.

## Teaching link

Start with the course's automated-evaluator lab, inspect the question-level
report, then use the checklist to decide which questions require deterministic
checks, calibrated semantic review, or human adjudication.
