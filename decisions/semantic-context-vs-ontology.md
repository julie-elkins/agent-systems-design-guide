# Design decision: name semantic context without overstating ontology

## Problem

Agents that work with structured data need more than a list of fields. They need
the meaning of business terms, the relationships between entities, the definitions
of measures, and the limits of what the source can answer. Those pieces are often
described as an ontology even when the system does not provide formal logical
axioms or automated reasoning.

## Decision

Use these terms distinctly:

- **Ontology:** a model of concepts, entities, relationships, and their meaning.
  A formal ontology may also include logical axioms, constraints, and reasoning
  rules.
- **Semantic layer:** governed definitions for metrics, dimensions, joins, and
  query behavior. It describes how a business consistently measures and queries
  data.
- **Semantic context:** the bounded definitions, relationships, source limits,
  and usage guidance supplied to an agent so it can interpret a request.
- **Provenance:** the record of where a definition or result came from, when it
  was observed, and what review or certification supports it.

Evaluate whether an agent used the right semantic context separately from whether
it produced valid syntax, reached the source, or returned a plausible result.

## Instead of

Calling every metadata catalog, retrieval index, or generated graph an ontology;
or requiring every agent system to adopt a formal ontology implementation.

## Why

The cost of the wrong label is practical: a generated context layer can improve
term resolution and query grounding while still containing a wrong relationship,
an uncertified metric definition, or an incomplete source boundary. Calling that
result a formal ontology or treating it as proof of correctness hides the review
work still required.

The distinction also keeps architecture proportional. A system can expose
semantic context through an MCP server, API, or local contract without owning a
graph database or a logic engine.

## Official guidance considered

- [NVIDIA Auto Ontology](https://github.com/NVIDIA/auto-ontology), NVIDIA,
  accessed 2026-11-08. Used as a public example of a metadata-backed semantic
  context layer, not as a dependency or endorsement.
- [Agent Assurance Harness evaluation checklist](https://github.com/julie-elkins/agent-assurance-harness/blob/main/AGENT_EVALUATION_CHECKLIST.md),
  Julie Elkins, accessed 2026-11-08. Used for the separation of semantic outcome,
  evidence, provenance, and human review.

## Implementation

- [Semantic-dimension reporting in `nl2sql-eval`](https://github.com/julie-elkins/nl2sql-eval)
  records which semantic failure dimension a synthetic question exercises.
- [MCP evaluation checklist](https://github.com/julie-elkins/mcp-server-design-guide/blob/main/MCP_EVALUATION_CHECKLIST.md)
  asks whether an agent-facing result exposes the meaning and relationships that
  shaped it.

## Verification

Reviewers should be able to classify a claim or result as syntactic, semantic,
evidentiary, or provenance-related without treating those categories as
interchangeable. The `nl2sql-eval` report must preserve its existing populations
and scores while allowing failures to be inspected by semantic dimension.

## Costs if wrong

The vocabulary may be too abstract for a particular learner or domain. A semantic
dimension is also a diagnostic label, not a validated measure of production
semantic correctness. Future examples must keep those limits visible.

## What this does not prove

Metadata, retrieval, a graph, or a passing evaluation does not prove that a
business definition is correct, that a relationship is complete, that a generated
query is safe for every source, or that a system provides formal logical reasoning.
Human review and independent evidence remain necessary.

## Decided by

Julie, 2026-11-08.

## Teaching link

Ask learners to classify one system description as an ontology, semantic layer,
semantic context, or provenance claim, then identify what evidence would still be
needed before trusting an answer.
