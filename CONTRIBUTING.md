# Contributing

This repo is a vocabulary, not a framework. Contributions extend the vocabulary; they do not extend the codebase. Read this document before opening a PR.

---

## What belongs in this repo

**In scope.**

- A new **primitive** — a building block of task description that is not already covered.
- A new **dimension** — a property along which tasks vary that is not already covered.
- A **benchmark map** — re-describing an existing benchmark in the vocabulary.
- A **case study** — a worked example of the ontology applied to a real phenomenon.
- A **clarification** to an existing entry — a sharper definition, a missing signature, a clearer example.
- A **glossary refresh** — keeping `GLOSSARY.md` consistent with edits to source files.

**Out of scope.**

- Algorithms, harnesses, evaluation runners.
- New benchmarks. (Existing benchmarks are mapped here; new ones are built elsewhere.)
- Critiques of specific models or vendors.
- Philosophical commentary unattached to a primitive, dimension, or measurement.
- Domain-specific ontologies (medical-task ontology, legal-task ontology, etc.) — these may live in forks; this repo stays domain-agnostic.

---

## The extractive constraint

**Every primitive or dimension added to this repo must come with a concrete signature: a description of how it would be observed in benchmark spec, harness config, or agent trace.**

This is the load-bearing rule. It is what keeps the ontology from drifting into philosophy.

If you propose a new primitive or dimension and cannot answer "*where would I see this in a real artifact?*" with a specific pointer — to a config file, a log line, a metric, a dataset field — the proposal is not yet ready. Sharpen the signature first; submit second.

A practical test: a primitive is ready if a moderately careful reader could go to three different real benchmarks or agent traces and identify the primitive in each. If they cannot, the proposal needs to either (a) sharpen the signature, or (b) be merged into an existing primitive that already passes this test.

The same test applies to dimensions: a dimension is ready when at least three real task settings can be unambiguously placed on the dimension's scale.

This constraint replaces the more common "is this concept correct?" test with "is this concept *operational*?" That is intentional. The ontology earns its right to exist by what it lets you describe — not by what it lets you say.

---

## How to add a primitive

A primitive PR adds an entry to `PRIMITIVES.md`. The entry must have:

1. **Definition** — one to two sentences naming the entity precisely.
2. **Signature** — where you would observe this primitive in benchmark spec, harness config, or agent trace. Be specific: a config field, a log line, an API parameter, a state variable.
3. **Examples** — at least three worked examples in distinct task contexts. The contexts must be genuinely different (e.g., coding / dialog / web; or sandbox / production / multi-agent). Three examples in the same context do not count.
4. *(Optional)* **Composition note** — how this primitive interacts with the others in non-obvious ways.

Update `GLOSSARY.md` in the same PR.

---

## How to add a dimension

A dimension PR adds an entry to `DIMENSIONS.md`. The entry must have:

1. **Definition** — one to two sentences naming the dimension precisely.
2. **Scale** — the values the dimension can take. Categorical (e.g., *static / actor-driven / exogenous*) or ordinal (e.g., *low / medium / high*). Continuous is acceptable but rare.
3. **Instrumentation** — how the dimension is measured from a real artifact. Pointer to a metric, a count, a ratio, or a protocol — not a vague "the evaluator decides."
4. **Examples** — at least three worked examples placing distinct task settings on the scale.
5. *(Optional)* **Interaction note** — which other dimensions covary with this one in practice.

Update `GLOSSARY.md` in the same PR.

---

## How to add a benchmark map

A benchmark map PR adds an entry to `BENCHMARK-MAPS.md`. The entry must have:

1. **One-paragraph description** of the benchmark — what it nominally measures, how scoring works.
2. **Primitives table** — fill in each of the 14 primitives for this benchmark. If a primitive is "not applicable" or "trivial," say so explicitly; do not omit.
3. **Dimensions table** — place the benchmark on each of the 13 dimensions. If a dimension's value is variable across the benchmark's tasks, give the modal position and note the variance.
4. **"What this re-description makes visible" paragraph** — one paragraph naming what the leaderboard view of this benchmark hides that the ontology view surfaces. Be specific; do not generalize.

The aim is descriptive, not adversarial. A good benchmark map is a translation, not a critique.

---

## How to add a case study

A case study PR adds an entry to `CASE-STUDIES.md`. The entry must have:

1. **Phenomenon** — a one-paragraph description of the real-world observation being explained.
2. **Mapping** — how the phenomenon maps onto specific primitives and dimensions. Cite the entries by name and link.
3. **Reframe** — what the ontology lets you see that the original framing missed.
4. **Operational implication** — what an evaluator, harness builder, or product owner would do differently after reading this.

Case studies should be short. Two pages of prose with one good table is better than ten pages of speculation.

---

## Style

- English only in repo files. (Discussion threads in any language.)
- One sentence per line in markdown sources is fine; renders the same in HTML.
- Avoid hedging ("perhaps," "may suggest," "could potentially"). Make claims, defend them in the signature, withdraw them on counter-evidence.
- Do not use this repo to settle scores with specific vendors, models, or labs.
- No emoji unless they are the load-bearing example of a primitive.

---

## Process

1. Open an issue first if the proposal is non-trivial — a new primitive, a new dimension, a substantive case study. Brief alignment in the issue saves a round of PR review.
2. Submit one concept per PR. Multi-concept PRs will be split.
3. PRs that lack a signature, lack three cross-context examples, or do not update `GLOSSARY.md` will be marked *needs revision* and not merged until the gaps are closed.
4. License of contributions: by opening a PR, you agree your contribution is licensed under the repo's MIT license.

---

## Why these rules

The ontology is only as strong as its descriptive grip on real artifacts. Every contribution that loosens that grip — a primitive without a signature, a dimension without a scale, a case study without an operational implication — makes the next contribution easier to loosen further. The rules above are conservative on purpose. The cost of an extra revision round is trivial; the cost of a vocabulary that drifts is cumulative.
