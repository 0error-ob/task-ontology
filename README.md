# Task Ontology

### Reading AI Evaluation Claims

A vocabulary and template system for analyzing what public AI evaluation claims are actually allowed to support.

The core question: **What does this score actually license?**

---

## What this is

Most AI evaluation claims travel further than their artifacts warrant. A benchmark score is a property of a *(model, harness, prompt, oracle, sample distribution)* joint object — not a property of the model alone. A regression alert is a metric shift — not yet an attribution. A migration recommendation is an aggregate comparison — not a per-workflow guarantee.

This repo provides the vocabulary and templates for reading claims precisely: what task structure was actually measured, what the oracle could and could not see, which dimensions dominated the result, and where the inference breaks down.

It is not an algorithm, framework, SDK, or leaderboard.

---

## Use cases

Use this repo when you need to:

- turn an eval result into a defensible public claim
- explain what a benchmark score does and does not show
- compare two model results without overclaiming
- write a model migration note for a specific workflow
- review a leaderboard claim before citing it
- prepare a release note, blog post, or README section involving eval results

---

## Templates

- [templates/claim-reading.md](./templates/claim-reading.md) — analyze a single public eval claim across seven fields.
- [templates/migration-note.md](./templates/migration-note.md) — write a defensible model migration recommendation in cohort terms.
- [QUICKSTART.md](./QUICKSTART.md) — three longer diagnostic templates: benchmark, agent regression, model migration.

---

## Examples

- [examples/](./examples/) — worked applications: benchmark leaderboard claim, agent regression, model migration eval.

---

## Field notes

- [field-notes/](./field-notes/) — public eval claims read through these templates as they surface.

---

## Vocabulary

The vocabulary the templates use:

- [MANIFESTO.md](./MANIFESTO.md) — the full argument.
- [PRIMITIVES.md](./PRIMITIVES.md) — building blocks of a task.
- [DIMENSIONS.md](./DIMENSIONS.md) — properties along which tasks vary.
- [BENCHMARK-MAPS.md](./BENCHMARK-MAPS.md) — existing benchmarks re-described.
- [CASE-STUDIES.md](./CASE-STUDIES.md) — additional worked applications.
- [GLOSSARY.md](./GLOSSARY.md) — one-line term index.
- [CONTRIBUTING.md](./CONTRIBUTING.md) — extension protocol and extractive constraint.

---

## Status

**v0.1.2** — intentionally incomplete. This release adds a first-use path: QUICKSTART + three worked examples.

> **Without a task ontology, agent evaluation collapses into leaderboard engineering.**

---

## Cite

If you use this vocabulary in published work:

```bibtex
@software{chen_2026_task_ontology,
  author  = {Caesar Chen},
  title   = {{Task Ontology: A Substrate for Agent Evaluation}},
  year    = {2026},
  version = {0.1.2},
  doi     = {10.5281/zenodo.19839872},
  url     = {https://github.com/0error-ob/task-ontology}
}
```

Machine-readable metadata: [CITATION.cff](./CITATION.cff) · DOI: [10.5281/zenodo.19839872](https://doi.org/10.5281/zenodo.19839872)
