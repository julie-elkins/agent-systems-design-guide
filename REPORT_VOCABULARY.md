# Evaluation report vocabulary

This vocabulary is shared by the learner course, implementation repositories,
and portfolio demonstrations. It defines meanings, not one required JSON shape.
Each implementation may keep its own report schema if it preserves these
distinctions.

## Evidence statuses

| Status | Meaning |
| --- | --- |
| `passed` | Required evidence was observed and the declared rule was met. |
| `failed` | Required evidence was observed and the declared rule was not met. |
| `blocked` | A required authorization or control prevented a safe decision. |
| `not_evaluated` | Required evidence was missing, invalid, or not run. |
| `not_applicable` | The check does not apply to the declared workflow. |
| `rerun_required` | The result is too variable, incomplete, or otherwise invalid to decide. |
| `unavailable` | A provider, framework, or integration could not run in the current environment. |

`unavailable` describes an execution boundary. For release purposes, required
unavailable evidence normally maps to `not_evaluated`; it must not silently
become `passed`.

## Release decisions

Release decisions are separate from evidence statuses:

- `release` — the declared release contract was met.
- `hold` — do not release while a blocking issue remains.
- `revise` — change the system or evaluation and rerun.
- `rollback` — restore a known-good revision.
- `escalate` — route an unresolved or high-risk decision to an accountable human.

For example, a case can be `failed`, the evaluator can be `passed`, and the
overall release decision can be `hold`. A framework can be `unavailable`, which
makes the relevant evidence `not_evaluated` and may also make the release
decision `hold`.

## Required dimensions

When a comparison is used to support a release decision, report the dimensions
separately when they apply:

- task correctness;
- safety and refusal behavior;
- tool-call correctness;
- latency and timeout behavior;
- cost and usage limits;
- user or business outcomes; and
- evaluator validity and human-review state.

Use `not_evaluated` or `not_applicable` when a dimension was not measured. Do
not invent a score to make the report look complete.

## Cross-repository mapping

| Concept | Course and Python labs | Apple platform lab |
| --- | --- | --- |
| Evidence passed | `passed` | `PASS` / `passed` |
| Evidence failed | `failed` | `FAIL` / `failed` |
| Release held | `hold` | release decision `hold` |
| Repeatability problem | `rerun_required` | `RERUN REQUIRED` |
| Required integration unavailable | `not_evaluated` | `UNAVAILABLE` plus `not_evaluated` mapping |
| Missing or invalid evidence | `not_evaluated` | explicit evidence status |
| Authorization prevented safe action | `blocked` | explicit blocked scenario |

The mapping is intentionally semantic rather than structural. A Python report
may use dictionaries and a Swift report may use Codable structs, but a learner
should not have to relearn what “passed” or “not evaluated” means.

## Claim boundary

These statuses describe the declared evidence and decision rule. They do not
prove live model quality, production safety, generalization, or business
impact outside the tested workflow, revisions, cases, and environment.
