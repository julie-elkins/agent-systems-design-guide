# Design decision: share evaluation meanings without sharing schemas

## Problem

The course, Apple platform lab, and design guide need to teach the same evidence
concepts, but they use different languages and implementation formats. If the
terms drift, a learner may interpret `UNAVAILABLE`, `hold`, or a missing score
as a successful evaluation.

## Decision

Publish one semantic report vocabulary and map each implementation to it. Keep
the course, Swift lab, and design guide schemas separate.

## Instead of

Forcing every repository to emit one shared JSON schema, or leaving each
repository to invent independent status meanings.

## Why

The repositories serve different purposes: the design guide explains reasoning,
the course teaches decisions with small Python labs, and the Apple repository
demonstrates a native platform boundary. Shared meanings support transfer while
separate schemas keep each implementation readable and appropriately scoped.

## Official guidance considered

- [Agent Evaluation and Release](../AGENT_EVALUATION_AND_RELEASE.md), this repository.
- [Agent Assurance Harness evaluation checklist](https://github.com/julie-elkins/agent-assurance-harness/blob/main/AGENT_EVALUATION_CHECKLIST.md).

## Implementation

- [Report vocabulary](../REPORT_VOCABULARY.md)
- [Claude Evaluation Lab](https://github.com/julie-elkins/claude-evaluation-lab)
- [Apple Platform Agent Quality Lab](https://github.com/julie-elkins/apple-platform-agent-quality-lab)

## Verification

Review the mapping, confirm release decisions remain separate from evidence
statuses, and run the course and Apple lab negative controls.

## Costs if wrong

The vocabulary may be too abstract for a learner or too restrictive for a
platform-specific report. The separate schemas preserve room for local detail;
the mapping must be updated when a repository adds a new status.

## What this does not prove

A shared vocabulary does not make the evaluations equivalent, independent, or
valid for production. It only makes the meanings and boundaries inspectable.

## Decided by

Julie, 2026-10-31.

## Teaching link

Ask learners to translate one course result and one Apple result into the
semantic vocabulary before comparing the release decisions.
