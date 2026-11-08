# Enterprise AI Evaluation Team Charter — Reference Model

This charter describes a lean evaluation function that can support product,
engineering, security, and release decisions. It is a public operating-model
example for learners and reviewers. It is not evidence that a staffed team,
hosted annotation queue, or live production monitoring service exists.

An **evaluation function** is the people, workflow, evidence, and decision
ownership required to determine whether a system is ready for a declared use.
The function may be operated by one person, a small team, or a larger
organization; the responsibilities still need to be visible.

## 1. Mission and decision boundary

The function answers:

> Does the available evidence support release, hold, revision, rollback, or
> further human review for this declared system and use?

It does not answer:

- whether a model is universally safe or intelligent;
- whether a synthetic result predicts every production population;
- whether an automated judge is ground truth;
- whether an evaluation result authorizes a product, security, privacy, or
  incident decision that belongs to another owner.

The release decision is human-owned. An evaluator can recommend `release`,
`hold`, `revise`, `rollback`, `escalate`, or `not_evaluated`; it cannot
silently publish or deploy.

## 2. Lean team shape

One person may hold several roles, but the responsibilities must not disappear.
**Accountable** means the person who owns the final decision. **Responsible**
means the person who performs or coordinates the work.

| Role | Responsible for | Must produce |
| --- | --- | --- |
| Evaluation lead | Defines the decision, claim boundary, failure cost, evidence contract, and evaluation plan. | Intake disposition, evaluation plan, thresholds, uncertainty statement, and recommendation. |
| Rubric and label-operations lead | Maintains rubrics, reviewer sampling, calibration, disagreement routing, adjudication, and label provenance. | Versioned rubric, reviewer assignment, label record, unresolved queue, and adjudication record. |
| Judge-validity lead | Tests model-based evaluator bias, repeatability, false passes, false failures, abstention, and human agreement. | Calibration report, bias probes, disagreement analysis, and judge limitations. |
| Failure and red-team lead | Finds high-risk failures and converts confirmed weaknesses into regression cases. | Failure taxonomy, negative controls, threat cases, regression links, and residual-risk note. |
| Evaluation-platform lead | Maintains reproducible execution, trace contracts, revision binding, cost and latency ceilings, and evidence retention. | Run manifest, trace/evidence report, evaluator revision, resource report, and reproducibility check. |
| Release or product owner | Makes the human release, hold, revise, rollback, or escalation decision. | Recorded disposition, owner, follow-up date, and rollback or containment path. |

Security, privacy, legal, incident-response, and data owners remain accountable
for their own gates. The evaluation function supplies evidence; it does not
absorb those authorities.

## 3. Intake flow

Every request starts with a bounded intake record. Do not begin by choosing a
model judge or collecting a large dataset.

| Stage | Responsible role | Required output | Stop or escalate when |
| --- | --- | --- | --- |
| 1. Request | Requester and evaluation lead | Decision to support, system revision, affected users, failure cost, deadline, and requested disposition. | The decision or owner is unclear. |
| 2. Triage | Evaluation lead and release owner | Priority, risk tier, scope, data class, and required partner reviews. | The request involves protected data, external effects, or high-consequence decisions without an owner. |
| 3. Evidence contract | Evaluation lead and platform lead | Required observations, terminal states, trace fields, allowed sources/actions, and claim boundary. | The requested claim cannot be connected to observable evidence. |
| 4. Case and rubric design | Rubric lead and failure lead | Case manifest, expected outcomes, rubric, calibration set, negative controls, held-out set, and sampling plan. | Cases are unrepresentative, duplicated, leaked into tuning, or impossible to review independently. |
| 5. Execution | Platform lead | Revision-bound run manifest, statuses, resource usage, and evidence report. | Required evidence is missing, unavailable, unbound, or exceeds a declared ceiling. |
| 6. Human review | Rubric lead and assigned reviewers | Labels, reviewer state, disagreement categories, and unresolved findings. | A high-impact case lacks qualified review or disagreement cannot be resolved. |
| 7. Judge validation | Judge-validity lead | Agreement, bias, repeatability, abstention, and false-pass/false-fail analysis. | The judge is unstable, uncalibrated, or being treated as ground truth. |
| 8. Adjudication | Designated human adjudicator | Disposition for every known disagreement or missing finding. | The adjudicator cannot establish the reference decision or required evidence. |
| 9. Release decision | Release or product owner | `release`, `hold`, `revise`, `rollback`, `escalate`, or `not_evaluated`, with rationale and owner. | A blocking safety, evidence, privacy, security, or delivery condition remains unresolved. |
| 10. Follow-up | Evaluation lead and owning engineering team | Regression case, monitoring signal, due date, and next review trigger. | The failure has no owner or cannot be detected again. |

## 4. Minimum intake record

Use this as a public, sanitized template. Real systems may add fields for
privacy, regulatory, or organizational controls.

```text
evaluation_id:
requester:
decision_owner:
system_or_workflow:
revision_under_review:
declared_use:
affected_users_or_risk_slice:
failure_cost:
requested_decision:
data_class:
allowed_sources_and_actions:
required_partners:
deadline:
```

The intake is incomplete until the requester can state what decision the
evaluation will change and what failure must not pass unnoticed.

## 5. Review cadence

Cadence is a control against stale rubrics and forgotten findings. It is not a
promise that every organization needs the same calendar.

| Cadence | Purpose | Minimum evidence |
| --- | --- | --- |
| Per change or release candidate | Re-run revision-bound checks and compare against the declared baseline. | Run manifest, regression result, resource report, and release recommendation. |
| Weekly intake triage | Prioritize new requests, assign owners, and remove blocked work from a false-green queue. | Queue state, priority, owner, blocker, and next action. |
| Every two weeks | Calibrate reviewers and inspect disagreement, label drift, and adjudication backlog. | Calibration set, agreement summary, disagreement categories, and rubric changes. |
| Monthly health review | Review evaluator availability, false passes, false failures, drift, cost, latency, and regression escapes. | Health report, open risks, and corrective actions. |
| Quarterly construct review | Reconsider whether the evaluation still measures the intended capability and whether new failure families require coverage. | Construct review, coverage change, research questions, and retirement or expansion decision. |
| After an incident or material failure | Convert the failure into a reviewed case and verify that the control catches it. | Incident-to-regression link, owner, re-test, and residual-risk disposition. |

## 6. Escalation rules

Escalation means routing a decision to an accountable human owner with the
evidence and uncertainty preserved. It does not mean silently increasing a
score or retrying until the result turns green.

| Trigger | Immediate action | Escalate to |
| --- | --- | --- |
| Required trace, evidence, or revision binding is missing | Mark `not_evaluated`; stop interpretation. | Evaluation lead and platform lead. |
| Safety, privacy, authorization, or security boundary is violated | Hold the affected release or action; preserve the bounded finding. | Security/privacy owner and release owner. |
| Judge or reviewer disagreement is unresolved | Keep the case in review; do not collapse labels. | Adjudicator and evaluation lead. |
| Judge instability, bias, or false-pass rate exceeds the predeclared rule | Remove the judge from release authority; use human review or deterministic checks. | Judge-validity lead and release owner. |
| New failure appears after a change | Block or narrow the affected claim; create a regression case. | Owning engineering team and release owner. |
| Latency, cost, energy, or stop ceiling is exceeded | Report the resource breach separately from task outcome. | Platform lead and product owner. |
| Evaluator, telemetry, or review queue is unavailable | Mark the affected dimension `not_evaluated`; do not treat absence as success. | Evaluation lead and release owner. |
| Production feedback or drift changes the case population | Sample, review, and recalibrate before extending the claim. | Evaluation lead and product/data owner. |

## 7. Release ownership

The evaluation function makes a recommendation. The release or product owner
accepts, rejects, or defers that recommendation. Required partner gates remain
separate.

| Decision | Minimum condition | Owner |
| --- | --- | --- |
| Release | Required evidence exists, blocking thresholds pass, uncertainty is understood, and partner gates are complete. | Release or product owner. |
| Hold | A blocking failure, missing evidence, unresolved high-risk disagreement, or stale result remains. | Release or product owner. |
| Revise | The system, rubric, case set, evaluator, or evidence contract needs a bounded change and rerun. | Owning engineering or evaluation lead, with release owner aware. |
| Rollback | A released change causes a declared blocking regression or unsafe effect. | Release owner with engineering and incident-response owners. |
| Escalate | The evidence cannot support a safe local decision or ownership is disputed. | Accountable product, security, privacy, legal, or incident owner. |
| Not evaluated | The required check did not run, was unavailable, or was not valid for the claim. | Evaluation lead records the boundary; release owner decides whether to proceed. |

An automated report may make a recommendation, but it must not impersonate
approval.

## 8. Team health metrics

Track signals that change decisions, not vanity counts:

- intake age and blocked-request backlog;
- time from request to first bounded evidence;
- case coverage by risk, tool, source, and failure family;
- reviewer agreement and unresolved disagreement rate;
- adjudication age and label-provenance completeness;
- evaluator availability, abstention, false-pass, and false-fail rates;
- regression escapes and time from failure to durable case;
- drift and recalibration frequency;
- evaluation cost, latency, and queue load;
- proportion of results that are `not_evaluated` or `review_required`.

Do not optimize the team for the number of cases, dashboards, or model calls
alone. More volume can create weaker evidence if review quality and case
validity fall.

## 9. What the current portfolio demonstrates

| Area | Current state | Evidence |
| --- | --- | --- |
| Failure discovery and regression | Demonstrated with synthetic scenario packs and negative controls. | [Agent Assurance Harness](https://github.com/julie-elkins/agent-assurance-harness) |
| Trace and evidence contracts | Demonstrated with metadata-only traces, revision binding, and monitorability checks. | Harness docs and course Labs 6–8 |
| Rubrics, human review, and judge validity | Demonstrated in the response-quality study and course lessons. | [LLM Eval Validity](https://github.com/julie-elkins/llm-eval-validity), [Claude Evaluation Lab](https://github.com/julie-elkins/claude-evaluation-lab) |
| Design reasoning and release boundaries | Documented as reusable patterns and checklists. | [Agent Systems Design Guide](README.md) |
| Security and red teaming | Demonstrated with synthetic threat cases and incident-to-regression paths. | Harness security labs and readiness checklists |
| Team operating model | Newly documented here and in the [workstream crosswalk](EVALS_WORKSTREAM_CROSSWALK.md). | This charter |

## 10. Reference integration boundary

A domain-specific review system and a workflow-assurance harness should be
connected by a narrow contract, not by sharing source content.

The domain system remains authoritative for the subject-matter decision. For
example, an item-review system owns the correctness, blueprint alignment,
psychometric, and editorial judgments for the items it is authorized to
process. The assurance harness owns a different question: did the declared
workflow run completely, preserve its evidence, handle refusals, and produce a
reviewable result?

The bridge should:

1. Start with a revision-bound run identifier and declared workflow version.
2. Send metadata such as stage names, status values, counts, schema version,
   evidence-presence flags, bounded resource usage, and hashes.
3. Keep source bytes, protected content, answer keys, reviewer identities, and
   private traces inside the domain system.
4. Treat missing or refused evidence as `not_evaluated` or `review_required`,
   never as a passing result.
5. Return an assurance report to the private workflow without granting the
   public repository access to the underlying data.
6. Keep final subject-matter and release decisions with the authorized human
   owner.

An illustrative metadata-only handoff looks like this:

```text
run_id:
workflow_version:
source_revision:
stages_expected:
stages_completed:
stages_refused:
evidence_contract_status:
review_required:
resource_ceiling_status:
artifact_hashes:
assurance_disposition:
```

The public repositories are reference material and implementation examples;
they are not runtime dependencies. A production or internal workflow may
vendor, pin, or reimplement the public contract after reviewing its change
history. It should not fetch an unreviewed public revision during a
high-consequence review.

This boundary proves that process assurance can be separated from domain
judgment. It does not prove that the domain result is correct, that metadata
cannot identify a person, or that a public implementation is suitable for
every organization. Those require local privacy, security, and data-ownership
reviews.

## 11. Remaining gaps and future work

These are gaps in the operating model, not reasons to weaken the current paper’s
claims:

| Gap | State | Appropriate next step |
| --- | --- | --- |
| Standard intake and release records | This charter supplies public templates; no automated queue exists. | Use the templates for a synthetic learner exercise or future internal workflow. |
| Label-operations queue | Rubrics, reviewer packets, agreement, and adjudication exist; a hosted annotation queue does not. | Add a queue only when a real review population and privacy boundary are defined. |
| Team staffing and training | Roles are defined; no staffed organization is claimed. | Map actual people, skills, and partner gates in an internal document when applicable. |
| Production watch | Offline and synthetic monitorability is demonstrated; live deployment monitoring is not. | Add a real deployment only with security, privacy, delivery, and data-ownership reviews. |
| Agent Arena | Matched flat-versus-multi-agent study is planned, not completed. | Hold cases, tools, permissions, budget, and stopping rules constant before comparing. |
| User and business outcomes | The public portfolio does not claim production adoption or user impact. | Define approved outcome measures and a privacy-safe collection plan before studying them. |

## 12. Learner capstone

To practice the charter safely:

1. Fill out the minimum intake record for one synthetic workflow.
2. Name the decision, failure cost, owner, evidence contract, and escalation path.
3. Select cases, rubric, calibration examples, negative controls, held-out cases, and a sampling plan.
4. Run the offline harness and record statuses, resource bounds, and limitations.
5. Review or adjudicate the synthetic findings.
6. Produce a release, hold, revise, rollback, escalate, or not-evaluated record.
7. Convert one confirmed failure into a regression case and name the next review trigger.

This demonstrates that the learner can operate an evaluation process. It does
not certify them to approve a real production system.
