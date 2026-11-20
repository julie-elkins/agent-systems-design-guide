# Design decision: treat domain evidence as a governed source contract

## Problem

Generic model capabilities can be repackaged as domain products without
clarifying what evidence makes the workflow useful or trustworthy. That can
make a plausible summary, learner-status label, or recommendation look like a
validated domain decision.

## Decision

Require a domain evidence contract before using a domain source to support an
evaluation or release decision. The contract names the source owner,
authorization, purpose, provenance (where the evidence came from), revision or
time window, freshness, retention, access rules, missing-data behavior, and the
decision the evidence may change.

Preserve original observations separately from model-generated interpretations.
Treat the interpretation as derived evidence that requires validation. If
required evidence is missing, stale, conflicting, or structurally unavailable,
the workflow returns `not_evaluated` or `review_required` rather than claiming
success.

## Instead of

Treating access to a larger dataset, a new interface, or a model-generated
label as proof that a system is differentiated or correct.

## Why

The decision depends on evidence that another person can inspect and trust.
Separating source observations from derived interpretations prevents the model
from becoming the invisible source of truth. Explicit missing-data behavior also
keeps a failed data boundary from becoming a confident but unsupported result.

## Official guidance considered

No external technical standard is required for this public design pattern. The
pattern is aligned with the repository's existing evidence-contract,
metadata-only, refusal, and human-decision boundaries. Reviewed 2026-11-20.

## Implementation

- [Agent evaluation and release](../AGENT_EVALUATION_AND_RELEASE.md)
- [Claude Evaluation Lab lesson 2](https://github.com/julie-elkins/claude-evaluation-lab/blob/main/course/lesson-scripts/lesson-02-declare-the-decision.md)
- [Claude Evaluation Lab lesson 42](https://github.com/julie-elkins/claude-evaluation-lab/blob/main/course/lesson-scripts/lesson-42-data-network-and-supply-chain-controls.md)

## Verification

Use a synthetic case containing a public course description, an original
learner answer, and an AI-generated “understood” label. Verify that the workflow
keeps the records distinct, records source metadata, and returns
`not_evaluated` or `review_required` when the original observation is missing or
conflicts with the derived label.

## Costs if wrong

If this boundary is too weak, a model-generated interpretation may be treated as
ground truth and a generic capability may be presented as a durable domain
advantage. If it is too strict, some useful exploratory work may require human
review earlier than necessary. The selected boundary favors inspectable evidence
for decisions that affect release, teaching, or learner status.

## What this does not prove

The contract does not prove that a source is correct, that a learner model is
valid, that a domain system will outperform a general model, or that a public
synthetic exercise predicts production outcomes. Those claims require
decision-specific validation and authorized data-owner review.

## Decided by

Julie Elkins, informed by the referenced public post and comments; 2026-11-20.

## Teaching link

Learners should first write the decision, then classify each record as an
original observation or a derived interpretation, identify the source contract,
and state what the workflow does when required evidence is unavailable.
