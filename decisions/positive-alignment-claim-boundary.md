# Design decision: keep positive alignment separate from response-quality claims

## Problem

An evidence-grounded response-quality study can be mistaken for a complete
alignment evaluation if it reports safe handling, human review, or evidence
support without stating which human-value outcomes were not measured.

## Decision

Keep positive alignment as a separately named future research surface. Add
explicit checklist questions for uncertainty, human decision authority, value
conflict, pluralism, correction, and longer-term outcomes, while keeping the
current response-quality pilot focused on evidence-grounded task performance.

## Instead of

Adding value-sensitive cases to the frozen pilot after data collection, or
describing evidence grounding and refusal behavior as proof of human
flourishing or complete alignment.

## Why

The current study can make a clearer and more credible claim when its measured
construct—what the evaluation actually tests—is separated from broader
alignment questions. The future dimensions are useful design requirements, but
introducing them now would change the research question and make the existing
comparison harder to interpret.

## Official guidance considered

- [Positive Alignment: Artificial Intelligence for Human Flourishing](https://arxiv.org/abs/2605.10310),
  arXiv, accessed 2026-11-19.
- [Evaluation research checklist](../EVALUATION_RESEARCH_CHECKLIST.md).

## Implementation

- [Evaluation research checklist](../EVALUATION_RESEARCH_CHECKLIST.md)
- [LLM Eval Validity response-quality protocol](https://github.com/julie-elkins/llm-eval-validity/tree/main/research/response-quality)

## Verification

The checklist requires separate treatment of safety, authorization, evidence,
uncertainty, value conflict, human authority, and user outcomes. The
response-quality protocol states that positive alignment and flourishing are
outside the frozen pilot's claim boundary.

## Costs if wrong

Keeping the dimensions separate may delay a broader alignment study. Collapsing
them too early would create a stronger risk: readers could mistake a bounded
synthetic evidence result for evidence about real users, social values, or
long-term human benefit.

## What this does not prove

The checklist and protocol do not establish that any model is aligned, safe in
production, beneficial to users, or capable of preserving every legitimate
value. They define what must be measured before making those claims.

## Decided by

Julie Elkins, with Codex implementation support, 2026-11-19.

## Teaching link

Start with the response-quality protocol, mark the constructs it measures, then
use the alignment questions to design a separate future study without changing
the frozen pilot's cases or results.
