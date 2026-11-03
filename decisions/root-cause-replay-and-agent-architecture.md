# Design decision: diagnose failures before changing prompts

## Problem

A plausible agent failure can originate in the model, prompt, skill, environment,
tool, code, orchestration, evaluator, or data. A cheap rerun from the failure point
can also look like full validation even though it holds earlier history fixed. Teams
need a reusable design rule for diagnosing failures and comparing flat and multi-agent
architectures.

## Decision

Require an explicit root-cause hypothesis before changing the system. Use failure-point
replay as a diagnostic filter, then run promising changes from the beginning on matched
cases before using the result for a release decision. Compare flat and multi-agent
architectures at the complete user-task boundary, including handoffs, context, cost,
latency, recovery, authorization, and maintenance after model upgrades.

## Instead of

Treating every failure as a prompt problem, treating a replay as full validation, or
keeping a subagent because its isolated output looks strong.

## Why

Prompt edits can mask environment and code defects. Replay is useful because it is
cheap and isolates the observed decision, but it cannot reveal a new earlier
divergence. Multi-agent designs add handoffs and context boundaries; their value must
be demonstrated by end-to-end evidence rather than isolated subagent quality.

## Official guidance considered

- [Claude evaluation guidance](https://docs.anthropic.com/en/docs/build-with-claude/define-success), Anthropic, accessed 2026-11-03.
- [Claude Code skills](https://docs.anthropic.com/en/docs/claude-code/slash-commands), Anthropic, accessed 2026-11-03.
- [Codex skills](https://developers.openai.com/codex/skills/create-skill), OpenAI, accessed 2026-11-03.
- [Codex evals](https://developers.openai.com/codex/use-cases/ai-app-evals), OpenAI, accessed 2026-11-03.
- [LangSmith evaluation](https://docs.langchain.com/langsmith/evaluation), LangChain, accessed 2026-11-03.

## Implementation

- [Agent evaluation and release guide](../AGENT_EVALUATION_AND_RELEASE.md)
- [Claude Evaluation Lab](https://github.com/julie-elkins/claude-evaluation-lab)
- [Agent Assurance Harness](https://github.com/julie-elkins/agent-assurance-harness)

## Verification

Confirm that the guide distinguishes replay from full validation, requires a
root-cause hypothesis, and evaluates architecture alternatives with matched cases
and end-to-end evidence. Confirm the course includes the same concepts in Lessons 25,
28, 37, and 39 and preserves a synthetic, offline learner path.

## Costs if wrong

The additional comparison and full-run requirement consume more time and evaluator
budget. That cost is accepted because a prompt-only fix or isolated subagent result
can approve a flawed system while the original failure remains or moves earlier in
the trajectory.

## What this does not prove

The design rules do not prove that a particular prompt, model, skill, or architecture
is better. They define the evidence needed to make that claim for a declared workflow,
case set, revision, and decision.

## Decided by

Julie, 2026-11-03.

## Teaching link

Ask learners to classify one synthetic failure, write a competing root-cause
hypothesis, run a diagnostic replay, and then state what a full validation run must
still check. If architecture changes, compare a flat and multi-agent path on the same
task cases.
