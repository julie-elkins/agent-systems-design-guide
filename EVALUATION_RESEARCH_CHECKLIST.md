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

## 5. LLM-as-a-judge and human review

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

## 6. Reliability and variance

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

## 7. Reporting and release

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
