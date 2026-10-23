# Research Design Guide for AI Systems

Research is not a longer README and a result is not automatically evidence. A
defensible AI research project has a question, a bounded method, an artifact another
person can inspect, and a claim that does not exceed what the study measured.

This guide describes the smallest complete path from an observed engineering problem
to a reproducible working paper. It is written for builders, evaluators, technical
educators, reviewers, hiring managers, and recruiters who need to distinguish a
research signal from a polished demo.

## 1. Start with the decision, not the topic

Begin with the decision the evidence is meant to support:

- Who will use the result?
- What decision will it change?
- What happens if the decision is wrong?
- What evidence would be sufficient for that decision?
- What must the study never claim?

Weak question:

> Are AI agents reliable?

Bounded question:

> After a tool, prompt, data, policy, evaluator, or deployment change, can
> revision-aware evidence reduce unsupported approval decisions?

The bounded question identifies the unit of analysis, the comparison, the consequence
of error, and the evidence needed to answer it.

## 2. Describe the gap without overstating novelty

Before building, search adjacent work and write down:

1. what the field already measures;
2. what those studies leave outside their scope;
3. what your study adds; and
4. what your study deliberately does not attempt.

Do not claim that no one has studied a broad area such as agent reliability,
provenance, continuous evaluation, security, or human approval. Those are active
areas. A defensible contribution is usually narrower:

> This study tests the connection between X and Y under Z conditions.

For example, a study may connect revision-aware evidence to organizational
reauthorization decisions after system change. The contribution is the tested
connection, not ownership of every component in it.

## 3. Define the object and the claim

Write the object being studied before choosing a metric:

- a workflow revision;
- a tool-selection decision;
- an evaluation instrument;
- a reviewer decision;
- a release gate; or
- a bounded agent outcome.

Then define the claim in the same scope.

An evaluation score is not a property of an agent in the abstract. It is evidence
produced by a particular scenario set, evaluator, workflow, revision, and decision
rule. If any of those change, the claim may need to be rerun or narrowed.

### Keep the claim states visible

- **Observed:** what the trace, test, or evaluation directly recorded.
- **Inferred:** what those observations support within the declared scope.
- **Unestablished:** what was not tested or cannot be concluded from the available
  evidence.

An *epistemic boundary* is the line between what the evidence supports and what it
does not. Put that line in the paper, the report, and the README.

## 4. Write the protocol before the result

The protocol should state:

- the primary research question;
- hypotheses;
- baseline;
- intervention;
- unit of analysis;
- case or participant selection;
- primary and secondary outcomes;
- exclusions;
- stopping rule;
- analysis method; and
- what the study will not claim.

A *baseline* is the comparison condition without the proposed method. An
*intervention* is the method being tested. A *stopping rule* says when the study ends
so cases are not added only because they improve the result.

The protocol must be versioned before the result-producing implementation. A public
protocol lock is not the same as formal preregistration, but it makes the sequence
auditable:

```text
protocol revision -> implementation revision -> run revision -> result
```

Record the exact commits, dates, runtime, case-set hash, and command used.

## 5. Choose the smallest complete comparison

Use the smallest matrix that can answer the question:

| condition | purpose |
|---|---|
| baseline | shows what ordinary reporting would decide |
| mechanism or intermediate condition | isolates the value of one control |
| full intervention | tests the complete proposed process |

The intermediate condition matters. Without it, an improvement cannot be attributed
to the mechanism you think caused it.

For a changing agent workflow, a useful comparison is:

1. pass/fail reporting;
2. revision-aware evidence; and
3. revision-aware evidence plus explicit reauthorization.

*Reauthorization* means an explicit decision that current evidence still supports
continued use after a relevant change.

## 6. Build a public artifact with a safe boundary

Start offline and synthetic when the research question does not require live data.
This gives the reader a reproducible first run and prevents provider access, privacy,
and cost from becoming hidden variables.

The artifact should expose:

- identifiers;
- revisions;
- statuses;
- decisions;
- counts;
- hashes;
- bounded cost counters; and
- refusal or escalation codes.

It should not expose:

- credentials;
- private traces;
- raw prompts;
- tool payloads;
- customer data;
- unpublished exam content;
- copied third-party content; or
- internal incident details.

Metadata-only output proves that a declared control ran and what bounded state it
reported. It does not prove the correctness of hidden state or the safety of a live
system.

## 7. Add a negative control

A *negative control* is a deliberately plausible case that should fail or escalate.
For each positive path, add a case that removes, stales, misbinds, or fabricates a
required condition.

Examples:

- a green test result with missing security evidence;
- a current result from an unapproved source;
- a stale evaluation attached to a new workflow revision;
- a successful transport response with empty task context; or
- a release with no accountable owner or rollback evidence.

The test is meaningful only if the negative control is caught. A green run without a
caught negative control proves that the code executed, not that the control works.

## 8. Separate the decision oracle from the system under study

When a pilot needs a reference decision, define the *decision oracle* before
generating condition outputs. The oracle is a bounded reference for the study, not a
universal authority.

Record:

- which fields the oracle may inspect;
- how each expected decision is derived;
- which cases are `reauthorized`, `review_required`, or `blocked`; and
- what the oracle cannot establish.

Do not let the system being evaluated define its own success criteria after seeing
the result.

## 9. Choose statistics that match the design

The analysis must follow the data-generating process:

- deterministic synthetic matrix: report exact counts and control outcomes;
- sampled cases: report sampling method and uncertainty;
- repeated stochastic runs: report seeds, repetitions, and run-to-run spread;
- human ratings: report rater selection, rubric, agreement, and adjudication;
- deployment data: report missingness, drift, and the population represented.

Do not attach a p-value or confidence interval merely because a paper usually has
one. A *confidence interval* quantifies uncertainty around an estimate under a
specified sampling model. It does not turn a fixed synthetic case matrix into
population evidence.

Report costs with benefits. A control that catches more failures but requires more
evaluation or review work has produced a tradeoff, not an unconditional win.

## 10. Report the result in layers

Use this order:

1. **Observed result:** what happened in the run.
2. **Scoped inference:** what that result supports within the tested conditions.
3. **Limitations:** what remains uncertain or outside the design.
4. **Next test:** what evidence would reduce that uncertainty.

Example:

> The full protocol produced zero false-ready decisions on the fixed synthetic
> matrix.

That may support:

> The implementation caught the pre-specified negative controls in this pilot.

It does not support:

> The protocol makes production AI agents safe.

## 11. Make the artifact reproducible

A reader should be able to find:

- the canonical source repository;
- the exact revision used;
- the case-set or fixture hash;
- the environment;
- the one-command run;
- expected output;
- tests;
- limitations;
- license; and
- a clear statement of what is not included.

Use separate repositories when ownership is genuinely different:

| repository | owns |
|---|---|
| design guide | reusable decisions and method |
| research repository | question, protocol, fixture, analysis, paper |
| implementation repository | executable synthetic controls |
| learning path | reader sequence and exercises |

Link these repositories; do not copy canonical content into every one. Duplication
creates maintenance drift and makes it harder to identify which result is
authoritative.

## 12. Treat human review as a new study phase

Adding independent reviewers is not a small implementation detail. It changes the
study population and requires a new protocol covering:

- who the reviewers are;
- what information they receive;
- the rubric and training;
- consent and privacy requirements;
- whether institutional or organizational review is required;
- sampling and exclusion;
- inter-rater agreement; and
- adjudication.

*Inter-rater agreement* measures how consistently independent reviewers apply the
same rubric. It does not prove the rubric is correct.

Do not silently add human data to an initially synthetic study. Record the transition
as a new phase with its own limitations.

## 13. Write the paper as an evidence path

A practical working-paper structure is:

1. Abstract.
2. Introduction and decision consequence.
3. Research question and hypotheses.
4. Related work and contribution boundary.
5. Method and protocol.
6. Artifact and reproducibility.
7. Results.
8. Limitations and broader impacts.
9. Future work.
10. Data, code, and availability statement.

The abstract should say what was measured, not what the project hopes to become.
Results should not appear before the method. Limitations belong near the claim they
limit, not only in a final paragraph.

## 14. Make the work legible to reviewers and hiring teams

The strongest signal is not the word "research." It is a visible chain:

```text
question -> protocol -> implementation -> negative control -> result -> limitation -> next test
```

A hiring manager can then see:

- how you choose a problem;
- how you prevent yourself from moving the goalposts;
- how you make the work reproducible;
- how you handle failure and uncertainty; and
- how you turn a result into the next engineering decision.

Recruiter-facing material should link to the evidence path, not summarize the paper
as a claim of production impact that the artifact cannot establish.

## Working example

The current working example is
[From Evaluation to Reauthorization](https://github.com/julie-elkins/llm-eval-validity/tree/main/research/agent-reauthorization).

- The research repository owns the question, protocol, fixture, analysis, and paper.
- The [Agent Assurance Harness](https://github.com/julie-elkins/agent-assurance-harness)
  owns the executable synthetic pilot.
- The result records the source commit and case-set hash.
- The reporting checklist separates observed findings from unestablished claims.

The example is intentionally limited. It demonstrates a research path and a
controlled pilot; it does not certify production safety or general organizational
effectiveness.

## Decision record

For every non-obvious research-design choice, record:

- **Decided**
- **Instead of**
- **Why**
- **Costs if wrong**
- **Decided by**

The alternative matters. Without it, the record makes the chosen path look inevitable
and prevents a future reviewer from understanding what was traded away.

MIT License.
