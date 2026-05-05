# Task Ontology

### A Substrate for Agent Evaluation

> **Benchmarks do not merely measure tasks. They instantiate a theory of what a task is.**

> *Task definition is not neutral. It encodes the evaluator's theory of intelligence.*

---

## What this is

A vocabulary for describing how tasks are constructed, projected, and operationalized in agent evaluation. Primitives, dimensions, and benchmark re-descriptions — not algorithms, not a framework, not a leaderboard.

Before we ask whether an agent succeeds, this asks: *what kind of task has been constructed, what has been made observable, what has been hidden, and what does the scoring interface assume?*

---

## What this is not

- **Not** a planning formalism in the MDP / POMDP tradition.
- **Not** a cognitive task analysis taxonomy in the GOMS / KLM / HCI tradition.
- **Not** a specification language for RL environments or agent harnesses.

Those traditions answer *how to solve a task once it has been formalized.* This project asks the prior question: *how is a task formalized in the first place, and what does that choice cost?*

---

## A motivating example: what is a "coding task"?

The phrase *"AI helps engineers write code"* names a real problem. But the moment we try to evaluate it, we have to cut. And how we cut determines almost everything that follows.

The same underlying problem can be cut as:

| Cut | What gets evaluated |
|---|---|
| Coding benchmark | Does the patch pass the test suite? |
| Debugging task | Can the agent localize and explain the root cause? |
| Agent planning task | Does the agent decompose work into sound subgoals? |
| Tool-use task | Does the agent select, sequence, and parameterize tools efficiently? |
| Repo-navigation task | Can the agent build a working map of an unfamiliar codebase? |
| Regression-control task | Does the patch avoid breaking invariants elsewhere? |
| Cost-accuracy optimization | Does it succeed under a token / wallclock / dollar budget? |

Each cut yields a different benchmark, surfaces a different model preference, draws a different evaluation conclusion, and points to a different product direction. **The cut is the theory.** Most of the field debates models. Very little of the field debates the cut.

---

## Three claims

**1. Tasks are constructed, not given.** No agent task arrives free of framing. A modeler chose where to draw the boundary, what counts as input, what the success signal is, what the actor is allowed to observe. Those choices are theory-laden, and they propagate downstream into every metric.

**2. Benchmarks are projections of tasks under chosen interfaces, not measurements of intrinsic difficulty.** A benchmark fixes one cut and one interface. Two benchmarks targeting "the same" capability can diverge on which model wins, because they project the underlying task differently. Treating a leaderboard as a measurement of capability hides the projection.

**3. Agent failures are diagnostic of task structure, not just model capability.** When an agent fails, the failure has a structure: at the oracle, at decomposition, at tool selection, at context reconstruction, at verification. That structure is information about the task as much as about the agent. Pass-rate erases it.

---

## Apply it

For templates, worked examples, and field notes — reading public eval claims through this vocabulary — see [eval-claim](https://github.com/0error-ob/eval-claim).

---

## Core vocabulary

- [PRIMITIVES.md](./PRIMITIVES.md) — building blocks of a task.
- [DIMENSIONS.md](./DIMENSIONS.md) — properties along which tasks vary.
- [BENCHMARK-MAPS.md](./BENCHMARK-MAPS.md) — existing benchmarks re-described in this vocabulary.
- [CASE-STUDIES.md](./CASE-STUDIES.md) — additional worked applications.
- [GLOSSARY.md](./GLOSSARY.md) — one-line term index.
- [MANIFESTO.md](./MANIFESTO.md) — the full argument.
- [CONTRIBUTING.md](./CONTRIBUTING.md) — extension protocol and extractive constraint.

---

## Status

**v0.1.2** — intentionally incomplete. The aim of this release is to lock the vocabulary. Extension is governed by the extractive constraint in [CONTRIBUTING.md](./CONTRIBUTING.md): *every primitive or dimension must come with a concrete signature in benchmark spec, harness config, or agent trace. If you cannot point to where it would be observed, do not add it.*

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
