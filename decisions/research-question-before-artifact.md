# Research question before artifact

## Problem

AI research projects can accumulate code, models, metrics, and polished
documentation before the reader can identify the question the study is
designed to answer. That makes an engineering decision look like a research
contribution and makes it difficult to tell whether the method supports the
claim.

## Decision

Make the research question the first recorded artifact. Require the protocol,
implementation, analysis, paper, and public explanation to map back to that
question.

## Instead of

Starting with an implementation or a broad topic, then selecting the research
question and contribution language after results are available.

## Why

A question-first record forces the study to name one primary outcome, a
baseline, an intervention, matched controls, and a claim boundary before
execution. It also gives reviewers a stable way to distinguish a measured
finding from an engineering choice or an unsupported assertion.

## Official guidance considered

No external standard is treated as a substitute for the study protocol. This
guide incorporates external reviewer feedback as a private design input and
keeps the public artifact generic.

## Implementation

- [Research Writing Guide](../RESEARCH_WRITING_GUIDE.md)
- [Research Design Guide](../RESEARCH_DESIGN_GUIDE.md)
- [Evaluation Research Checklist](../EVALUATION_RESEARCH_CHECKLIST.md)
- [Response-quality research question record](https://github.com/julie-elkins/llm-eval-validity/blob/main/research/response-quality/RESEARCH_QUESTION.md)

## Verification

The guide is checked by following its sequence against the response-quality
study: question, protocol, matched conditions, outcomes, results, limitations,
and next test.

## Costs if wrong

The question-first sequence adds a small documentation step before
implementation. If the question is poorly chosen, the early record makes that
mistake visible sooner; without it, the project can accumulate a large
artifact that answers a different question than the paper claims.

## What this does not prove

Writing a clear question does not prove that the study is novel, statistically
powerful, ethically approved, or externally valid. It makes those later
judgments easier to inspect.

## Decided by

Julie Elkins, November 28, 2026.

## Teaching link

Start with the [Research Writing Guide](../RESEARCH_WRITING_GUIDE.md), then
follow the linked study question record before reading the implementation or
results.
