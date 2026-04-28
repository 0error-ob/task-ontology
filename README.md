# Task Ontology

### A Substrate for Agent Evaluation

> **Benchmarks do not merely measure tasks. They instantiate a theory of what a task is.**

> *Task definition is not neutral. It encodes the evaluator's theory of intelligence.*

---

## What this is

Task Ontology is a vocabulary for describing how tasks are constructed, projected, and operationalized in agent evaluation.

It provides primitives, dimensions, benchmark re-descriptions, and worked examples for asking: what kind of task has been constructed, what has been made observable, what has been hidden, and what does the scoring interface assume?

It is not an algorithm, framework, SDK, or leaderboard.

---

## Use it

- **Start here:** [QUICKSTART.md](./QUICKSTART.md) — three diagnostic templates you can use in 10 minutes.
- **See examples:** [examples/](./examples/) — benchmark claims, agent regression claims, model migration evals.
- **Read the full argument:** [MANIFESTO.md](./MANIFESTO.md).

---

## Core vocabulary

- [PRIMITIVES.md](./PRIMITIVES.md) — building blocks of a task.
- [DIMENSIONS.md](./DIMENSIONS.md) — properties along which tasks vary.
- [BENCHMARK-MAPS.md](./BENCHMARK-MAPS.md) — existing benchmarks re-described in this vocabulary.
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
