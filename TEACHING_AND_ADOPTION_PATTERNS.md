# Teaching and adoption patterns

This page is the reusable teaching layer for the public design guides. It is
not a fourth course: the [AI Teaching
Strategies](https://github.com/julie-elkins/ai-teaching-strategies) repository
holds the strategy cards, while the course and harness repositories hold the
lessons and runnable controls.

## The teaching contract

Before expanding a lesson, lab, or video, write down:

- the audience and the decision they need to make;
- the failure that the teaching move should expose;
- the smallest demonstration and degraded case;
- the learner artifact or behavior that will count as evidence;
- the facilitator calibration step;
- the privacy and accessibility boundary; and
- what the evidence does not prove.

This is the same evidence-to-claim discipline used in system design. A
completion click is delivery evidence. It is not proof that the learner can
perform the target task. A positive comment can guide topic selection. It is not
proof of learning or production impact.

## Reusable patterns

1. **Audience calibration:** start with the learner's decision and prerequisite
   gap before selecting technical depth.
2. **Demonstration first:** show a complete positive path and a bounded failure
   path before asking learners to generalize.
3. **Failure-driven labs:** make the important failure repeatable and safe to
   inspect, with explicit terminal states and negative controls.
4. **Assessment and feedback:** use observable criteria, keep `not_evaluated`
   separate from `passed`, and route disagreements to human adjudication.
5. **Instructor calibration:** align facilitators on a fixed observer rubric
   before scaling delivery.
6. **Learner adoption evidence:** measure the target behavior, not only views,
   completions, attendance, or sentiment.

## Attention-adjusted evidence value

Human attention is part of the evidence budget. A useful working heuristic for
feedback collection is:

```text
attention-adjusted evidence value =
  P(usable feedback)
  × expected change in the decision
  ÷ attention cost
```

`P(usable feedback)` means the likelihood that the person will provide an
interpretable answer. Expected change means the decision-relevant information
that answer may add. Attention cost includes time, effort, context switching,
and accessibility burden.

This is a teaching heuristic, not a universal quality score. Implicit
feedback—edits, retries, rejections, or abandonment—can guide review sampling,
but it remains a proxy and must not silently become a pass/fail label.

## Boundaries

Keep public teaching examples synthetic or metadata-only. Do not copy private
learner records, unpublished exam content, raw production traces, credentials,
or provider-private prompts into a public lesson or portfolio artifact.

These patterns support course and enablement decisions within a declared
activity. They do not establish learner causality, broad adoption, production
readiness, or job performance.
