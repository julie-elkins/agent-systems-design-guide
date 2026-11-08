# Design decision: document the evaluation team as an operating model

## Problem

The portfolio demonstrates evaluation controls, research validity, security
checks, and learner labs, but the ownership and workflow between those
artifacts are distributed. A reviewer could see many capabilities without
seeing how an evaluation function would accept work, review evidence, escalate
risks, and make a release decision.

## Decision

Document a lean public evaluation-team charter with six responsibilities:
evaluation lead; rubric and label operations; judge validity; failure and
red team; evaluation platform; and release or product ownership.

Make the charter the operating-model index for the existing repositories,
course, labs, and research rather than creating a new implementation
repository.

## Instead of

Creating a hosted annotation platform, live production monitoring service,
new benchmark, or separate team repository before a real data population,
privacy boundary, and accountable release owner exist.

## Why

The current artifacts already demonstrate most of the technical workstreams.
The missing signal is how those workstreams connect to people, decisions, and
review timing. A public charter makes that reasoning inspectable while keeping
synthetic evidence separate from production claims.

The charter also documents a metadata-only integration boundary: a private
domain-review system may use the assurance contract to verify workflow
completeness and evidence handling, while source content and domain judgments
remain inside the authorized system.

## Official guidance considered

- [Agent Evaluation and Release](../AGENT_EVALUATION_AND_RELEASE.md), Julie Elkins,
  accessed 2026-11-08.
- [Agent Evaluation Checklist](https://github.com/julie-elkins/agent-assurance-harness/blob/main/AGENT_EVALUATION_CHECKLIST.md),
  Julie Elkins, accessed 2026-11-08.
- [Evaluation workstream crosswalk](../EVALS_WORKSTREAM_CROSSWALK.md), Julie Elkins,
  accessed 2026-11-08.

## Implementation

- [Evaluation team charter](../EVALS_TEAM_CHARTER.md)
- [Evaluation workstream crosswalk](../EVALS_WORKSTREAM_CROSSWALK.md)
- [Agent Assurance Harness](https://github.com/julie-elkins/agent-assurance-harness)
- [LLM Eval Validity](https://github.com/julie-elkins/llm-eval-validity)
- [Claude Evaluation Lab](https://github.com/julie-elkins/claude-evaluation-lab)

## Verification

The charter must:

- name an owner and decision for every intake;
- preserve `not_evaluated`, `review_required`, and escalation states;
- separate evaluation recommendation from human release approval;
- map each workstream to an existing artifact or mark it as future work;
- include a learner capstone that can run offline and synthetically; and
- define a metadata-only bridge that does not export protected source content;
- keep production monitoring, user outcomes, and agent-architecture comparison
  outside the current evidence claims.

## Costs if wrong

The charter could make the portfolio look like a deployed organization rather
than a public operating-model example, or it could create maintenance work
without improving learner understanding. The document therefore labels current
evidence, partial capability, and future work separately.

## What this does not prove

The charter does not prove that a staffed evaluation team exists, that the
portfolio has live production telemetry, that its synthetic cases represent
all users, or that its research results establish universal model or agent
quality.

## Decided by

Julie, 2026-11-08.

## Teaching link

Read the charter after the workstream crosswalk. Then complete its learner
capstone using a synthetic workflow from the Agent Assurance Harness. The
exercise is to make ownership, evidence, escalation, and release boundaries
visible—not to simulate authority over a real production deployment.
