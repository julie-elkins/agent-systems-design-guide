# Design decision: constrain multi-objective candidate selection

## Problem

An agent development loop can produce several candidates with different
quality, cost, latency, safety, and evidence results. A single weighted score
can hide a safety failure or make a cheap but weakly evidenced candidate look
competitive.

## Decision

Apply hard constraints for safety, evidence completeness, held-out
performance, and repeated-run stability before calculating a Pareto frontier.
A Pareto frontier is the set of eligible candidates where no alternative is at
least as good on every declared objective and better on one. Keep quality,
total cost including retries, and latency as separate objectives, and require
human review to choose among frontier candidates.

## Instead of

Choose the cheapest candidate, maximize one composite score, or let an
automated optimizer select and release a candidate without a human owner.

## Why

Safety and evidence completeness are release boundaries, not preferences that
can be traded away for lower cost. Once unsafe or weakly evidenced candidates
are removed, the frontier preserves the real trade-off for the person who owns
the decision. This also makes repeated-run instability visible instead of
mistaking one lucky run for a stable improvement.

## Implementation

- [Agent Assurance Harness Pareto comparison](https://github.com/julie-elkins/agent-assurance-harness/blob/main/tools/pareto_candidate_report.py)
- [Synthetic candidate fixture](https://github.com/julie-elkins/agent-assurance-harness/blob/main/examples/pareto-candidate-comparison/candidates.json)
- [Release capstone](https://github.com/julie-elkins/claude-evaluation-lab/blob/main/labs/05-release-capstone/README.md)
- [Evaluation-driven optimization decision](evaluation-driven-optimization.md)

## Verification

Run:

```text
uv run python tools/pareto_candidate_report.py
```

The synthetic negative controls must exclude the unsafe and unstable
candidates, mark the dominated candidate, and leave two safe candidates on the
frontier. The report must state that human review remains required.

## Costs if wrong

The constraints can hold a useful candidate for review or reject a candidate
whose instability is an artifact of too few repeated runs. Weakening them too
far has the more serious cost: a cheap but unsafe candidate can appear to be a
valid optimization target.

## What this does not prove

The comparison does not prove live model quality, production safety, global
optimality, or generalization beyond the declared synthetic cases. It does not
choose or release a candidate.

## Decided by

Julie Elkins, 2026-11-04.

## Teaching link

Start with the capstone's controlled baseline-versus-variant comparison, then
run the companion frontier report and explain why hard constraints are applied
before cost optimization.
