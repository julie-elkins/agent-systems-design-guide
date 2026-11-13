# Evaluation research checklist

Use this checklist before treating an agent benchmark, evaluator, or score as
evidence for a decision.

An **evaluation construct** is the capability or quality the evaluation claims
to measure. **Construct validity** asks whether the evaluation actually measures
that capability. A larger case count cannot repair an unclear construct,
misleading task setup, or invalid success rule.

This checklist is a practical synthesis of the papers listed below. It
implements selected principles; it does not reproduce any paper's benchmark,
leaderboard, or reported result.

For executable examples, see the
[Agent Assurance Harness](https://github.com/julie-elkins/agent-assurance-harness).
For the related frozen provider study, see
[LLM Eval Validity](https://github.com/julie-elkins/llm-eval-validity/tree/main/research/response-quality).
The repositories are linked companions: the design guide states the decision
rules, the harness demonstrates bounded controls, and the research repository
records study-specific evidence.

## Paper-to-checklist map

| Research source | Design question it adds |
| --- | --- |
| [Establishing Best Practices for Building Rigorous Agentic Benchmarks](https://arxiv.org/abs/2507.02825) and the Agentic Benchmark Checklist (ABC) | Could task setup or reward/scoring rules count an empty, incomplete, or unsafe result as success? |
| [Agent Evaluation Reliability: More Tasks Won't (Always) Fix an Agent Leaderboard](https://arxiv.org/abs/2610.00651) | Is uncertainty caused by tasks, scaffolds, environments, or the model claim itself? |
| [Judging LLM-as-a-Judge with MT-Bench and Chatbot Arena](https://arxiv.org/abs/2306.05685) | Have position, verbosity, self-preference, and judge-versus-human agreement been tested? |
| [G-Eval: NLG Evaluation using GPT-4 with Better Human Alignment](https://arxiv.org/abs/2303.16634) | Is the rubric explicit, and could the judge favor model-generated or stylistically verbose text? |
| [No Free Labels: Limitations of LLM-as-a-Judge Without Human Grounding](https://arxiv.org/abs/2503.05061) | Does the judge have human grounding, disagreement review, and an abstention boundary? |
| [Quantifying Variance in Evaluation Benchmarks](https://arxiv.org/abs/2406.10229) | Is the observed difference larger than variation from seeds, framing, sampling, or benchmark design? |
| [AgentBench: Evaluating LLMs as Agents](https://arxiv.org/abs/2308.03688) | Does the evaluation cover more than one interactive environment or one narrow task family? |
| [τ-bench](https://arxiv.org/abs/2406.12045) | Does the evaluation inspect user interaction, policy compliance, tool calls, final state, and repeated reliability? |
| [Measuring AI Ability to Complete Long Software Tasks](https://arxiv.org/abs/2503.14499) | Is task duration measured against a human baseline at more than one reliability threshold? |
| [A Live Benchmark and Automated Evaluation for Generative Research Synthesis](https://arxiv.org/abs/2508.20033) | Are synthesis quality, retrieval quality, and citation verifiability separated? |
| [Do Agent Benchmarks Measure Capability?](https://arxiv.org/abs/2607.22368) | Does the protocol require the claimed capability, or can a shortcut satisfy the score? |
| [Towards a Science of AI Agent Reliability](https://arxiv.org/abs/2602.16666) | Are consistency, robustness, predictability, and safety treated as distinct reliability properties? |
| [Stanford CS329A Part 9: Future Research Areas](https://www.youtube.com/watch?v=AyO6wyu4DEg&list=PLangBM27OtEA&index=9) | If the system generates its own tasks, critiques, or improvements, are diversity, verifier validity, curriculum difficulty, reward hacking, and resource tradeoffs measured separately? |
| [Harness Self-Evolution for Long-Horizon Terminal Agents](https://arxiv.org/abs/2610.27435) | Are harness changes and repeated-run behavior measured separately from the underlying model? |
| [Trial by Trace: Benchmarking AI Agent Harnesses for Reliability and Failure Attribution](https://arxiv.org/abs/2610.32285) | Does the trace identify whether a failure came from the task, model, harness, tool, environment, or evaluator? |
| [Beneath the Surface: What We Overlook When We Talk About Agent Evolution](https://arxiv.org/abs/2611.07624) | Can aggregate gains hide regressions, cost increases, or loss of previously demonstrated capability? |
| [The Horizon Gap: Planning, Memory, Execution, Training, and Evaluation for Long-Horizon LLM Agents](https://arxiv.org/abs/2608.06663) | Are long-horizon planning, memory, execution, training, and evaluation claims kept distinct? |

## Use this checklist to assess or build an evaluation environment

Use **assess mode** when reviewing an existing study, benchmark, evaluator, or
platform:

1. State the decision and evaluation layer.
2. Complete each applicable item using the available study or environment
   evidence.
3. Record missing evidence, uncertainty, ownership, and the next action.
4. Preserve the difference between observed, inferred, and unestablished claims.

Use **build mode** before creating or changing the evaluation environment:

1. Treat each applicable item as a design requirement.
2. Define the case set, reference standard, evaluator, revision record,
   uncertainty method, and maintenance owner before implementation.
3. Add a positive path and a negative control that can expose a plausible
   false pass.
4. Define how model, provider, dependency, rubric, and data changes trigger
   regression testing or reauthorization.
5. Mark an item not applicable only with a documented reason.

The smallest useful evaluation environment starts with a declared construct,
decision, failure cost, versioned cases, reference outcome, validity check,
uncertainty statement, and limitation boundary. Add platform features only when
they support a declared decision or close a measured evidence gap.

## 1. Claim and decision

- [ ] What decision will this evaluation support?
- [ ] What capability or quality is being measured?
- [ ] What is explicitly outside the claim?
- [ ] What failure would make the decision unsafe or materially wrong?
- [ ] Is the unit a response, step, tool call, trajectory, session, or complete task?
- [ ] Are correctness, safety, authorization, evidence completeness, latency, cost,
      and user outcome separate when they have different consequences?

## 2. Task setup and success rules

- [ ] Is the task specified independently of the implementation being evaluated?
- [ ] Are success, failure, refusal, timeout, and incomplete-result states explicit?
- [ ] Can an empty response, partial response, or failed side effect be counted
      as success?
- [ ] Does the reward or scoring rule measure the declared task outcome rather
      than an easy proxy?
- [ ] Are negative controls present for plausible false passes?
- [ ] Are task instructions, policies, tools, and expected outcomes versioned?
- [ ] Has a second person or independent check reviewed the task and scoring
      design before the benchmark is used?
- [ ] If tasks or curricula are generated by the system, are validity,
      difficulty, deduplication, diversity, held-out separation, and
      reward-hacking controls explicit?

## 3. Environment and interaction

- [ ] Does the environment contain the tools, permissions, data, state, and
      failure modes required by the claim?
- [ ] Are simulated users clearly separated from human-user evidence?
- [ ] Are tool and environment failures tested separately from model failures?
- [ ] Does the evaluation inspect the final state, not only the agent's answer?
- [ ] Are policy constraints and authorization boundaries part of the task?
- [ ] Are multi-turn dependencies, retries, recovery, and state changes covered?
- [ ] If several environments are used, are their difficulty and failure
      mechanisms described rather than silently pooled?

## 4. Case design and evidence

- [ ] Are normal, boundary, missing-information, conflicting, unauthorized, and
      historical-failure cases represented?
- [ ] Are calibration, development, regression, and held-out cases separate?
- [ ] Are duplicated or highly correlated cases identified so they are not
      counted as independent evidence?
- [ ] Is the reference outcome documented and independent of the evaluator being
      tested?
- [ ] Are high-risk slices reported separately from the pooled result?
- [ ] Is contamination or leakage from tuning into measurement addressed?
- [ ] Does every result say what evidence it observed and what it did not observe?

## 5. Operational currency and change management

Model behavior, provider APIs, pricing, and available models can change without
notice. An **evergreen maintenance model** is the ownership, review cadence,
compatibility record, and regression process that keeps guidance current without
silently changing the meaning of an older result.

- [ ] Are the model, provider, API, model revision, and run date recorded when
      they affect the claim?
- [ ] Are dependencies, runtime, prompts, tools, data, rubric, and evaluator
      revisions pinned or otherwise reconstructable?
- [ ] Are relevant provider changelogs and deprecation notices reviewed and
      recorded?
- [ ] Are compatibility checks and regression tests run after a model,
      provider, API, dependency, prompt, tool, rubric, or data change?
- [ ] Is there a documented way to interpret older studies after a relevant
      model or provider change?
- [ ] Is an owner and review cadence defined for current guidance and
      compatibility?
- [ ] Are pricing, latency, availability, and privacy changes kept separate
      from measurement-quality claims?

## 6. Independent evidence and external credibility

Independent evidence means a person, team, reproduction, or published case
study that did not simply reuse the system's own success claim. It is especially
useful when evaluating whether a platform is credible beyond its author or
original study. It is not required for every private exploratory experiment.

- [ ] If platform credibility or broad adoption is claimed, is there evidence
      from an independent user, reviewer, reproduction, or published case study?
- [ ] Does the independent evidence use a declared scope, version, and decision
      rather than an unbounded testimonial?
- [ ] Are differences between the independent setup and the original setup
      documented?
- [ ] Are unresolved disagreements or failed reproductions visible?
- [ ] If no independent evidence exists, is that limitation stated instead of
      implied away?

## 7. LLM-as-a-judge and human review

An **LLM-as-a-judge** is one language model grading another model's output. It
is a measurement component, not an oracle.

- [ ] Is the judge rubric observable, bounded, and tied to the decision?
- [ ] Has the judge been compared with human-reviewed cases?
- [ ] Are position/order, verbosity, self-preference, and style biases probed?
- [ ] Are pairwise and absolute scoring choices justified for this decision?
- [ ] Are judge disagreements, abstentions, and invalid outputs visible?
- [ ] Is reviewer calibration completed before the main review?
- [ ] Is adjudication defined for disagreements?
- [ ] Is judge repeatability measured across repeated runs or samples?
- [ ] Is human review retained for ambiguous, high-impact, or novel cases?

### 7.1 Question-level rubric contract and score composition

A **question contract** is a versioned set of explicit evaluator questions.
Each question states the construct it measures, the evidence it may use, how
its answer is represented, how much it contributes to the score, and whether a
failure blocks the decision. The contract makes a judge score inspectable; it
does not make the judge valid without calibration.

- [ ] Does every question have a stable ID, a named construct, a question type,
      an evidence reference, a weight, and a blocking rule?
- [ ] Is the question type appropriate to the decision: binary for a real
      boundary, choice for mutually exclusive behaviors, or ordered for
      explicitly defined partial correctness?
- [ ] Are answer levels and score boundaries observable rather than defined by
      vague quality language?
- [ ] Are weights and the aggregate threshold documented as policy, and are
      blocking questions kept separate from informative questions?
- [ ] Is the per-question answer, evidence, score, and status retained instead
      of reporting only the aggregate?
- [ ] Has the question set been audited for overlap so one failure is not
      charged twice without a documented reason?
- [ ] Has each important question been compared with independent human-reviewed
      calibration cases, with agreement and disagreements reported separately
      from the aggregate score?
- [ ] Are calibration, development, regression, and held-out cases kept
      separate?
- [ ] Is the rubric or question set versioned, and does a change trigger
      regression testing and a new evaluator-validity review?
- [ ] Is the evaluated model prevented from silently rewriting the rubric or
      question set that judges its output?

## 8. Reliability and variance

**Evaluation reliability** means the result is sufficiently stable under the
repeat conditions relevant to the intended claim. Stability for a fixed
model-and-scaffold system does not automatically establish stability for the
underlying model.

- [ ] Are repeated runs or repeated samples planned?
- [ ] Is the variation from model sampling, task framing, scaffold, environment,
      reviewer, and judge separated where possible?
- [ ] Is a pass^k-style measure useful for the decision? Pass^k means the
      probability that an agent succeeds across k independent trials.
- [ ] Are confidence intervals, variance summaries, or uncertainty bounds reported?
- [ ] Is the observed difference larger than the measured evaluation noise?
- [ ] Would adding more similar cases actually reduce the dominant uncertainty?
- [ ] Are ceiling and floor effects visible?
- [ ] Are results pooled only when the tasks and consequences justify pooling?
- [ ] For repeated coding-agent runs, are Pass@1, Pass@k, Pass^k, and flip
      rate defined and selected according to the decision?
- [ ] Are failed attempts checked for unintended repository changes before a
      retry is called recovered?
- [ ] Does an aggregate improvement preserve previously covered tasks, or are
      regressions visible, explained, and explicitly accepted by the decision
      owner?

### Long-horizon and protocol validity

A **long-horizon evaluation** measures whether an agent can complete tasks of
different durations at a declared reliability threshold. A **protocol-validity
check** asks whether the task setup and scoring rule actually require the
capability the benchmark claims to measure.

- [ ] Is task duration anchored to a documented human-professional baseline?
- [ ] Are at least two reliability thresholds reported, such as 50% and 80%,
      instead of one headline success rate?
- [ ] Are failures located within the trajectory, including planning, tool
      choice, reasoning, abandonment, looping, recovery, and final-state
      verification?
- [ ] Are context quality, task decomposition, tools, permissions, and
      environment failures controlled or reported separately from model
      behavior?
- [ ] Could an empty, partial, stale, or unsafe result satisfy the scoring rule?
- [ ] Are negative controls designed to expose plausible shortcut passes?
- [ ] If the benchmark uses simulated users or synthetic environments, is that
      evidence kept separate from human-user or production evidence?
- [ ] Does the report state which reliability surfaces remain unmeasured?

### Self-improvement and resource efficiency

**Meta-verification** means independently checking whether a verifier's
finding or score is itself supported by the evidence. A self-improvement loop
should not be treated as reliable merely because its score increases.

- [ ] Are generator, verifier, and meta-verifier responsibilities separated?
- [ ] Are verifier false positives, false negatives, fabricated findings, and
      unsupported scores tested with independent controls?
- [ ] If the system proposes tasks, are task validity, difficulty, diversity,
      duplication, and held-out separation measured?
- [ ] Could the generator or solver exploit the reward or scoring rule?
- [ ] Are quality, safety, latency, cost, energy, and privacy reported as
      separate dimensions?
- [ ] If local and cloud execution are compared, are routing rules, hardware,
      model revision, context, and privacy boundaries held constant or reported?
- [ ] If the system learns from prior runs, are memory retrieval, in-context
      learning, weight updates, stale state, and contradictory state evaluated
      as distinct mechanisms?

## 9. Reporting and release

- [ ] Are accepted, refused, failed, skipped, unavailable, and not-evaluated
      cases included in the denominator policy?
- [ ] Are results reported by relevant task, environment, tool, and risk slice?
- [ ] Are baseline and candidate conditions matched?
- [ ] Were thresholds and stopping rules written before interpreting the results?
- [ ] Is a human owner named for release, hold, rollback, or escalation?
- [ ] Does the report distinguish observed facts, scoped inferences, and claims
      that remain unestablished?
- [ ] Does the report state whether it supports a benchmark comparison, a
      workflow decision, a research observation, or only further investigation?

## What this checklist does not prove

Completing the checklist does not prove model quality, production safety,
generalization to unseen users, or agreement with every human judgment. It
establishes that the evaluation design made its claim, evidence, uncertainty,
and limitations inspectable before the result was used.

## Repository mapping

- [Agent Evaluation and Release](AGENT_EVALUATION_AND_RELEASE.md) explains the
  lifecycle and release reasoning.
- [Agent Assurance Harness checklist](https://github.com/julie-elkins/agent-assurance-harness/blob/main/AGENT_EVALUATION_CHECKLIST.md)
  provides operational evidence rows and statuses.
- [LLM Eval Validity](https://github.com/julie-elkins/llm-eval-validity)
  contains the research studies and metadata-only results.
- [MCP evaluation checklist](https://github.com/julie-elkins/mcp-server-design-guide/blob/main/MCP_EVALUATION_CHECKLIST.md)
  applies the interaction and tool-contract portions to MCP servers.
