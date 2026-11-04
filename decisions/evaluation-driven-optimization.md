# Design decision: constrain evaluation-driven optimization

## Problem

An evaluation can become part of an agent-development loop that changes prompts,
skills, model settings, tool descriptions, policies, or harness code. That loop
can improve the cases used during tuning while overfitting the grader, hiding
high-risk regressions, or confusing run-to-run noise with a real improvement.

## Decided

Teach evaluation-driven optimization as a controlled comparison with separate
tuning and held-out cases, a grader-health preflight, one meaningful change at a
time, separate quality and operational dimensions, and an explicit
accept/reject/revert action. Require fresh cases after release or before a
candidate is reconsidered.

## Instead of

Optimize against one undifferentiated dataset, accept the highest score as the
release decision, or add a provider-specific optimizer as a new source of
authority.

## Why

The decision is whether a changed workflow is safer and more useful for its
declared task, not whether it can maximize a number. Tuning and held-out
separation makes overfitting more visible. Grader consistency, observed
variation, and remaining headroom tell the reviewer whether the measurement can
detect change. An explicit revert action limits the cost of a candidate that
does not generalize.

## Implementation

- [Agent evaluation and release guide](../AGENT_EVALUATION_AND_RELEASE.md)
- [Public course Lesson 28](https://github.com/julie-elkins/claude-evaluation-lab/blob/main/course/lesson-scripts/lesson-28-baselines-variants-uncertainty-and-slices.md)
- [Public course Lab 5](https://github.com/julie-elkins/claude-evaluation-lab/blob/main/labs/05-release-capstone/README.md)
- [Agent evaluation checklist](https://github.com/julie-elkins/agent-assurance-harness/blob/main/AGENT_EVALUATION_CHECKLIST.md)

## Verification

Confirm that a candidate report identifies the changed factor, tuning cases,
held-out cases, grader-health state, observed variation, headroom, blocking
regressions, and the accept/reject/revert action. Run the public synthetic
capstone and confirm that a candidate that improves tuning cases but regresses
held-out high-risk cases is held and marked for reversion or investigation.

## Costs if wrong

The controls add dataset-management and review work, and a useful candidate may
be held until fresh evidence is available. That cost is preferable to shipping
a change that wins a local score while making a high-risk workflow worse. The
pattern remains provider-neutral and permits `not_evaluated` when grader health
or fresh evidence has not been established.

## What this does not prove

The pattern does not prove global optimality, production safety, business
impact, or generalization to all future traffic. It does not authorize an
automated optimizer to release a change without human ownership.

## Decided by

Julie Elkins, 2026-11-04.

## Teaching link

Use the course capstone to inspect a candidate that improves the tuning cases,
regresses a high-risk held-out case, and must be reverted or investigated before
release.
