# Research Writing Guide for AI Systems

Research writing makes the evidence path visible. It tells a reader what
question was asked, why the method could answer it, what was observed, and
where the evidence stops. A polished engineering description is not
automatically a research contribution.

This guide is for an engineer, evaluator, educator, or independent researcher
turning an AI-system investigation into a readable working paper.

## 1. Ask the research question before building

Write the question before choosing the paper title, implementation, chart, or
result.

A useful research question names:

- the question word;
- the experiment or comparison;
- the clause that defines the outcome and scope.

The pattern is:

```text
question word + experiment + clause
```

Examples:

> How does adding provider-enforced structured output change
> evidence-contract satisfaction on a fixed matrix of evidence-grounded tasks?

> Can a revision-aware evidence protocol reduce unsupported reauthorization
> decisions after a workflow change?

> When does a multi-agent decomposition improve task completion after tools,
> budget, and stopping rules are held constant?

Avoid a kitchen-sink question that asks about correctness, safety, latency,
cost, user experience, generalization, and organizational impact at once.
Choose one primary outcome. Make the other measurements secondary or future
work.

Before implementation, record:

1. the primary research question;
2. the decision the answer may inform;
3. the baseline;
4. the intervention;
5. the unit of analysis;
6. the primary outcome;
7. the stopping rule; and
8. the claims the study will not make.

The question record is the anchor. The implementation, protocol, analysis, and
paper should be traceable back to it.

## 2. Define the gap narrowly

Do not begin with “no one has studied this.” AI evaluation is an active field,
and broad novelty claims are rarely defensible.

Instead, write four short statements:

1. **Known:** what prior work measures or establishes.
2. **Unresolved:** what remains outside that work.
3. **Test:** the narrower relationship this study examines.
4. **Boundary:** what this study deliberately does not attempt.

Example:

> Prior work evaluates interactive agents and studies evaluator bias. This
> study examines the narrower question of whether a provider-facing output
> constraint changes evidence-contract satisfaction when the task matrix,
> tools, validator, and budget are held constant. It does not estimate
> production reliability or rank providers.

The contribution is the tested connection, not ownership of every component.

## 3. Use only three kinds of substantive sentences

In a research paper, every substantive sentence should be one of these:

### A finding from the study

> Structured output reduced protocol acceptance in both tested Claude
> configurations.

This must be traceable to the method and results.

### A cited claim

> Prior work shows that benchmark construction choices can change measured
> agent performance (Zhu et al., 2025).

This needs a reference that actually supports the sentence.

### A research question

> Does a provider-facing schema improve evidence-contract satisfaction when
> local validation is held constant?

This identifies what the study is designed to learn.

An assertive sentence that is neither a result nor a cited claim is usually
unsupported framing. Rewrite it as a question, support it with a citation, or
remove it.

This rule does not mean every transition needs a citation. It means the
paper must not smuggle assumptions into confident-sounding prose.

## 4. Define terms before using them

Write the definition at the first point where the term affects the reader's
interpretation.

For example:

- **Provider** means the external service that runs the model.
- **Application** means the local program that sends requests, executes the
  bounded workflow, and records results.
- **Harness** means the executable test boundary that runs the declared cases
  and checks the declared controls.
- **Structured output** means a provider-facing schema constraint on the
  response format.
- **Evidence contract** means the minimum observable fields required before a
  result counts as usable evidence.

Do not assume that a familiar term in engineering has the same meaning in a
research paper. If two components answer different questions, name them
separately.

## 5. Map the method to the question

The method should explain why each design choice is necessary, not only what
the code does.

Use this map:

| Question element | Method decision | Why it is needed |
|---|---|---|
| Intervention | Provider-facing schema | Isolates the output constraint being tested |
| Baseline | Prompt instructions plus local validation | Shows the comparison without the intervention |
| Primary outcome | Evidence-contract satisfaction | Measures usable evidence rather than formatting alone |
| Secondary outcome | Protocol acceptance and refusal | Shows whether the intervention changes the failure surface |
| Controls | Same cases, tools, budget, stopping rules, and validator | Prevents resource differences from explaining the result |
| Negative control | A plausible green case that must fail or escalate | Tests whether the evaluator can catch the failure it claims to detect |

If a method choice is not connected to the question, either explain its role
or remove it.

## 6. Write contributions as nouns

Contributions are what the reader can take away from the study. They are not
the sequence of actions the author performed.

Weak contribution:

> We separate protocol acceptance from evidence satisfaction.

That describes an action.

Stronger contribution:

> A separated outcome definition that distinguishes protocol acceptance from
> evidence-contract satisfaction.

Useful contribution forms include:

- a bounded comparison;
- a dataset or synthetic case matrix;
- an evaluation protocol;
- a measurement definition;
- a replication result;
- a calibration result;
- a reproducibility artifact;
- a taxonomy of failure modes; or
- a documented boundary on what a metric does not establish.

Each contribution should answer:

1. What is the noun?
2. What does it show?
3. What evidence supports it?
4. What does it not prove?

## 7. Use a compact abstract structure

A general-purpose abstract usually needs six or seven sentences:

1. **Motivation and definition:** What problem matters, and what does the key
   term mean?
2. **Gap:** What remains unclear?
3. **Question and method:** What comparison or experiment answers the gap?
4. **Measurement:** What was the primary outcome?
5. **Results:** What happened in the declared scope?
6. **Contribution:** What does the study add?
7. **Boundary:** What should the reader not infer?

Template:

> [Problem] matters because [consequence]. We define [key term] as
> [definition]. It remains unclear whether [gap]. We compare [baseline] with
> [intervention] under [matched controls]. We measure [primary outcome] and
> observe [bounded result]. This contributes [noun-based contribution].
> The result does not establish [important boundary].

Do not introduce a list of models, tools, arms, metrics, and limitations before
the reader understands the question.

## 8. Use the paper as an evidence path

Recommended order:

1. Abstract.
2. Introduction: consequence, known work, gap, question, contribution.
3. Scope and claim boundary.
4. Related work.
5. Method: unit, conditions, controls, outcomes, analysis.
6. Results.
7. Discussion: observed result and scoped inference.
8. Limitations, ethics, and broader impacts.
9. Reproducibility and artifact boundary.
10. Conclusion.

The introduction should answer “why this question?” The method should answer
“why this design?” The results should answer “what happened?” The discussion
should answer “what does it mean within scope?”

Do not introduce a decision in the discussion if the method never defined it.
Do not present a result before defining the outcome. Do not use the conclusion
to make a broader claim than the limitations allow.

## 9. Separate research decisions from engineering decisions

An engineering decision chooses how to build or run the artifact. A research
decision determines what comparison can answer the question.

Examples:

- Engineering: use a JSON parser to validate the returned object.
- Research: use the same local validator in both conditions so the provider
  schema is the isolated intervention.

Record consequential decisions with:

- Decided;
- Instead of;
- Why;
- Costs if wrong;
- Decided by; and
- What the choice does not prove.

This makes the reasoning inspectable and gives a future reviewer a fair chance
to challenge the choice.

## 10. Audit claims before sharing

For every assertive sentence, ask:

- Is this a result from this study?
- If not, is it supported by a citation?
- If neither, should it be a question or be removed?
- Does the sentence name the population and conditions it actually covers?
- Does the result support the strength of the wording?

Then check:

- the question is narrower than the title;
- the primary outcome is singular and defined;
- every contribution is a noun;
- every unfamiliar term is defined before use;
- the method explains the design choices;
- the results use the declared denominators;
- limitations are adjacent to the claims they limit; and
- the artifact boundary is honest.

## 11. Current study example

The [response-quality study](https://github.com/julie-elkins/llm-eval-validity/tree/main/research/response-quality)
uses this sequence:

```text
question -> matched protocol -> frozen conditions -> evidence outcomes
-> human calibration -> scoped result -> limitation -> next test
```

Its [research question record](https://github.com/julie-elkins/llm-eval-validity/blob/main/research/response-quality/RESEARCH_QUESTION.md)
is the canonical explanation of why the primary outcome is
evidence-contract satisfaction and why protocol acceptance is secondary.

The study remains a bounded synthetic pilot. The guide does not turn it into
production evidence or a claim about general model quality.
